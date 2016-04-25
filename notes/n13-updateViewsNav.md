Update the navigation portion of application.html.erb as below:
```ruby
<% if current_user %>
    <button type="button" class="navbar-toggle" data-toggle="collapse" 
                                                data-target=".navbar-responsive-collapse">
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
    </button>
    <div class="navbar-collapse collapse navbar-responsive-collapse">
      <ul class="nav navbar-nav col-md-6">
        <li><%= link_to "My Portfolio", my_portfolio_path %></li>
        <li><%= link_to "My Profile", edit_registration_path(current_user) %></li>
        <li><%= link_to "My Friends", my_friends_path %></li>
      </ul>
      <ul class="nav navbar-right col-md-4">
        <li class="col-md-8 user-name text-right">
          <i class="fa fa-user"></i>
          <%= current_user.full_name.present? ? current_user.full_name : current_user.email %>
        </li>
        <li class="col-md-4 logout"><%= link_to("logout", destroy_user_session_path, 
                                                 class: "btn btn-xs btn-danger",
                                                 method: :delete) %></li>
      </ul>
    </div>
<% end %>
```
Add the following classes to custom.css.scss file for styling:
```css
.logout {
  padding-left: 0;
}

.user-name {
  padding-top: 15px;
}

.nav.navbar-right li {
  .btn {
    color: #fff !important;
    margin-top: 5%;
  }
  .btn-danger:hover {
    background-color: darken(#d9534f, 20%) !important;
  }
}
```
Move over the navigation code (navbar) to a _navigation.html.erb partial under the app/views/layouts folder from the application.html.erb file and in it's place put the following line in the application.html.erb:

    <%= render 'layouts/navigation' %>

move the link to finance tracker outside the if block within the _navigation.html.erb file:

    <%= link_to 'Finance Tracker', root_path, class: "navbar-brand" %>

If you have a logout path in your index page you may remove it at this time

Make a commit, push to github and deploy to heroku