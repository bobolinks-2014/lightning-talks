# rake! 

We use rake everyday, but do we really, truly KNOW rake? 

## Default rake tasks

There are a LOT. You've been warned...


```
rake about                              # List versions of all Rails frameworks and the environment
rake assets:clean[keep]                 # Remove old compiled assets
rake assets:clobber                     # Remove compiled assets
rake assets:environment                 # Load asset compile environment
rake assets:precompile                  # Compile all the assets named in config.assets.precompile
rake cache_digests:dependencies         # Lookup first-level dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake cache_digests:nested_dependencies  # Lookup nested dependencies for TEMPLATE (like messages/show or comments/_comment.html)
rake db:create                          # Creates the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use d...
rake db:drop                            # Drops the database from DATABASE_URL or config/database.yml for the current RAILS_ENV (use db:...
rake db:fixtures:load                   # Load fixtures into the current environment's database
rake db:migrate                         # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:status                  # Display status of migrations
rake db:rollback                        # Rolls the schema back to the previous version (specify steps w/ STEP=n)
rake db:schema:cache:clear              # Clear a db/schema_cache.dump file
rake db:schema:cache:dump               # Create a db/schema_cache.dump file
rake db:schema:dump                     # Create a db/schema.rb file that is portable against any DB supported by AR
rake db:schema:load                     # Load a schema.rb file into the database
rake db:seed                            # Load the seed data from db/seeds.rb
rake db:setup                           # Create the database, load the schema, and initialize with the seed data (use db:reset to also ...
rake db:structure:dump                  # Dump the database structure to db/structure.sql
rake db:version                         # Retrieves the current schema version number
rake doc:app                            # Generate docs for the app -- also available doc:rails, doc:guides (options: TEMPLATE=/rdoc-tem...
rake log:clear                          # Truncates all *.log files in log/ to zero bytes (specify which logs with LOGS=test,development)
rake middleware                         # Prints out your Rack middleware stack
rake notes                              # Enumerate all annotations (use notes:optimize, :fixme, :todo for focus)
rake notes:custom                       # Enumerate a custom annotation, specify with ANNOTATION=CUSTOM
rake rails:template                     # Applies the template supplied by LOCATION=(/path/to/template) or URL
rake rails:update                       # Update configs and some other initially generated files (or use just update:configs or update:...
rake routes                             # Print out all defined routes in match order, with names
rake secret                             # Generate a cryptographically secure secret key (this is typically used to generate a secret fo...
rake stats                              # Report code statistics (KLOCs, etc) from the application
rake test                               # Runs test:units, test:functionals, test:generators, test:integration together
rake test:all                           # Run tests quickly by merging all types and not resetting db
rake test:all:db                        # Run tests quickly, but also reset db
rake time:zones:all                     # Displays all time zones, also available: time:zones:us, time:zones:local -- filter with OFFSET...
rake tmp:clear                          # Clear session, cache, and socket files from tmp/ (narrow w/ tmp:sessions:clear, tmp:cache:clea...
rake tmp:create                         # Creates tmp directories for sessions, cache, sockets, and pids
```

Many of these will be familiar to you, but there are a several new ones we know very little about. 

Let's explore some new, nifty tasks: 

###rake notes

Here's the Rails description for rake notes:

```rake notes                                          # Enumerate all annotations (use notes:optimize, :fixme, :todo for focus)```

There are three default notes that are built into Rails apps: OPTIMIZE, FIXME and TODO. 
Note: They need to be in upcase in the code, but downcase in the terminal.

#####example

Here's a method that needs some work

```Ruby
# TODO: look for more efficient way to do this
def format_date
	date = self.created_at
	[date.month, date.day, date.year].join(".")
end
```

How can I find a list of outstanding tasks tagged with 'TODO'?

```
$ rake notes:todo
app/models/post.rb:
  * [5] look for more efficient way to do this
```

What if we wanted to create our own notes? 

```rake notes:custom                                   # Enumerate a custom annotation, specify with ANNOTATION=CUSTOM```

```Ruby
# BOBO: may need some refactoring (tweet, tweet)
def full_name
	"Mr. Bobo, the Link"
end
```

Let's run a search for tasks tagged with 'BOBO':

```
$ rake notes:custom ANNOTATION=BOBO
app/models/user.rb:
  * [16] may need some refactoring
```

Now let's run a search for all notes:

```
$ rake notes
app/controllers/posts_controller.rb:
  * [7] [FIXME] need to fix redirect_to

app/models/post.rb:
  * [5] [TODO] look for more efficient way to do this
 ```

This will not return results for custom notes. If you'd like to include them, you'll need to redefine rake routes to add your own custom tag. Check out the Codemancers resources link provided below for step-by-step instructions. 

Also note the following: 

>By default, rake notes will look in the app, config, lib, bin and test directories. If you would like to search other directories, you can provide them as a comma separated list in an environment variable SOURCE_ANNOTATION_DIRECTORIES. -from RailsGuides

###rake assets:precompile

Have you ever deployed to Heroku and wondered, 'Where the heck is my CSS?'

Rails comes with a rake task to compile the assets. By default, compiled assets are written to the /assets directory.  

>You can call this task on the server during deployment to create compiled versions of your assets directly on the server.

Here's what you probably missed: 

```$ RAILS_ENV=production bin/rake assets:precompile```

For more info on the asset pipeline, check out the RailsGuide link below. 


###custom rake tasks

If you're wanting something fancy, you can even build your own rake task.

Let's try adding everyone's favorite rake command: `rake db:yolo`
Rails was kind enough to give us a generator for creating new tasks.

```Ruby
$ rails g task bobo yolo
      create  lib/tasks/bobo.rake
```

Here's what our new `bobo.rake` file contains:

```Ruby
namespace :bobo do
  desc "TODO"
  task yolo: :environment do
  end

end
```

We'll need to make a couple changes for our `bobo:yolo`

```Ruby
namespace :bobo do
  desc "drop, create, migrate bobo's database"
  task yolo: [:environment,'db:drop', 'db:create', 'db:migrate']

end
```

Let's give it a whirl!

```
$ rake bobo:yolo
== 20140825014830 CreateUsers: migrating ======================================
-- create_table(:users)
   -> 0.0081s
== 20140825014830 CreateUsers: migrated (0.0083s) =============================

== 20140825014902 CreatePosts: migrating ======================================
-- create_table(:posts)
   -> 0.0215s
== 20140825014902 CreatePosts: migrated (0.0218s) =============================

$
```

Hooray!

To learn more about rake tasks and custom rake tasks, check out the resources below...

##Resources

[RailsGuides: The Rails Command Line](http://guides.rubyonrails.org/command_line.html#rake)
[RailsGuides: The Asset Pipeline](http://guides.rubyonrails.org/asset_pipeline.html)
[Jaco Pretorius: All Rails db Rake Tasks and What They Do](http://www.jacopretorius.net/2014/02/all-rails-db-rake-tasks-and-what-they-do.html)
[Codedecoder: custom rake task in rails](http://codedecoder.wordpress.com/2013/05/03/custom-rake-task-in-rails/)
[Codemancers: Redefine rake routes to add your own custom tag in Rails](http://crypt.codemancers.com/posts/2013-07-12-redefine-rake-routes-to-add-your-own-custom-tag-in-Rails/)
