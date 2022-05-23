# Leveling Up: From Planning to Production

Thomas Countz from Zendesk
About skills for becoming a senior engineer

Four techniques:

- **ask first; solve later**
  - start by breaking down the questions, not jumping to solutions
  - goal is to eliminate "hidden" work
  - given a task to complete:
    - Ask for and store users' first and last names separately on the signup form.
  - start by asking questions and noting their urgency:
    - How do new users sign up? --> important
      - e.g., you might find you have two versions of the same signup form
    - How do we store their information? --> important
      - e.g., you might find that there's already a first_name and last_name column on Users, but that we use it inconsistently: Sometimes the full name is stored in first_name while last_name is nil.
    - What about multiple given names? --> can wait
    - Didn't so-and-so work on something like this last quarter? --> not necessarily relevant
- **share your findings as you discover them**
  - goal is to enable coworkers to share helpful clues/knowledge they have inside their heads
  - don't let imposter syndrome prevent you from sharing your in-progress questions
  - possible routes for sharing:
    - slack threads
    - standalone github notes
- **make code executable for better feedback**
  - define some duck types (Ruby objects that have the behavior of the object you intend to work on)
    - eg, you'd define a User class that implements the relevant methods for this task: first_name and last_name getters plus a `split` method that separates full names saved as a single string into two strings
  - can configure the file to work with sqlite instead of the full database (will use your coworkers' local disk for storage)
  - the goal is to ask your coworkers to hold less in their heads
- **prepare for post-deployment**
  - make a note for yourself, eg on the pr, the behavior the changes should have in the browser and on the database; include metrics and perhaps a summary of the errors you hope _not_ to see
