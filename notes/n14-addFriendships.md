Now we're going to add friends to the application, ability to view others users using the app and follow them so we can track their stock picks

First thing is add routes, add the following route to config/routes.rb file:

    get 'my_friends', to: 'users#my_friends'

Now you have to create a my_friends action in the users_controller.rb file, then a my_friends.html.erb file under the app/views/users folder, fill it in with some temporary text

Verify that the route is loading in the browser and you are able to view the text

Create a friendship model which will create a friendships table

    rails generate model Friendship

then go to friendship.rb model file and add in

    belongs_to :user
    belongs_to :friend, :class_name => 'User'

in the user.rb file

    has_many :friendships
    has_many :friends, through: :friendships

Add these two lines to the migration file within the create table:

    t.belongs_to :user
    t.belongs_to :friend, class: 'User'

Then run 

    rake db:migrate 

to create the table

Do a commit and push to github