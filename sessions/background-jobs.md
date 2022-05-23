# Growing Your Background Job Knowledge

Jake Anderson
Sponsored talk by Weedmaps

## Intro

- the lifecycle of a request:
  - client --> router --> controller --> model --> database
  - sometimes, to add a feature or some logic, you'll add a **service** as a substep of the controller step.
    - what if the service is taking too long?
      - decide whether you really need to wait on the service call to resolve
        - if yes, attempt to make it faster or re-architect the system so the code no longer has to wait on this service
        - if no, move it to a background job
- anatomy of a background job:
  - a service call gets added to a list (queue) with its associated args
  - the queue is stored somewhere
  - as new items come in, 1 or more runners shift items off the queue and process them
- how does pushing things to a queue help?
  - prevents too many things running at the same time
  - since the queue is stored somewhere, we can inspect it. We can see:
    - jobs currently running
    - jobs waiting to run
    - successful runs
    - failed runs + ability to retry
  - stability and visibility

## ActiveJob: Rails' Queueing System

- out-of-the-box support for async processing
- 3 basic steps:
  - create the job class
  - tell the job class to add itself to the q
  - Rails takes care of the rest
- the job class:
  - Inherits from `ApplicationJob` or directly from `ActiveJob::Base`
  - has a `perform` method
  - may specify which q to run on
- adding the job to the q
  - call `perform_later` with args
  - be aware the args will be serialized by Rails, then deserialized when the job runs
  - enqueuing options:
    - `set`: a method on the job class for when you don't wan to run it right away; possible args:
      - `wait`
      - `wait_until`
      ```ruby
      SomeJob.set(
        queue: :urgent,
        wait_until: Time.current.midnight
      ).perform_later
      ```
- how does Rails actually run these jobs?
  - it depends... BYOB (backend)
  - by default, Rails stores these jobs in memory and processes them off your RAM
    - the downside: the q flushes/breaks down when the server restarts or crashes
      - e.g.: Heroku servers restart their dynos every 24 hours

## Additional features of ActiveJob

- available backends for ActiveJob:
  - Rails has a whole list of out of the box solutions available, including database storage (`delayed_job`), in-memory (ActiveJob default (async), `sucker_punch`), and Redis (sidekiq)
  - you can also find ways to support other backends, such as Kafka
- why note use the backend library directly, pushing a bit of logic direclty onto a sidekiq q for example? why use an ActiveJob class?
  - Global IDs (`gid`) that directly identify a model (an ID unique to each record across all tables; ie, no record on User or Account will share a GID in common)
  - custom serializers
  - automatic localization (il8n)
  - callbacks: before, after, and around hooks for both enqueuing and performing the job
  - error handling
  - main benefit: a unified API for interacting with jobs regardless of the backend
