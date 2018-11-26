---
layout: post
title:  "Exploring Functional Programming in Ruby"
date:   2018-11-26 15:40:56
categories: update
---

While working in Elm over the past few years, I had the opportunity to learn about functional programming.  This was quite a bit different from the more object oriented approaches of the languages I had started programming in, Ruby and JS. Although it took me a bit to get used to, I have came to appreciate many of the concepts I was exposed to in functional programming.  One concept in particular is the ability to pass a function as an argument.

As I mentioned in a previous post, I really like doing exercises in Exercism.io.  They provide me the opportunity to play around with language features in a low-risk way.  So often what I do is try to solve the coding problems from that site in a variety of ways.  Today I was working on making this set of specs pass:

```
class AcronymTest < Minitest::Test
  def test_basic
    assert_equal "PNG", Acronym.abbreviate('Portable Network Graphics')
  end

  def test_lowercase_words
    assert_equal "ROR", Acronym.abbreviate('Ruby on Rails')
  end

  def test_punctuation
    assert_equal "FIFO", Acronym.abbreviate('First In, First Out')
  end

  def test_all_caps_word
    assert_equal "GIMP", Acronym.abbreviate('GNU Image Manipulation Program')
  end

  def test_punctuation_without_whitespace
    assert_equal "CMOS", Acronym.abbreviate('Complementary metal-oxide semiconductor')
  end

  def test_very_long_abbreviation
    assert_equal "ROTFLSHTMDCOALM", Acronym.abbreviate('Rolling On The Floor Laughing So Hard That My Dogs Came Over And Licked Me')
  end

  def test_consecutive_delimiters
    assert_equal "SIMUFTA", Acronym.abbreviate('Something - I made up from thin air')
  end
end
```

It was pretty easy to get them all passing with something like

```
class Acronym
  def self.abbreviate(phrase)
    phrase.split(/\W/).map{|w| w[0].to_s.upcase}.join("")
  end
end
```


but I wanted to try and solve it a few other ways. I thought it would be fun to try passing a function as an argument similar to how it would be done in Elm.  It also seemed like it would be nice to have a #keep_if method on string that would make things easier than splitting or gsubbing.  So I came up with the following:


```
class Acronym
  def self.abbreviate(phrase)
    phrase.keep_if(part_of_acronym)
  end

  class << self
    private

    def part_of_acronym
      -> (char, last_char) { acronymable?(char, last_char) }
    end


    def acronymable?(last_char, char)
      !last_char || (last_char.match(/\W/) && char.match(/\w/))
    end
  end
end

class String
  def keep_if(checker, accum=Accumulator.new(self))
    char = accum.remainder[0]
    remainder = accum.remainder[1..-1]
    string = accum.string
    return accum.string if !char
    if checker.call(accum.last_char, char)
      string += char.upcase
    end
    keep_if(checker, Accumulator.new(remainder, string, char))
  end
end

class Accumulator
  attr_reader :string, :last_char, :remainder
  def initialize(remainder, string="", last_char=nil)
    @last_char = last_char
    @remainder = remainder
    @string = string
  end
end

```

While this does add quite a bit more code, I think there are some benefits.

1.  String #keep_if is reusable.  For other cases, I just need to determine the appropriate function for what should be kept and pass it in.
2.  I think the syntax ```phrase.keep_if(part_of_acronym)``` is more explicit and readable.



