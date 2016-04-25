In users_controller.rb file within the my_friends action, create an @friendships instance variable:

    @friendships = current_user.friends

In the navbar (_navigation.html.erb) add a link to my_friends_path in place of Link3

    <%= link_to "My Friends", my_friends_path %>

In the views for my_friends.html.erb under the app/views/users folder, add the following:
```ruby
<%= render 'friends/lookup' %>
  <% if @friendships.size > 0 %>
<% else %>
  <div class='row col-lg-12'>
    <p class="lead">You don't have any friends yet. Please add some!</p>
  </div>
<% end %>
```
Copy over the code from stocks/list to this after if @friendships.size > 0 above:
```ruby
<table class="table table-striped">
    <thead>
      <tr>
        <th>Name</th>
        <th>Symbol</th>
        <th>Current Price</th>
        <% if @user.id == current_user.id %>
        <th>Actions</th>
        <% end %>
      </tr>
    </thead>
    <tbody>
      <% @user_stocks.each do |user_stock| %>
      <tr>
        <td><%= user_stock.name %></td>
        <td><%= user_stock.ticker %></td>
        <td><%= user_stock.price %></td>
        <% if @user.id == current_user.id %>
        <td>
          <%= link_to 'Delete', user_stock_path(user_stock),
                                :method => :delete,
                                :data => { :confirm => "Are you sure?" },
                                :class => "btn btn-xs btn-danger" %>
        </td>
        <% end %>
      </tr>
      <% end %>
    </tbody>
</table>
```
Now add the following routes to routes.rb file:

    resources :users, only: [:show]
    resources :friendships