---
layout: post
title: Does the expression 50.days.ago respect the Time Zone?
published: true
---
Does the expression `50.days.ago` (rails syntactic sugar for a time 50 days ago before the current moment in time) respect the user’s timezone?

50.days returns an `ActiveSupport::Duration` as we in the class `Numeric` in `time.rb`.
```ruby
def days
  ActiveSupport::Duration.days(self)
end

```
50.days.ago then calls the `ago` method in `ActiveSupport::Duration`.
Let’s take a look at this method in `duration.rb`.
```ruby
# Calculates a new Time or Date that is as far in the past
# as this Duration represents.
def ago(time = ::Time.current)
  sum(-1, time)
end
alias :until :ago
alias :before :ago
```
We see that the method `ActiveSupport::Duration#ago` calls a private method called sum.
```ruby
private
  def sum(sign, time = ::Time.current)
    unless time.acts_like?(:time) || time.acts_like?(:date)
      raise ::ArgumentError, "expected a time or date, got #{time.inspect}"
    end

    if parts.empty?
      time.since(sign * value)
    else
      parts.inject(time) do |t, (type, number)|
        if type == :seconds
          t.since(sign * number)
        elsif type == :minutes
          t.since(sign * number * 60)
        elsif type == :hours
          t.since(sign * number * 3600)
        else
          t.advance(type => sign * number)
        end
      end
    end
  end

```
And this method references a variable called `parts`. Where'd that come from?
`parts` must be an attribute of `ActiveSupport::Duration`. So I figured it would be from the initializer.
```ruby
def initialize(value, parts) #:nodoc:
  @value, @parts = value, parts.to_h
  @parts.default = 0
  @parts.reject! { |k, v| v.zero? } unless value == 0
end
```
But we only passed one argument to the initializer: `ActiveSupport::Duration.days(self)` through the expression `50.days`. So `parts` must be an empty hash! 
```
irb(main):001:0> nil.to_h
=> {}
irb(main):002:0> {}.empty?
=> true
```
Which means parts.empty? returns true.
Taking a look at the private method `sum` again, I realized that the method returns the following expression:
```ruby
time.since(sign * value)
```
We know that sign is -1 (see the `ago` method). We know that value is 50 (from `50.days`). And we know that time is set to the default `Time.current` (also see the `ago` method).
So the expression is equivalent to
```ruby
Time.current.since(-50)
```
So the question of whether the expression `50.days.ago` respects the time zone of the user simplifies to this question: does Time.current respect the time zone of the user? A quick look at the current method tells us the answer.
```ruby
# Returns <tt>Time.zone.now</tt> when <tt>Time.zone</tt> or <tt>config.time_zone</tt> are set, otherwise just returns <tt>Time.now</tt>.
def current
  ::Time.zone ? ::Time.zone.now : ::Time.now
end
```
YES IT DOES!
So after a long rabbit hole excursion, we can safely assume that 50.days.ago respects the time zone of the user! Thanks Rails team!