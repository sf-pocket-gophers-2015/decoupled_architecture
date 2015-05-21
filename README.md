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

```
gem install rails-api
```
