# Tuesday morning keynote: Xavier Noria

## Member of Rails core team

- about how he refactored the rails autoloader for the `zeitwerk` gem
  - included in Rails 6 as an option; in Rails 7, it will be the only autoloader and classic will not be included
  - implemented it as a second, parallel method that you can opt out of and continue using the classic autoloader if you want
  - why? to enable reloading
- the repo: [https://github.com/fxn/zeitwerk](https://github.com/fxn/zeitwerk)
  - "Zeitwerk is able to load your project's classes and modules on demand (autoloading), or upfront (eager loading). You don't need to write require calls for your own files, rather, you can streamline your programming knowing that your classes and modules are available everywhere. This feature is efficient, thread-safe, and matches Ruby's semantics for constants."
- [more docs explaining zeitwerk](https://guides.rubyonrails.org/classic_to_zeitwerk_howto.html)
- the existing autoloader is implemented in ActiveSupport
- you can override the default implementations of these lookups
- classic autoloader:
  - steps:
    - file name conventions
    - autoload_paaths
    - Module#const_missing
    - User -> user.rb
  - limitations:
    - the nesting is unknown
    - relative vs. qualified is unknown
    - Module#const_missing is the last step
      - meaning it's sometimes never called if you don't hit the fallback, just resolving the constant instead
- `autoload :User, 'user'`
  - this method call reads as "load the file in the second arg"
- rough sktch of idea for alternative for classic autoloader:
  - steps:
    - file name conventions
    - autoload_paths
    - scan the project tree beforehand
    - `user.rb` --> `autoload :User, 'user'`
  - technical difficulties:
    - Module#autoload uses an internal `require`
    - require is idempotent
      ```ruby
      require 'user' # --> true
      require 'user' # --> false
      ```
      - if you use require, can only load a file once/can't reload
    - no API to remove an autoload
      - how does `require` know the file has already been loaded? a simple array instance variable called `$LOADED_VARIABLES`. can implement reload by just calling `pop`:
      ```ruby
      require 'user' # --> true
      $LOADED_VARIABLES.pop
      require 'user' # --> true
      ```
      - cleaned up:
      ```ruby
      autoload :User, 'user' # --> true
      Object.send(:remove_const, :User)
      autoload :User, 'user' # --> true
      ```
    - implicit namespaces
      - based on file paths/directory names
      ```ruby
      # hotel.rb
      class Hotel
        include Pricing
      end
      ```
      vs:
    - explicit namespaces
      ```ruby
      #hotel/pricing.rb
      module Hotel::Pricing
      end
      ```
      - the idea: define a callback
    - Module#autoload has been deprecated for 7 years
      - but zeitwerk convinced Matsumoto not to remove the code from Rails
- the gem: Zeitwerk
  ```ruby
  initializer :let_zetwerk_take_over do
    if config.autoloader -- :zeitwerk
      # add reloader methods to autoloader config, overwrite some other things
    end
  end
  ```
