Create a folder called friends under app/views folder and within it create a partial called _lookup.html.erb and fill it in (You can copy over code from stocks/lookup partial and make updates as below):
```ruby
<div id="friend-lookup">
  <h3>Search for friends</h3>
  <%= form_tag search_friends_path, remote: true, method: :get, id: 'friend-lookup-form' do %>
      <div class="form-group row no-padding text-center col-md-12">
        <div class="col-md-10">
          <%= text_field_tag :search_param, params[:search_param],
                             placeholder: "first name, last name or email", autofocus: true,
                             class: 'form-control search-box input-lg' %>
        </div>
        <div class="col-md-2">
          <%= button_tag(type: :submit, class: "btn btnplg btn-success") do %>
              <i class="fa fa-search"></i> Look up a friend
          <% end %>
        </div>
      </div>
  <% end %>
  <%= render 'common/spinner' %>
  <% if @users %>
      <% if @users.size > 0 %>
          <div id="friend-lookup-results" class="well results-block col-md-10">
            <table class="search-results-table col-md-12">
              <tbody>
              <% @users.each do |user| %>
                  <tr>
                    <td><strong>Name:</strong> <%= user.full_name %></td>
                    <td><strong>Email:</strong> <%= user.email %></td>
                    <td><strong>Profile:</strong> <%= link_to "View profile", user_path(user),
                                                              class: "btn btn-xs btn-success" %>
                      <% if current_user.not_friends_with?(user.id) %>
                          <%= link_to "Add as my friend", add_friend_path(user: current_user, 
                                                                          friend: user),
                                      class: "btn btn-xs btn-success",
                                      method: :post %>
                      <% else %>
                        <span class="label label-primary">
                            You are friends
                          </span>
                      <% end %>
                    </td>
                  </tr>
              <% end %>
              </tbody>
            </table>
          </div>
      <% else %>
          <p class="lead col-md-12">
            No People match this search criteria
          </p>
      <% end %>
  <% end %>
  <div id="friend-lookup-errors"></div>
</div>
```
Add routes for the views for search and add friend in the routes.rb file:

    get 'search_friends', to: 'users#search'
    post 'add_friend', to: 'users#add_friend'