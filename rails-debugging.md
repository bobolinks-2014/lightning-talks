# Debugging in Rails

As our development environments get more complex, debugging is more important than ever. Rails has a few handy tools that let us peek behind the scenes and see what's going on.

## Links (TL;DR)

### Built-in Rails Debugging: http://guides.rubyonrails.org/debugging_rails_applications.html

### Better Errors Gem: https://github.com/charliesome/better_errors

## Built-In Debugging

## View Helpers - debug, to_yaml, inspect

View helpers let us isolate variables in a view and inspect them right on the page when it's rendered in the browser. For examples of this, let's take a look at a very basic craigslist implementation in Rails. This is a chunk of our categories/show view:

```<h1><%= @category.name.capitalize %></h1>
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
  </tr>
  <% @articles.each do |article| %>
  <tr>
    <td><%= link_to article.title, category_article_path(@category, article) %></td>
    <td><%= article.text %></td>
    <td><%= link_to 'Edit', edit_category_article_path(@category, article) %></td>
    <td><%= link_to 'Destroy', category_article_path(@category, article), method: :delete, data: { confirm: 'You serious?' } %></td>
  </tr>
  <% end %>
```

Here's what we see when we load the page:
![Untouched page](http://i.imgur.com/09Yh4NG.png)

If we insert the "debug" helper to inspect @articles....

```<th>Title</th>
    <th>Text</th>
  </tr>
  <!-- Let's try to use a view helper here to make sure we're including the right objects in the @articles variable. -->
  <%= debug @articles %>
  <% @articles.each do |article| %>
  <tr>
    <td><%= link_to article.title, category_article_path(@category, article) %></td>
    <td><%= article.text %></td>
```

...we get something like this:
![Debug illustrrated](http://i.imgur.com/CIAKvRg.png)

Here's how the syntax looks if you want to use YAML instead. This:

`<%= @articles.to_yaml %>`

gives us this:
![YAMLicious](http://i.imgur.com/YtDuOxK.png)

Notice the gross formatting. Let's fix that.

`<%= simple_format @articles.to_yaml %>`

![YAML formatted](http://i.imgur.com/FfKfA15.png)

You can also use inspect.

`<%= @articles.inspect %>`

![Inspect](http://i.imgur.com/MPmWrEK.png)

Basically, pick your poison. View helpers are your console log, your puts, your soul's innermost reflection. Use them wisely, and don't forget to get rid of them when you want your page looking nice again.


##Better Errors
Better errors is a gem that "replaces the standard Rails error page with a much better and more useful error page." It can do all kinds of cool stuff. 

To get the full feature set, you need to do two things. First, you need to add it to your Gemfile:


```ruby
group :development do
  gem "better_errors"
end
```

And next you need to add a secondary gem to your Gemfile:

```ruby
gem "binding_of_caller"
```

Since we're using Rails, that's all you need to do to get it working. Once you've done that, you've got a dramatically improved error page that gives you fun features like a full stack trace to help you find errors, source code inspection with syntax coloring and error highlighting, and local and instance variable inspection.