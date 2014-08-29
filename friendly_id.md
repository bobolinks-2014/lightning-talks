
#Getting Friendly with friendly_id (Too easy...)



#Why you should care? 

friendly_id is a gem for Rails that helps clean up your urls and make it easy for users to find specific sites instantly. It creates URLS for items/categories/ users etc  with their specific name rather than their ID. This make it super simple for a user to find themselves or anyone/ thing else they are trying to navigat to.

#Examples

	before: localhost:3000/bobos/8675309/projects/60657

	after: localhost:3000/bobos/rob/projects/amozin

#How to install install/include gem

	gem install friendly_id

after the gem is installed, include it into your Gemfile like so:

	gem 'friendly_id', '~> 5.0.0' # Note: You MUST use 5.0.0 or greater for Rails 4.0+

How to make semi-pretty URLs without friendly_id?

-go into the model you want to change and create the method to_param like so: 

```
class Kosher < ActiveRecord::Base
  def to_param
    "#{id} #{name}".parameterize
  end
end
```

How to make spiced up URLs with friendly_id:

```
class Kosher < ActiveRecord::Base
  extend FriendlyId
  friendly_id :name
end
```
lastly you need to add slugs to your code to make everything work

###(Side note: 

####what's a slug? 
semi-friendly urls

####Why is it called a slug?

from internet=> Also called catchline. a short phrase or title used to indicate the story content of newspaper or magazine copy.
)


```
class Kosher < ActiveRecord::Base
  extend FriendlyId
  friendly_id :name, use: :slugged
end
```
and to your migration: 

`$ rails g migration add_slug_to_koshers slug:string`

which creates: 

```
class AddSlugToKoshers < ActiveRecord::Migration
  def change
    add_column :koshers, :slug, :string
    add_index :koshers, :slug #need to add this since it will be used to find records
  end
end
```

#Best uses

Best used for sites where you want specific URL names to be easily found. ex: all the freaking blogs we created could use them. 

#That's it? 

![alt text](http://memecreator.eu/media/created/lnh4ri.jpg)

#Lots more to know- 
Fix urls if you edit the title. 
Have old urls automatically redirect to the new url 
Make your web app easy to use and accessible!  
Get rid of %20 betwen spaces
Many more reasons!!      


#Sources: 
http://railscasts.com/episodes/314-pretty-urls-with-friendlyid?view=asciicast

https://github.com/norman/friendly_id

http://blog.ereslibre.es/?p=343

