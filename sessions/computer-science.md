# Computer science you might (not) want to know

Andy Andrea

## Numbers

```ruby
0.1 + 0.2 == 0.3  # false
```

- why?
  - we use a base 10/decimal system, but computers use base 2/binary/bicimal
  - if we switch our base, some numbers can be represented by a finite number of digits instead of infinite
    - e.g., 1/3 in base 10 is an infinitely long decimal. But in base 3, it's just 0.1
    - not practical for a computer to attempt to store infinite decimasl b/c would require infinite space. Instead, it rounds off to save space.
      - this rounding causes ruby to evaluate 0.1 + 0.2 as something other than 0.3 (because it's not using base 10 to store the float?)
- ruby's `Rational` data type stores fractions as an equation, and the `BigDecimal` type stores fractions as an array of digits with an exponent
- why is this important?
  - to know when to use floats vs. decimals
    - e.g. when handling money: floats will give you rounding errors, but they have performance advantages over decimals
- why this is less important for rails devs
  - not super relevant to us b/c rails hides these details in abstractions
  - more important to understand how & when to use sth than how it works

## Algorithms

- definition: "A process or set of rules to be followed... especially by a computer"
- intro to comparison of sorting algorithms and their runtimes
- overview of time complexity and Big O, Big Omega, and Big Theta notation
- example of writing an algorithm and evaluating its time complexity
  - binary search
    - realworld use case for binary search: `git-bisect`
- caveats:
  - algorithmic complexity is rarely the true bottleneck for a web app
    - database calls and network requests take much longer
  - algorithmic complexity is a poor metric in real applications and is hard to measure accurately anyway (lots of confounding factors)
  - algorithms can be misused, e.g., trying to write your own sort instead of using the one provided by your framework
