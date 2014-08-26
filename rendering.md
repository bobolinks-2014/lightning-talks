

# Rendering in Rails


## Default Rendering
In Rails, controllers automatically render views if the name is the same as the route. For instance, if your controller looks like this:

```
class CategoriesController < ApplicationController
  def show
    @category = Category.find(params[:id])
  end
end
```
And a corresponding route:
```
resources :categories

```
And a view app/views/categories.html.erb. It will automatically render that view.
```
<div class="container">
  <h1 class="category-name"><%= @category.name %></h1>

  <div class="category-articles">
  <% @category.articles.each do |article| %>
    <li><%= link_to article.title, category_article_path(@category, article) %></li>
  <% end %>
  </div>
</div>
```
Rails will automatically look for, and render the appropriately named template in the view path if you do not specify a path in the controller.

## Non-Default Rendering
You can of course, render a different template from a different controller by specifying the file path.

```
render "articles/show"
```

You can also render a file outside your file structure.

```
render file:
"/u/apps/cheese_factory/current/app/views/cheeses/show"
```

You can render other stuff like JavaScript:

```
render js: "alert('Holy Jeebus!');"

```

## Redirect
While render tells Rails which view to use, redirect_to tells the browser to send a new request for a different url.

```
redirect_to root_path
```

Rails even has a convenient 'back' argument

```
redirect_to :back

```


