---
title: "call and send in Ruby"
---
- `send` can be used anywhere
- `call` can only be used with proc
- can not `call` private method

```rb
class MyClass
  def test_send
    puts "using send"
    send(:foo)
    send(:bar)
  end

  def test_call
    puts "using call"
    :foo.to_proc.call(MyClass.new)
    :bar.to_proc.call(MyClass.new)
    # :bar.to_proc ==> proc {|object| object.bar}
    # :bar.to_proc.call(MyClass.new) ==> MyClass.new.bar
    # bar is private so can not call in object
  end

  def foo
    puts "foo"
  end

  private
  
  def bar
    puts "bar"
  end
end

MyClass.new.test_send
MyClass.new.test_call
```
