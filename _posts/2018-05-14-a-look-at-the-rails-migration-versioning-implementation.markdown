---
layout: post
title: "A look at the rails migration versioning implementation"
date: 2018-05-14
comments: true
---

<p class="intro">So i took a hiatus for a while, and didnt used rails for a couple of years.</p>

Now that i am back building a product with rails, i have noticed `ActiveRecord::Migration` is verioned and you need to inherit from a versioned class `ActiveRecord::Migration[{n}]` where n is the migration version, it could be **4.2**, **5.0**, **5.2** etc...

How does this works?
<br />
<br />

## The naive approach:
So, the first thing i tried to do was declare a class with `[5.0]` at the end of the name, turns out this is not valid ruby class syntax:

test.rb:
```ruby
#/usr/bin/env ruby
class MyClass[5.0]
  def hello
    p "hello"
  end
end

c = MyClass.new
c.hello
```

running the script gives us:

```
test.rb:1: syntax error, unexpected '\n', expecting &. or :: or '[' or '.'
test.rb:5: syntax error, unexpected keyword_end, expecting end-of-input
``` 

## The way rails do it:

After searching on the rails source code, i stumbled at [this](https://github.com/rails/rails/blob/master/activerecord/lib/active_record/migration.rb#L536) snippet of code:

```ruby
def self.[](version)
  Compatibility.find(version)
end
```

of course! now it makes sense.

the `ActiveRecord[5.2]` idiom is just calling the class method `[]` with argument `5.2`,
later that method, is just doing a `dynamic-dispatch` with the `Compatibility.find` method.

## Our own implementation:
[Richard Feynman](https://en.wikipedia.org/wiki/Richard_Feynman) once said: **"What I cannot create, I do not understand"** so with that in mind, lets make our own implementation:

test.rb:
```ruby
#/usr/bin/env ruby
class MyClass
  def self.[](version)
    case version
      when 1
        V1
      when 2
        V2
    end
  end
end

class V1
  def hello
    p "Hello from V1"
  end
end

class V2
  def hello
    p "Hello from V2"
  end
end

c1 = MyClass[1].new
c1.hello

c2 = MyClass[2].new
c2.hello
```

running gives us the expected output:

```ruby
"Hello from V1"
"Hello from V2"
```

## Conclusion:
i hope this sheds a bit of light, on the underlying "magic" rails implement, dont be afraid to dive into the source of rails and find out things on your own.
