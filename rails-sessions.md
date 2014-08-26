#Sessions in Rails
###What gives?
I know we ran into a number of issues when making web apps and a large proponent of those problems involves user authentication and the storing of such sessions to make them public and available.

##The wrong way
When doing this earlier today, we stopped and asked someone how to handle the logic and they said, "Do you think your User controller should be handling these requests?" The correct answer is, no, it should not be. So please don't have things like this all up in your business:

```ruby
class UsersController < ApplicationController
    def create_session_blah
        @user = User.find_by(email: params[:email])
        if @user.authenticate(params[:password])
            session[:user_id] = @user.id
        else
            blah
        end
    end
end
```
So, what we have there is a failure on all accounts. The beauty about a session controller is that we can set that bad mamma jamma up to accept this little thing called RESTfullnes. I know, I know REST is like the Internet--just a fad that is sure to die off. But, in the off chance that it is around for more than 2 years (doubtful) let's set it up that way.

##The 'REST' of the way
See what I did there? I tried to make a pun for RESTful routing and what have you's. Ok, let's take a look at how we could work this session as a controller and keep it peachy-clean jelly bean.

>Sorry if this drifts at times, I'm watching the True Blood season finale and everything is getting ultra real right now. Not real in the 'vampires aren't a real thing anymore' real, but more in the sense of the show being all 'dramatic'.

```ruby
class SessionsController < ApplocationController

end
```

That is a simple enough first step. Let's make a controller to handle all these little thingys. Next, we can start making a few RESTful situations to adhere to.

```ruby
class SessionsController < ApplocationController
    def create
        #we will use this to create a new session(aka, logging in)
    end

    def destroy
        #we will use this to destroy a session(aka, log out)
    end
end
```

Yeah, I'm not going to write out this entire class but I think you get the idea. The last thing I will show you is how to, if you want, update your routes as well.

```ruby
    get "logout" => "sessions#destroy", :as => "logout"
    get "login" => "sessions#new", :as => "login"
```

So, you are looking at this jazz and you are saying...but Joey, what does this all MEAN?! Well, aside from this picture of this cat...

![cat](http://stylopics.com/wp-content/uploads/2013/10/Funny-Cat-Pictures-cats-935656_500_375.jpg)

...there are some useful things to note here.
1. The part right after get is the standard url http://bobo-news.herokuapp.com/['logout'].
2. The Second part is a a sweet hash rocket saying, YO! Get the destroy route in the session controller-if you want-or don't. 
3. The last part, now this one is fancy as can be, is giving your route a name, and I will go into further detail re: this aspect below.

###Naming routes and why that is somesing kewl
If you want to use fancy ruby such as:
```
<%= link_to logout_path %>
```

You better have a fresh to death path name setup, or it will just blahhhh. So using `:as =>` is super useful so we can keep Rails convetion and continue about our merry way. Or is it marry way?

##Conclusion
I hope this was at least somewhat helpful, or at the very least, a resource to come back to if you forget because I have included an EXTREMELY useful rails cast that goes through almost everything I said.

##Resources
[Rails Cast for Auth](http://railscasts.com/episodes/250-authentication-from-scratch)
[Rails Guides for Security](http://guides.rubyonrails.org/security.html#what-are-sessions-questionmark)
[Bobo-News Webpage for overall awesomeness](http://bobo-news.herokuapp.com)