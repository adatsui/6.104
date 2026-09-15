Personal Goals

I would like to take this class to learn how to design software, and incorporate LLMs into both my process and product as well as various other tools into my tech stack. Additionally, I would like to learn about concept design, and the most recent techniques and technologies on designing software. Ultimately, I hope to gain two completed projects and the abilities to build them from the ground up.

To make this an enriching and enjoyable experience, I plan to use the time and stress management skills that I have learned throughout my time at MIT. Specifically, I plan to start assignments early, progress consistently, and ask questions as they come up. I also plan to attend lectures and recitations, and critically consider my engagement with LLMs to ensure that they are enhancing, not replacing my learning and skill-acquirement.

I am definitely concerned that completing two projects in one semester and with my other commitments is difficult, but I trust that the projects have been scaffolded and the assignments have been deliberately planned to mitigate this. I plan to respect the deadlines, prioritize the projects, and practice my time management skills to ensure that I am not bogged down in the projects at the end of the semester.

Overall, this class fits into my larger goals in life. I would like to be a SWE, and I think the projects would provide the necessary experience. Additionally, I would like to learn how to design software and think about the why, not just the how, behind software engineering, and I think the lectures in this class would provide an opportunity to do so.

In my education to date, I have taken 6.102, but the projects were largely scaffolded and unoriginal. This class would provide original projects to build from the ground up. Additionally, 6.102 largely focused on line- and function-level code, whereas this class would focus on larger amounts of code and less minutiae.

Problem Statement

Domain. Book readers. Oftentimes, before or during reading a book, a reader consults a secondary source to prime or summarize what they are about to or what they just read. These can be people who read for pleasure or for academic purposes.

Bad situations. Oftentimes, official introductions (written by scholars) and summaries (on websites such as SparkNotes, LitCharts, etc.) spoil the plot with phrases such as “this foreshadows…” or “this book is about…” Still, the reader may want to read an introduction to prime themselves for what they are about to read (detailing historical context, author biography, important information from past installments, etc.), or a summary to review what they just read (including important plot points, character developments, historical references, etc.).

Corroboration. LitCharts and SparkNotes, adjacent websites, receive more than 10 million views per year (about 5-7 and 4 million, respectively). Furthermore, many people buy classic books from reputable publishers for their introductions, as otherwise they could buy the classic books from cheaper publishers with the same text. Clearly, readers value well-written introductions and summaries.

Workarounds and comparables. Some people avoid reading introductions, summaries, or specific analysis sections to avoid spoilers. Other people try to read carefully, stopping before they sense that they face a spoiler. Some websites support spoiler tags (like Goodreads) or explicitly state the number of chapters that can be spoiled (Reddit book clubs), but these often rely on user discretion.

Solution sketch. For introductions, pull keywords from the book and generate a proper introduction via LLM. By not inputting explicit plot details, the LLM avoids accidentally revealing them. For summaries, only feed the parts of the book that the reader has read (for example, for a chapter 3 summary, only include chapters 1-3). This way, the LLM has the same information as the reader and avoids accidentally revealing them.

This could lead to challenges with the accuracy as the LLM is operating on limited information, and some of the benefits of introductions and summaries is that the author has read the entire text and knows what is important to highlight and comes up later, and what is not. Other general challenges with LLMs like bias exist. Additionally, obtaining the rights to copyrighted books may be difficult.
