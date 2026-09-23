# Exercise 1

## Part 1

The invariant about aggregation/counts of items is that for each request, the count is greater than or equal to the sum of the counts of the set of purchases.

The invariant relating requests and purchases is that for each request, the item matches the items of the set of purchases.

The first invariant is more important because it ensures that extra items that are not requested, are not purchases.

The action whose design is most affected by it is purchase, as it calculates the difference between the request's count and the purchases' count sum in order to determine if it is a valid purchase. It preserves this invariant by calculating and the additional invariant in addItem that there are no duplicate requests with the same item (ensuring that requests and purchases are consolidated).

## Part 2

The action removeItem may fail to preserve the first invariant. For example, an request for an item can be added, purchases for that item made, and then the request for the item removed and deleted. Then the purchases are still stored, and the sum of the counts of the set of purchases is greater than 0, while the effective count of the items is 0.

This can be fixed by not letting items with purchases be removed, or keeping the items and changing the count to be equal to the sum of the counts of the set of purchases, so no additional purchases will go through.

## Part 3

The registry can be closed and opened repeatedly as closing and opening do not specify a limited number of times and only mutate the active flag. The history of the registry is maintained and recoverable.

This is likely allowed so the registry can be reopened easily, for example if someone forgot to purchase a gift, and the registry can be reused, for example for a wedding after a baby shower, as the person likely would like the same thing.

## Part 4

Some reasons to add one is that registries may be opened mistakingly, or a registry opened intentionally may be doxxed and bombed with unwanted purchases from unwanted people. Additionally, the registry owner may not want to see the registry anyway or the registry may have served its purpose. In all of these cases, space to store data, which can be expensive, can be freed up.

Some reasons to not add one is that deleting a registry could delete the history. For example, now the registry owner cannot access the purchases in the registry, to see what has been bought. Additionally, the registry owner cannot easily reopen or reuse the registry.

## Part 5

The common query executed by a registry owner is to see all of the purchases in the registry, to see what has been bought.

The common query executed by a gift giver is to see all of the requests where the given count is greater than the sum of the counts of purchases for that request (the items and the difference in the given count and the sum of the counts of purchases for that request). That is, to see what has not been bought so far.

## Part 6

To state, add a hide Flag.

To create, automatically set hide to False.

Add actions to change hide:

hide (registry: Registry)
**where** registry exists and hide is False
**then** set hide to True

show (registry: Registry)
**where** registry exists and hide is True
**then** set hide to False

When the registry owner executes a query to see all of the purchases in the registry, check that hide is False before returning the result of the query.

## Part 7

SKU codes are preferable to names, descriptions, and prices, SKU codes are guaranteed to be the same for the same Items, and different for different items. This helps guarantee the first invariant. Names may be the same for different items (two different sized pan could both be labeled "medium pan"), and descriptions or prices can be different for the same items (a discount or sale could decrease the price).

# Exercise 2

## Part 1

a set of Users with
- a username String
- a password String

## Part 2

register (username: String, password: String) : return (user: User)
- **where** no User has the same username
- **then** create a User with this username and password, and return the new User

authenticate (username: String, password: String) : return (user: User)
- **where** a User with this username and password exists
- **then** return the User found

## Part 3

The essential invariant to maintain is that the usernames must be unique. This way, users are identifiable and cannot masquerade as different users. This is preserved in the *where* of register because the *where* conditions that no other User has the same username. The authenticate function does not mutate the state, so the authenticate function automatically maintains this invariant.

## Part 4

a set of Users with
- a username String
- a password String
- an email Email
- an confirmed Flag
- a secret Token

register (username: String, password: Password, email: Email) : return (user: User)
- **where** no other User has the same username
- **then** create a new User with this username, password, email, and the confirmed set to False; send an email to username requesting confirmation; return the User

confirm (username: String, token: String) : return (user: User)
- **where** a User with this username exists with this token and is not confirmed
- **then** set the user's confirmed to True

authenticate (username: String, password: String) : return (user: User)
- **where** a User with this username and password exist and is confirmed
- **then** return the User found

# Exercise 3

# Concept Specification

**concept** PersonalAccessTokening [User, Scope]

**purpose** let Users generate, authenticate, and revoke to set of Scopes with individual tokens and potentially pre-set expiration dates; do not force User to share or use one primary password

**principle** a User registers an account;
generates individual tokens to sets of Scopes with optional expiration;
authenticates to the individual Scopes with individual tokens

**state**

