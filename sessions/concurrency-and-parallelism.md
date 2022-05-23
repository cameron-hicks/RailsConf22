# Hold My Thread: Concurrency in Ruby

Clara Morgeneyer (Apple)

- paralellism != concurrency
  - paralellism = actually doing multiple things at once. You do the subtasks in parallel
  - concurrency = you split up the tasks into subtasks and take turns
  - eg:
    - You have to feed four dogs as fast as possible. In parallelism, you provide four bowls, one to each dog simultaneously. In concurrency, you provide one bowl and allow each dog to take one bite at a time. While the dog chews, it goes to the back of the line to allow the next dog to take a bite.
    - what is the "chewing" a metaphor for? **blocking i/o**
      - waiting for something to return, eg a response from an http request, a db write, or a read/write to/from file
  - parallel execution is faster than concurrent execution
- defining "process" and "thread"
  - these are operating system concepts
  - process = a program in execution
  - thread = a unit of execution within a process
    - a process has one or many threads
    - you have a main thread that is always running; if it's killed, all the others will be killed as well
- how ruby implements paralellism and concurrency

  - processes: create a new process with the method `fork`
    - they run in parallel and consume a lot of memory
    ```ruby
    100.times do |i|
      fork do
        fib(30) # fibonnaci sequence
      end
    end
    ```
    - this snippet runs the fibonnaci sequence 100 times in parallel executions
  - threads: create a new thread with `Thread.new` (pass it a block) and `t.join`

    ```ruby
    t = Thread.new do
      fib(30)
    end

    t.join
    ```

    - the execution context/`main` is the main thread. calling `t.join` means you'll wait for this thread to finish before finishing and killing the main thread.
    - benchmarks illustrating that using threads didn't produce performance improvements
    - threads run concurrently, not in parallel
      - because of teh GIL/GVL: global interpreter lock/global v\_\_ lock
        - prevents threads from running in parallel
    - why use threads then? for programs with a lot of blocking i/o

  - race conditions (= when multiple threads have side effects on the same object in their shared context)

    ```ruby
    sum = 0

    100.times do
      Thread.new do
        sum + 1
        puts sum # will not be what you expect
      end
    end

    ```

  - solution to race conditions: mutexes
    - first thread to enter the mutex locks it and no other thread can enter that section of code until this thread exits the mutex. One mutex shared by all threads.
      - you sacrifice performance by doing this
  - fibers:

    ```ruby
    f = Fiber.new do
      puts 1
      Fiber.yield  # means "stop"
      puts 2
    end

    f.resume # starts the fiber --> prints 1
    f.resume # allows the fiber to continue from the "yield" --> prints 2
    ```

  - ractors: an experimental Ruby feature

- Puma: a Rails gem for web servers. Uses threads to run concurrently
  - sidekiq is another edample of a concurrent ruby gem
  - servers handle each incoming http request in parallel, giving each request its own controller instance
    - but they share classes etc. from their comon context
    - rails has some protection against race conditions built-in
