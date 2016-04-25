Now we're going to add a display of the stocks the user is already tracking in the my_portfolio page in the users_controller.rb file within the my_portfolio action:

    @user_stocks = current_user.stocks
    @user = current_user

Then add the line below to the my_portfolio view under app/views/users folder:
```ruby
<%= render 'stocks/list' %>
```
Now create a _list.html.erb partial in the app/views/stocks folder and fill in the code below:
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
            <%= link_to 'Delete', 
              user_stock_path(user_stock), 
              :method => :delete, 
              :data => { :confirm => 'Are you sure?' }, 
              :class => 'btn btn-xs btn-danger' %>
        </td>
      <% end %>
      </tr>
    <% end %>
  </tbody>
</table>
```
To make the destroy action work, make the following change in the user_stocks_controller.rb file within the destroy action:
```ruby
redirect_to my_portfolio_path, notice: 'Stock was successfully removed from portfolio.'
```
You can now verify in server, do a commit and push to github/heroku and verify in production.