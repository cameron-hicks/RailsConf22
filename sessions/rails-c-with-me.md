# `rails c` with me: Turbocharge your use of the interactive console

**Sweta Sanghavi**

## intro

Why the console is awesome:

- can evaluate a piece of code in context
- little setup needed; works out of the box
- that context is configurable
- quick feedback

the irb (ruby) console gets you:

- core ruby libraries
- can import more libraries

calling `bundle console` gets you:

- core ruby libraries
- all the gems in your Gemfile

calling `rails console` gets you:

- core ruby libraries
- all the gems in your Gemfile
- your rails environment

## highlighted irb features

- uses top-level Subessions to save and differentiate context
  - call `irb` within an `irb` console
  - looks like `irb#1` vs `irb#2`: two Threads
    - use `fg 0` to switch back to `pry1` Thread
  - these threads have separate execution contexts (and separate variables) but can share an overall execution context called the Workspace via the `Binding` object.
    - can pass a `Binding` around to share context
  - abstract syntax trees
    - parsers process code into an abstract syntax tree, which is a low-level (compiled) tree represntation of the program's mechanics
    - Ripper's `sexp` method outputs the tree so you can see it. Give it a whole function definition for its arg
    ```sh
    irb#1(main)> Ripper.sexp("def full_name(first_name, last-name) first_name + '' + last_name end")
    => [:program,
      [[:def,
      #etc
      ]]]
    ```
- interactivity
  - powered by library GNU Readline (1989): provides in-line editing and history capabilities
    - used across shells
    - the Readline line editor was abstracted from GNU; is a C library that ships in standard Ruby and provides an interface for GNU Readline

## superpowers of `rails c`

- improving performance
  - breadcrumbs are helpful b/c rails semantic scopes can hide complexity
  - use Ruby's Benchmark module to measure the runtime to execute code
  ```sh
  irb> Benchmark.measure {|x| x.do_something }
  ```
- call `to_sql` to output the SQL underlying your ActiveRecord query
- call `explain` on an ActiveRecord query; will run the query with logging _and_ output the query plan (= what scans are necessary to complete the query; useful to decide if an index would help you)
- using the console in production
  - pass `--sandbox` when opening the console. Any changes you make in the prod console will be automatically rolled back on exit

## informing product decisions & impact with simple queries

- it's great to be able to answer data questions quickly to inform product decisions!
  - e.g.: "Based on the size of this cohort, would a manual fix be better?"

## misc

- `cmd + k`: clears the console
- `reload!`: reload the code after you've made changes to its implementation. It doesn't auto-reload. Note you'll need to reinstantiate objects you declared in the context pre-reloading
- `_`: get the value of the last evaluated item, even if you didn't save the return value to a varaible
- `ctrl + r`: reverse command search. pulls up previous things you've executed in the console
