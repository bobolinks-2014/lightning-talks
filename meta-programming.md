#Method Missing and Meta-programming

##Method Missing
###What is method_missing?

`method_missing` is a method that YOU DEFINE for a class. 

Due to special Ruby magic, this method is called whenever you try to make a non-existant method call.

It can be defined as an instance method:
````
class SomethingOrOther
	def method_missing(method, *arguments, &block)
		p "3"
	end
end
````
or, as a class method:
````
class SomethingOrOther
	def self.method_missing(method, *arguments, &block)
		p "3"
	end
end
````
So, when you try do this:
````
stuff = SomethingOrOther.new

stuff.nonexistant_method
````
Ruby understands it like this:
````
stuff.method_missing(:nonexistant_method)
````

###What can you use it for?
* Set the default response for undefined methods.
* Invoke dynamic functionality.

````
...
class AnotherThing
	def self.method_missing(method, *arguments, &block)
		if method.to_s =~ /^cool_/
			p "Way to go, smart guy. You just passed me #{arguments}"
		elsif method.to_s =~ /^lame_/
			p "Shut up, nobody asked you."
		else
			p "Try again"
		end
	end
end
...
````



##Define Method
`define_method` is another option, but it's less powerful.

````
class SomeOtherCrap
	define_method("just_a_string") do |argument|
		p "#{argument} bar."
	end
end

stuff = SomeOtherCrap.new

stuff.just_a_string("foo")

#=> "foo bar"
````

But, it can also be used for more dynamic method definitions. Here's an example I stole from [Ruby Monk](http://rubymonk.com/learning/books/2-metaprogramming-ruby/chapters/25-dynamic-methods/lessons/72-define-method):

````
class Doctor
  ["rhinoplasty", "checkup", "interpretive_dance"].each do |action|
	  define_method("perform_#{action}") do |argument|
			"performing #{action.gsub('_', ' ')} on #{argument}"
  	end
  end
end

doctor = Doctor.new
puts doctor.perform_rhinoplasty("nose")
puts doctor.perform_checkup("throat")
puts doctor.perform_interpretive_dance("in da club")
````


##Other cool stuff:
###Send

`.send()` takes a string; interprets it as a method to call on that object. 
````
[1,2,3,4,5,6,7,8,9].send('length')

#=> 9
````
This can be somewhat dangerous, as it can be used to call private messages.

###Eval
`eval()` takes a string, evaluates it as ruby code

````
eval ("[3,4,5,6].inject(:+)")

#=> 18
````
