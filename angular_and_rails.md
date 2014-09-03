# Angular and Rails
In this example, I will be using a basic blog to demonstrate why Angular is a very interesting alternative to solving your frontend woes.


## Comparing Angular to Underscore Templates
What have we been doing before? When we create a fully fledged web app with a backend and a front end, You have models, views and controllers. You need to do a couple of things to make this all work: make a, AJAX call to the database, store some of the request return data front end (if necessary), and then render your view. This same MVC structure holds true for Angular, but in a far more concise way.

Lets say you want to get a page that shows all your blog posts. It looks like as follows:
##Controllers
####Non Angular Controller of sorts

```javascript
var App = {
  init: function(){
    this.$el= $("#content");
    this.posts = new PostCollection();
    this.getPosts();
  },
  this.posts.fetch().done(function() {
    this.displayPosts(this.posts);
  }.bind(this));

  displayPosts: function(posts){
    console.log("displayInbox");
    var inbox = new PostsView(posts);
    inbox.render();
    this.$el.html(inbox.$el);
  }
};
$( document ).ready(function() {
    App.init();
});
```

####Angular Controller
You have a frontend controller to tell everybody when to render. It looks like a case statement. This is in your 'main.js' file. Apologies, this is in coffeescript.

**Angular Vocab**

* Module: a container for the different parts of an app including controllers, services, filters, directives

```javascript
blogApp = angular.module('blogApp', [])
blogApp.config(['$routeProvider', ($routeProvider) ->
  $routeProvider.
    when('/posts',{
      templateUrl: '../templates/posts/index.html',
      controller: 'PostIndexCtrl'
    }).
    when('/posts/:id',{
      templateUrl: '../templates/posts/_.post_detail.html',
      controller: 'PostDetailCtrl'
    }).
    when('/portfolio',{
      templateUrl: '../templates/portfolio.html'
    }).
    otherwise({
      templateUrl: '../templates/home.html',
      controller: 'HomeCtrl'
    })
])
```
###Models
####Non-Angular Model
To display all the posts, we break it up into two Models. We have a collection of posts, and then a post.
Post

```javascript
function Post(options){
	this.title = options.title
	// etc etc etc
}
```
Post Collection

```javascript
function PostCollection(){
  this.model = Post;
  this.models = [];
};
PostCollection.prototype.find = function(id) {
  return _.find(this.models, function(post){
    return post.id === id;
  });
};
PostCollection.prototype.fetch = function() {
  var request = $.get("http://localhost:9393/posts");
  request.done(function(returnVal){
    _.each(returnVal.posts, function(post){
      this.models.push(new this.model(post));
    }.bind(this));
  }.bind(this));
  return request;
};
```

####Angular Model
In angular, that AJAX call and the multiple models can all get condensed into one Controller.

**Angular Vocab**

* Controller: the 'business logic' behind the views
* Scope: context where the model is stored so that controllers, directives and expressions can access it

The controller is where we make the AJAX call. You store your collection of posts in a single array called '$scope.posts'.

```javascript
blogApp.controller('PostIndexCtrl', ['$scope', '$location', '$http', '$routeParams', function($scope, $location ,$http, $routeParams){
		// we store our collection of posts in here
    $scope.posts = [];
    console.log("index ctrl")
    $scope.file;
    $scope.post_index = $routeParams.id -1;
    // the AJAX call
    $http({method:'GET', url: './posts.json'}).success(function(data){
      $scope.posts = data;
    });
  }
])
```
### Views
####Non-Angular View
In your view, you have to render the collection and each post in the collection.
CollectionView

```javascript
function PostsView(collection){
  this.collection = collection;
  this.$el = $("<div></div>");
  this.template = _.template($("#posts-template").html());
}
PostsView.prototype.render = function() {
  this.$el.html(this.template());
  _.each(this.collection.models, function(post){
    var postView = new PostRowView(post);
    this.$el.find("#content").append(postView.render());
  }, this);
  return this.$el;
}
```

Post Row View

```javascript
function PostRowView(model){
  this.model = model;
  this.$el = $("<div></div>");
  this.template = _.template($("#post-row-template").html());
}
PostRowView.prototype.render = function() {
  this.$el.html(this.template(this.templateData()));
  return this.$el;
};
PostRowView.prototype.templateData = function() {
  return {
    id: this.model.id,
    title: this.model.title,
    date: this.model.date,
  };
};
```

There are two also templates, one for the collection and one for all the row-views.

```html
<script type="text/template" id="post-row-template">
    <li id="<%=title%>"><%=title%></li>
</script>
```
####Angular View
In angular, there are awesome `ng` tags that you can use to embed awesome non-static functionality into your HTML. This means looping and all sorts of great stuff (Chris's talk on Angular covered this kind of functionality and it was great. I won't try to repeat it here). You can now simply refer to the posts collection we created earlier in the post controller and loop through it.

```html
<ul ng-repeat="post in posts" class = "no-bullet">
    <li id = '{{post.id }}' class = "post panel"><p class="right">{{post.date}}</p><a href="#/posts/{{post.id}}"><h3>{{post.title }}</h3></a><p> {{post.abstract}}</p></li>
</ul>
```

## So what's the takeaway?
Angular is great. It really shortens the amount of code you have to write. It also seems to mirror a lot of the backend MVC structure a little bit more clearly. But that is my opinion.

## Getting Angular integrated with Rails
One thing that always gets me confused with some tutorials and instructions is they tell you to download a file- but they don't tell you where to put it. I promise, I'll tell you where to put the files.

**these instructions are super rough so please use the resource I provided from honeybadger.com for better instructions!**

1. Download Angular! Grab the files called angular.js as well as angular-mocks.js. **where to put it** `app/assets/javascripts`
2. **models** You will be making your own angular controllers (this is not actually the controller), so you will want to create a folder for them. Angular controller files are `.js` files. **where to put them** Create a path `app/assets/javascripts/angular/controllers`
3. Set up your asset pipeline. Put the following lines of code in `app/assets/javascripts/application.js`:

```
//= require jquery
//= require jquery_ujs
// Add the following two lines
//= require angular
//= require main
//= require_tree .
```

4. **Views** You will also have your own views in Angular. Views are just plain old `html` files. **Where to put them** Create a path `/public/templates`. Note that I just put views here because I followed a tutorial that said that's where they go. It might make more sense to put them in `app/views`
5. **Controller** create a main.js file. This will contain your frontend Angular controller. **where to put it** `app/assets/javascripts`
6. **More Views** You will need to add `ng-app = appName` in your main html page like so: `<html ng-app = "appName">` appName should match the name you will give the angular module in your main.js file.


## resources
a great resource. Maybe made by one of our ancestral DBCers? This is what I followed to get Angular set up with Rails. I highly endorse it. Don't neccessarily follow his lead on scaffolding or using coffeescript.
https://www.honeybadger.io/blog/2013/12/11/beginners-guide-to-angular-js-rails

The angular docs are crazy good. https://docs.angularjs.org/guide
