# Exercise 3

## Concept Questions

1. Within a context, strings are unique and not reused. But between contexts, strings may be reused. Contexts essentially split the strings into groups where they are guaranteed to be unique and not reused. In the URL shortening app, each shortUrlBase can have its own context, so the shortUrlSuffix do not repeat and each shortUrlBase and shortUrlSuffix combination is unique. Between different shortUrlBases, the shortUrlSuffix can repeat as the shortUrlBase is already unique and the shortUrlBase and shortUrlSuffix combination will be unique.

2. (a) *NonceGenerating* stores sets of used strings because new strings must be unique and not already used. This way, when new strings are generated, they can be checked against the used strings to check that they are unique and not already used. Without the sets of used strings, the specification can no longer know or guarantee that new strings are unique. (b) For each context, the number of used strings in the set is equal to the counter.

3. (a) One advantage is that it is easier to remember the shortened URLs, as it is just the website name and a common dictionary word. One disadvantage is that the space of shortened URLs is significantly smaller, potentially leading to the words running out. Additionally, as common dictionary words, the shortened URLs would be more guessable and the user could have unwanted others on their private website. (b) In **purpose** change "unique strings" to "unique, memorable common dictionary words" as well. Then in state and actions, change the type "Strings" to "CommonDictWords" (adding this type next to [Context] at the top). In state, add "a set of CommonDictWords." In the register action's **where**, add "shortUrlBase" is in "CommonDictWords."

## Reaction Questions

1. The first reaction is essentially to generate the shortUrlSuffix. The generation only needs the context via the shortUrlBase, not the targetUrl, so the targetUrl argument is not included. On the contrary, the second reaction is essentially to register the shortUrlBase with the shortUrlSuffix. The registration needs the shortUrlBase as well as the targetUrl, so the targetUrl argument is included. Essentially, only the arguments that are needed by the **then** action are included.

2. Although the specification is more succinct, sometimes the arguments and results of the concept are more general than the arguments and results of the reaction. This maintains concept independence.

3. Setting the expiry only requires the shortUrl, which is given in register (and seconds, which is preset). Additionally including request would needlessly complicate the third reaction. On the contrary, the second reaction needed nonce and targetUrl, which was given separately in Requesting.shortenUrl and NonceGenerating.generate, and the first reaction occurs right after request and before register, so it needs the data from request and does not have access to the data from register.

4. In the Requesting.shortenUrl of the **where** of the generate and register reactions, remove the shortUrlBase argument as they are the fixed base. In the NonceGenerating.generate and UrlShortening.register of the **then** of the generate and register reactions, set the context and shortUrlBase to the fixed base. The concepts do not need to change 

5.
**reaction** expired

**when** Expiring.expireResource () : (resource: shortUrl)

**then** UrlShortening.delete(shortUrl)

## Extending the design

1.

**concept** Counting [Action]

**purpose** tracks the number of times that an action has happened

**principle** after initializing a counter and then incrementing n times, getting the count returns n

**state**
a set of Counters with
- an item Item
- a count Number

**actions**

initialize (item):
- **where** no Counters exists with the item
- **then** create a new Counter with the item and the count set to 0

increment (item):
- **where** the item exists in one of the Items
- **then** increment the count by 1

getCount (item): (count: Number)
- **where** the item exists in the set of Items
- **then** return the count of the item

----

**concept** Ownership [User, Resource]

**purpose** provide access to a Resource only to the User who owns it

**principle** after registering a User and an Resource, the User checking the resource succeeds and others fail

**state**
a set of KeyVals with
- a User
- a set of Resources

**actions**

register (user, resource):
- **where** resource is not already in a set of Resources of a KeyVal
- **then** if user exists in a KeyVal, add resource to the set of resources; otherwise
create a new KeyVal with the user and resource

checkOwner (user, resource):
- **where** a KeyVal exists with the user, and the resource is in the KeyVal
- **then** success

2.

**reaction** startCounting

**when**

Requesting.shortenUrl(user)

UrlShortening.register(): (shortUrl)

**then**

Counting.initialize(item: shortUrl)

Ownership.register(user, resource: item)

----

**reaction** addCount

**when** UrlShortening.lookup (shortUrl)

**then** Counting.increment(item: shortUrl)

----

**reaction** checkCount

**when** where Requesting.getAnalytics (user, shortUrl)

**when** Ownership.checkOwner(user, resource: shortUrl)

**then** Counting.getCount(item: shortUrl)

3.

To allow users to choose their own short URLs, add a new request and reaction with an argument for a shortUrlSuffix. The reaction should register the shortUrlSuffix, and go into the used set of Strings of the appropriate context NonceGenerating to mark the shortUrlSuffix as used. Add an additional reaction analogous to startCounting to ensure that the User can track analytics similarly.

To use the "word as nonce" strategy to generate more memorable short URLs, add a set of words to the **state** of NonceGenerating. In the **then** of NonceGenerating, add that the nonce returned is from the set of words.

To include the target URL in analytics, include an id string and set of items. Then initializing should add the items: shortUrl to the id: targetUrl. Incrementing and getCount should find either the id or item and increment or return that count.

To generate short URLs that are not easily guessed, in the **then** of NonceGenerating, add that the nonce returned is a long, random string. (The same procedure but different rules as the "word as nonce" strategy.)

This feature is undesirable and should not be included because there is no way to check that the users requesting the analytics actually own the website of the target URL, and the analytics of the target URL may be leaked to non-owners. Nonetheless, this can be done by adding a reaction that returns a secret link to the analytics. Then the secret link rather than a specific user can be used to allow access into the analytics.