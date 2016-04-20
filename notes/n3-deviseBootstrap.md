In your gemfile, add the devise gem:

    gem 'devise'

Then run:

    bundle install --without production

Then install devise in your application:

    rails generate devise:install

    rails generate devise User

    rake db:migrate to add users table

Add the following line to the application_controller.rb file under app/controllers:

    before_action :authenticate_user!

Add a logout link to the homepage which is the index.html.erb view under app/views/welcome folder:

    <%= link_to "logout", destroy_user_session_path, method: :delete %>

Add 

    gem 'twitter-bootstrap-rails' 
    
in your gemfile and 

    bundle install --without production

Then run the following commands:

    rails generate bootstrap:install static

    rails g bootstrap:layout application

override using Y

Then add 

    gem 'devise-bootstrap-views' 

in your gemfile and 

    bundle install --without production

Under your app/assets/stylesheets folder, add the following line to your application.css file above the *= require_tree . line:

    *= require devise_bootstrap_views

Then run the following two commands from the terminal:

    rails g devise:views:locale en

    rails g devise:views:bootstrap_templates

In your config/routes.rb file add the following line:

    devise_for :users

Deploy to heroku and test out authentication by signing up some users and then logging in/out

A problem faced here by students is after adding the 

    before_action :authenticate_user! 

in the application controller, the check doesn't seem to be working. 

Also, the logout link may generate an error 

    No route matches [GET] "/users/sign_out" 

even with the 

    method: :delete. 

If you face this error, the issue may be the following: 

The welcome controller was inheriting from ActionController::Base instead of ApplicationController, 
once you correct this it should work.