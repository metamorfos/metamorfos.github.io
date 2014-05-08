---
layout: post
published: true
title: "Keeping testing simple"
---

I've been coding Ruby for about 2 years and writing tests for about half of the
time.
I started writing tests using `Rspec` as my testing framework because that's
what my team at the time used. But, with time I've come to think that `Rspec`
is a bit bloated. The simplicity of TDD frameworks like `Test::Unit` and
`Minitest` is hard to come close to for BDD framworks like `Rspec`. The amount
of "correctness" that are enforced when you are writing a spec gives too much
overhead.

My long-term mentor [Sirupsen][simon-twitter] [wrote][simon-article]:

> Tests should be the most transparent part of your stack. They are your
> definitive documentation, and something you will come back to again and
> again. And what is more lucid than the programming language you've been using
> for years? I understand and appreciate the behavior of Ruby, and it shouldn't
> feel like I'm writing a "bad spec" if I use that instead of my testing DSL.

I couldn't agree more, I want the feedback-loop between my mind and my fingers
be as short as possible while slinging out code. Having a testing framework
that is no different than the code under test simplifies the loop. It removes
the extra time of translating what I my production code is doing to a DSL.
My code is written in Ruby for a reason, it's simple and clear. I want my tests
to be the same. No magic, no ponies, just Ruby.

## Example

{% highlight ruby %}
class TestMeme < Minitest::Test
  def setup
    @meme = Meme.new
  end

  def test_that_kitty_can_eat
    assert_equal "OHAI!", @meme.i_can_has_cheezburger?
  end

  def test_that_it_will_not_blend
    refute_match /^no/i, @meme.will_it_blend?
  end

  def test_that_will_be_skipped
    skip "test this later"
  end
end
{% endhighlight %}

What's important to me is that `TestMeme` is just a simple subclass, and
`test_that_kitty_can_eat` is a simple method. If you want a better assertion,
then it's just a method definition away.

{% highlight ruby %}
def assert_palindrome string
  reverse = string.reverse
  assert_equal reverse, string
end
{% endhighlight %}

## Conclusion

Structuring your tests is as easy as structuring your code. Do you have a lot
of repetition in your tests? Extract into a Mixin and include it. Do you want
to write your own matcher to be more expressive? It's just a method away.
This not be it for everyone, but it sure hells floats my boat.


[simon-twitter]: https://twitter.com/sirupsen
[simon-article]: http://sirupsen.com/testing
