# [Acts As Taggable On](https://github.com/mbleigh/acts-as-taggable-on)


Rails plugin that allows users (website visitors) to give specific attributes (tags!) to multiple items. 

Basically, you need to tag something? Use this gem; it does nearly everything for you. 

## Set Up

### Follow instructions [here](https://github.com/mbleigh/acts-as-taggable-on) :)

But what does the gem do? 

Four, I said **four** new migration files!

```Ruby
acts_as_taggable_on_migration.acts_as_taggable_on_engine
add_missing_unique_indices.acts_as_taggable_on_engine
add_taggings_counter_cache_to_tags.acts_as_taggable_on_engine
add_missing_taggable_index.acts_as_taggable_on_engine
```

wtf. 

So what do you have to know is that there is a [polymorphic association](https://github.com/bobolinks-2014/lightning-talks/blob/master/polymorphic_assoc.md) with tags. 

It *does* use the old syntax for creating tables, i.e. self.up and self.down (inside of which is the create_table tag) instead of simply change

```Ruby
class ActsAsTaggableOnMigration < ActiveRecord::Migration
	def self.up
		create_table :tags do |t|
			t.string :name
		end
	end

	def self.down
		drop_table :tags
	end
end
```

Instead of 

```Ruby
class ActsAsTaggableOnMigration < ActiveRecord::Migration
	def change
		create_table :tags do |t|
			t.string :name
		end
	end
end
```

## What YOU need to do

Decide what you want to tag. In our example, we're tagging articles.

> Model! Add in acts_as_taggable

```Ruby
class Article < ActiveRecord::Base
  acts_as_taggable
end
```

> Forms! Add in tag_list

```HTML+ERB
	<% form_for :article do |f| %>
		<%= f.text_field :title %>
		<%= f.text_area :body %>
		<%= f.text_field :tag_list %>
		<%= f.submit %>
	<% end %>
```

This will then save a LIST of tags. But how does AATO know what we're dealing with? It automatically parses out comma-delimited tags.

Boom. 

```hello, this is a larger tag, dogs, boborules```

turns into

```"hello", "this is a larger tag", "dogs", "boborules", "dogs"```

In fact, this is what p-ing the records looks like: 

```Ruby
[#<ActsAsTaggableOn::Tag id: 1, name: "hello",                taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 2, name: "this is a larger tag", taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 3, name: "dogs",                 taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 4, name: "boborules",            taggings_count: 1>]
```

And if you make another article with the following tags: 

```"boborules", "cats", "dinosaurs", "constipation"```

Then your records would now look like this: 

```Ruby
[#<ActsAsTaggableOn::Tag id: 1, name: "hello",                taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 2, name: "this is a larger tag", taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 3, name: "dogs",                 taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 4, name: "boborules",            taggings_count: 2>,
 #<ActsAsTaggableOn::Tag id: 5, name: "cats",                 taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 6, name: "dinosaurs",            taggings_count: 1>,
 #<ActsAsTaggableOn::Tag id: 7, name: "constipation",         taggings_count: 1>]
```


> Things to look at  

  * ```taggings_count``` automagically knows how many times the tag has been called - but on *different* records. For example, saying "dogs" twice for one record doesn't increase the taggings_count by two. 

  * You can change the delimiter and it automagically gets rid of leading whitespace.


## Using - which is way more fun than not


```Ruby
# Find articles that matches all given tags:
Article.tagged_with(["dogs", "boborules"], :match_all => true)

# Find articles with any of the specified tags:
Article.tagged_with(["dogs", "boborules"], :any => true)

# Find articles that has not been tagged with dogs or boborules:
Article.tagged_with(["dogs", "boborules"], :exclude => true)
```

In order to get *all* the tags:

```Ruby
Article.tag_counts_on(:tags)

# You can add on an #order method; below we find all tags in order of most-used
Article.tag_counts_on(:tags).order(taggings_count: :desc)
```


## "Advanced" Topics

<small>aka stuff we haven't gotten to</small>

You can create multiple **categories** of tags for one Model. For example, on Article we could add the following line to the model in order to get a couple different categories of tags: 

```Ruby
acts_as_taggable_on :sports, :editorials, :business 
```

These help give your tags context. Check gem documentation for more info!







