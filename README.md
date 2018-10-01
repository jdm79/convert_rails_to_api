# How to convert an existing Rails app into an API

[Source](https://hashrocket.com/blog/posts/how-to-make-rails-5-api-only)

Goal: Optimise our existing Rails app to serve JSON content. 

## 1 - add config.api_only = true to config/application.rb

doing this makes our rack middleware shortened. on APIs you don’t need to have cookies or flash middlewares.

## 2 - remove unused web gems

e.g. coffee-rails, sass-rails, web-console etc

## 3 - get rid of:

* app/assets/
* lib/assets/
* vendor/assets/
* app/helpers/
* app/views/layouts/application.html.erb

should just have Mailer views on your app/views folder

## 4 - ApplicationController Inheritance

Change your ApplicationController to inherit from ActionController::API:

## 5 - Generators

From now on your generators will identify that you have set your api_only mode and then will generate API like controllers. Also there will be no more views being generated.

## 6 - JSON serializer

You can choose your preferred json serializer solution for that. Rails has an agnostic solution for that and relies on the implementation of to_json method. The top 2 solutions are: jbuilder and active_model_serializers.

jbuilder https://github.com/rails/jbuilder

## 7 - CORS

If your backend and frontend has different domains you'll need to enable CORS (cross-origin HTTP request). At this point you probably know that. Rails 5 now is "suggesting" to use rack-cors gem by letting commented that on new generated projects. Here it is a simple example:

``` ruby
# config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, "Rack::Cors" do
  allow do
    origins 'localhost:4200'
    resource '*',
      headers: :any,
      methods: %i(get post put patch delete options head)
  end
end
```

## Conclusion

Rails is well known for being a great tool for building web applications, and also for APIs. It took a long time, but now Rails 5 has brought the same Rails way to develop in the API world and this is amazing.


