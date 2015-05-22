# Decoupled Architecture: Rails as an API

## What is an API App?  What does it Meeeean?

- Instead of using Rails to generate dynamic HTML that will communicate with the server through forms and links, we can treat our web application as just another client, consuming a JSON API
- So our API app is a backend that serves data that is shared between one or more web apps or other native apps
- Often used together with a single-page JS app framework (like Backbone, Angular, Ember, etc)

## Why Use Rails for this?

- There's a lot more to Rails than the view layer and the asset pipeline
- Smart defaults for development and testing
- Detailed logging of every request
- Built in security measures
- Control of cache settings
- Routing
- Handy generators
- Developer ecosystem - gems and plugins that work with Rails out of the box
- Lots and lots more great reasons in the (Rails API gem readme)[https://github.com/rails-api/rails-api/blob/master/README.md#why-use-rails-for-json-apis]
- This had become such a popular pattern that the functionality provided by the Rails API gem will be a part of Rails 5 core

## Same Origin Policy/CORS

- Prevents one site from accessing a different sites DOM
- Permits scripts running on pages originating from the same site – a combination of scheme, hostname, and port number – to access each other's DOM with no specific restrictions
- applies to XMLHttpRequests unless the server provides an Access-Control-Allow-Origin (CORS) header

## Let's Code it Up!

We are going to create two separate apps.  One will be our API backend, and the second one will be the client, a totally separate app that's going to talk to our API.  

Let's start with the API server.  Install the Rails API gem
```
gem install rails-api
```

Create a new API app
```
rails-api new appname
```

- Note that now `ApplicationController` inherits from `ActionController::API` instead of `ActionController::Base`
- Also note that your `app` directory no longer has a `views` directory
- This also configures the generators to skip generating views, helpers and assets when you generate a new resource

Let's put in a route that we can hit, and a corresponding controller action.

In `routes.rb`

```ruby
resources :gophers
```

Create a `gophers_controller.rb`, and within that let's have one action.

```ruby
class GophersController < ApplicationController

  def index
    render json: { gophers: "pocket" }
  end
  
end
```

Let's bring up the server and leave it running, and we're ready to go.

Now let's switch to working on the client.  We could use whatever we want for this, we just need a server that will serve our `.js`, `.css`, and image files.  Rails, Sinatra, Node, some other thing.  For the purposes of this demo, let's make another Rails app.

Let's create a controller, and action, a view, and a corresponding route that will serve our `.js` file.   That's all review, the only difference is that when we bring up the client's server, we will need to run it on a different port, as our server is already running on 3000.  We can pass the `-p` flag to `bin/rails` to set a different port.

`bin/rails s -p 3003`

Now let's write some JS in our client app to hit our server app.  We'll make a file called `something.js` in our `app/assets/javascripts` folder and put an ajax call in there, something like this.

```javascript
$(document).on('page:change', function() {
    $.ajax({
        url: 'http://localhost:3000/gophers',
        type: 'get'
    }).done(function() {
        console.log('done');
    }).fail(function() {
        console.log('fail');
    });
});
```

Almost there!  Once we get our code working, we should expect to see a CORS error, because we haven't set any headers on our server to allow cross domain access.  One way to set these headers is right in the controller, with a `before_action`.  Something like:

```ruby
class GophersController < ApplicationController
  before_action :allow_cross_domain

  def index
    render json: { pocket: "gophers" }
  end

  private

    def allow_cross_domain
      headers['Access-Control-Allow-Origin'] = '*'
      headers['Access-Control-Allow-Methods'] = 'POST, GET, PUT, PATCH, DELETE, OPTIONS'
      headers['Access-Control-Allow-Headers'] = 'Origin, Content-Type, Accept, Authorization, Token'
    end
end
```

Note that this example is an extremely permissive policy.  You may want to restrict which domains can hit your API, or which methods you allow.

You can also use the `rack-cors` gem, which will set these headers in the rack middleware.

## Resources
- (Rails API gem)[https://github.com/rails-api/rails-api]
- (ASCIIcast for Rails API gem)[http://railscasts.com/episodes/348-the-rails-api-gem?view=asciicast]
- (Same Origin Policy)[http://en.wikipedia.org/wiki/Same-origin_policy]
- (CORS)[http://en.wikipedia.org/wiki/Cross-origin_resource_sharing]
- (Rack CORS gem)[http://en.wikipedia.org/wiki/Cross-origin_resource_sharing]
