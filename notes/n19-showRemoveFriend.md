Create a friendships_controller.rb file under app/controllers folder:
```ruby
class FriendshipsController < ApplicationController

  def destroy
    @friendship = current_user.friendships.where(friend_id: params[:id]).first
    @friendship.destroy
    respond_to do |format|
      format.html { redirect_to my_friends_path, notice: "Friend was successfully removed"}
    end
  end
end
```
Add show action to users_controller.rb file:
```ruby
def show
  @user = User.find(params[:id])
  @user_stocks = @user.user_stocks
end
```
Now create a show.html.erb page under app/views/users folder to show user profiles:
```ruby
<h2>User Details</h2>

<dl class="dl-horizontal">
    <dt><strong>Full Name</strong></dt>
    <dd><%= @user.full_name %></dd>
    <dt><strong>Email</strong></dt>
    <dd><%= @user.email %></dd>
</dl>

<% if @user.stocks.size > 0 %>
  <%= render 'stocks/list' %>
<% else %>
  <p class="lead">This user is not tracking any stocks</p>
<% end %>

<%= link_to "Back", my_friends_path, class: "btn btn-primary" %>
```
Make a commit of your code and deploy to Heroku!

Congratulations on completing the finance tracker app and don't forget to post your herokuapp link to the discussions!!