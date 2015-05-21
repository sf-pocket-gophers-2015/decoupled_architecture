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
- This had become such a popular pattern that the API functionality provided by the Rails API gem will be a part of Rails 5 core

## Same Origin Policy/CORS

- Prevents one site from accessing a different sites DOM
- Permits scripts running on pages originating from the same site – a combination of scheme, hostname, and port number – to access each other's DOM with no specific restrictions
- applies to XMLHttpRequests unless the server provides an Access-Control-Allow-Origin (CORS) header

## Let's Code it Up!

We are going to create two separate apps.  One will be our API backend, and the second one will be the client, a totally separate app that's going to talk to our API.  Let's start with the API server.

Install the Rails API gem
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
def index
  render json: { gophers: "pocket" }
end
```

Now let's switch to working on the client.  We could use whatever we want for this, we just need a server that will serve our `.js`, `.css`, and image files.  Rails, Sinatra, Node, some other thing.  For the purposes of this demo, let's make another Rails app.

```
rails new appname
```

Now let's write some JS in our client app to hit our server app.  We'll make a file called `something.js` in our `app/assets/javascripts` folder and put an ajax call in there

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

## Resources
- (Rails API gem)[https://github.com/rails-api/rails-api]
- (ASCIIcast for Rails API gem)[http://railscasts.com/episodes/348-the-rails-api-gem?view=asciicast]
- (Same Origin Policy)[http://en.wikipedia.org/wiki/Same-origin_policy]
- (CORS)[http://en.wikipedia.org/wiki/Cross-origin_resource_sharing]
- (Rack CORS gem)[http://en.wikipedia.org/wiki/Cross-origin_resource_sharing]
