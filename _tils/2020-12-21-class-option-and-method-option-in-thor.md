---
title: "class_option and method_option in thor"
---
`class_option` is used for all method in class.
`method_option` just be used in one method.
```rb
class_option :number, :type => :string, :description => 'Number to call', :default => '555-1212'

desc 'hi', 'Say hi!'
method_option :name, :type => :string, :description => 'Name to greet', :default => 'there'
def hi
  puts "Hi, #{options[:name]}! Call me at #{options[:number]}"
end

desc 'bye', 'say bye!'
def bye
  puts "Bye! Call me at #{options[:number]}"
end
```
