Create a new rails application from your rails projects directory called finance-tracker

    rails new name-of-app 
    
to create a new rails app.

Change directory into your finance-tracker folder and ensure that the rails server runs and you are able to preview the app.

Initialize a git repository for your app files, the message should be "Initial commit of app" 
(ensure you do this from the finance-tracker directory)

    cd /path/to/my/codebase

    git init

    git add .

    git commit

Update the readme file to say something relevant about the app. 
Example: "This is the finance tracker app from the Complete Ruby on Rails Developer course"

Make a commit of your change.

Create a github repository for your finance-tracker app in your github account.

Push your code to your github repository so it's saved:

    git remote add origin git@github.com:yourgithubaccountname/repo-name.git

    git push -u origin master # Remember you only need to use this command the first time

To view remotes setup in your environment (from your app directory):

    git remote -v

Re-organize your gemfile to put 

    gem sqlite3 

in group development, test and create a group for production.

In your production group add: 

    gem pg (postgres) 
    gem rails_12factor 

to make your app heroku ready.
 
Run

    bundle install --without production 

to update your .gemfile.lock file.

Create a welcome controller, a root route for welcome#index, an index action in your welcome_controller.rb (should be empty at this time).

Create a welcome folder under your app/views/ folder and within the folder create an index.html.erb file and fill it in with 

    <h1>Welcome to the finance tracker app</h1>

Preview your app and ensure it goes to this page in local development.

Commit your code to your git repo.

Push your code to your github repo.

Create a heroku app for your application.

To create a new production version of your app hosted in heroku:

    heroku create

To push your application code to heroku (deploy your app):

    git push heroku master

Ensure you have committed all your local changes to your git repo prior to pushing to heroku by checking git status.

Rename the heroku app to something more friendly and reasonable:

    heroku rename newnameofyourapp

Deploy your code to your heroku app and ensure you are able to preview the app and it takes you to 
the "Welcome to the finance tracker app" page.

