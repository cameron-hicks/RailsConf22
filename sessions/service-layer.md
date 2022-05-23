# Your service layer needn't be fancy, it just needs to exist

David Copeland (Mood Health)

- where does the business logic in an app go?
  - business logic is unknowable in advance and changes frequently. If you spread it across many directories and parts of the MVC, it becomes hard to manage.
  - typically, the convention is to put it in the model.
    - but the rails docs don't cover this aspect of models in much depth. they focus much more on the database stuff.
  - we need to manage business logic as its own architectural component: MBVC.
    - http requests --> controller --> business logic layer --> model
- service layer = business logic
  - so, a "service" is any bit of code that does business logic
- what does it mean for a service layer to be unfancy?
- we put these services in `app/services`
- Q: what are the differences in responsibility btw services and jobs? differences in implementation?
- objections:
  - validations are business logic! and those live on the model. --> a principle wiht a caviat: "No business logic in active records (except validations)."
  - objections against code that's ugly --> but it's a useless exercise to argue and gatekeep about what code is beautiful
    - it's okay if your service code is not object-oriented or someone else calls it "procedural"
  - bUt DoEs It ScAlE?? --> yes
    - over time, you'll have lots of services and they will be necessarily complex, sometimes not clear; but it will be easier to see your app for what it really does instead of what you intended for it to do b/c the logic is organized nicely
    - you have the opportunity to namespace new and related concepts as you discover them
- he has a book: sustainable web development david bryant copeland