a set of Users with
- a username String
- a set of Personal Access Tokens

a set of Personal Access Tokens with
- a set of Scopes
- a token String
- an optional expiration Time

**actions**

register (username: String) : return (pat: PersonalAccessTokening)
- **where** no other User has the same username
- **then** create a User with this username, and return the new User

generate (user: User, scopes: set of Scopes, expiration?: Time) : return (token: String)
- **where** a User with this username exists, a Personal Access Token of this User and the scopes do not exist (or do exist and is expired), scopes is nonempty, and the expiration is after the current time
- **then** randomly generate a token, create a new Personal Access Token with this scope, token, and expiration, and return the token

authenticate (scope: Scope, token: String) : return (user: User)
- **where** a Personal Access Token with this scope in the set of Scopes, and token exist and is not expired (or does not have an expiration)
- **then** return the Personal Access Token's User

revoke (user: User, token: String) : return (user: User)
- **where** a Personal Access Token with this user and token exist
- **then** remove and delete the Personal Access Token

# Difference from PasswordAuthenticating

For PasswordAuthenticating, each user has exactly one password, and the password is user-generated and does not expire. The username and password grants broad access, and both are required for authentication.

For PersonalAccessTokening, each user can have multiple tokens for multiple scopes or sets thereof, and the tokens are randomly-generated and can be set to expire. Then the token grants access only to what is in the scope, and as the tokens are randomly generated and unique, only the token is required for authentication.

This way, the impact of a token is minimized to its scope rather than the entire account, and a token can be set to expire, increasing the overall security.

# Change to GitHub documentation

To improve the GitHub documentation, I would explicitly explain the difference from PasswordAuthenticating, including examples of specific scopes that a token can be tailored too. Additionally, I would mention the improved security of a PersonalAccessTokening and the effects of a leaked token versus password.

# Exercise 4

## Part 1

**concept** URLShortening [URL]

**purpose** use shortened URLs to represent longer URLs for convenience and easy access

**principle** generate suffixes, either via a user-inputted suffix or randomly; looking up the shortened URL with the suffix gets the real URL

**state**

a set of Shortcuts with
- a real URL
- a suffix String

**actions**

generate (real: URL, userSuffix?: String) : return (shortened: String)
- **where** real goes to an actual website and if there is userSuffix, then it is different from the other suffixes
- **then** if there is not a userSuffix, then generate a suffix that is different from the other suffixes; create a Shortcut with the real URL and suffix String; return the suffix

get (suffix: String) : return (real: URL)
- **where** a Shortcut with this suffix exists
- **then** return the associated real URL

**Notes**

This design assumes that multiple different suffixes can point to the same URL.

## Part 2

**concept** ShoppingCart [User, Item]

**purpose** track the items that a user would like to buy

**principle** the user creates a cart, and adds and removes items to it; the cart of a particular user can be requested and read

**state**

a set of Carts with
- an User
- a set of Purchases

a set of Purchases with
- an Item
- a count Number

**actions**

create (user: User) : return (cart: Cart)
- **then** create a new cart with this owner and an empty set of purchases, and return the cart

addItem (cart: Cart, item: Item)
- **where** the cart exists
- **then** if the item exists in the cart's purchases, increase the count with the item by one; otherwise create a new purchase with the item and count one

removeItem (cart: Cart, item: Item)
- **where** the cart exists and the item exists in the cart's purchases
- **then** decrease the count with the item by one; if the count is now zero, delete the item and count

get (user: User) : return (cart: Cart)
- **where** the cart exists
- **then** return the purchases in the user's cart

**Notes**

This design lets anyone access any user's cart. Realistically, get should have some authentication to ensure that only the user and system admins can access a particular user's cart.

## Part 3

**concept** TOTP [User]

**purpose** let users authenticate by proving that they have access to a device

**principle** an encoded code is given to the user upon registration; the user enters the encoded code into the device; to authenticate, the user enters the generated code from the encrypted code and time

**state**

a set of TOTPs with
- an User
- a token String

**actions**

register (user: User) : return (encodedCode: String)
- **then** generate a token String; create a new TOTP with this user and the token; encrypt the token into a code to be entered into the device, and return the encrypted qr code

authenticate (user: User, time: Time, code: String) : return (user: User)
- **where** the user exists and the given code is the same as the correct code with the given time and the user's token
- **then** return the User

**Notes**

This design assumes that the user enters the encodedCode into the device. To be more safe, we can include actions to check that the user indeed does this or regenerate encodedCodes in case the user does not.