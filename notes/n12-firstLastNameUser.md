Add first_name and last_name to users table via migration and add a full_name method to User

    rails generate migration add_first_and_last_name_to_users

within migration file add the following within the def change method:

    add_column :users, :first_name, :string
    add_column :users, :last_name, :string

Run 

    rake db:migrate 

to create the table

Now add a full_name method to user.rb file
```ruby
def full_name
  return "#{first_name} #{last_name}".strip if (first_name || last_name)
  "Anonymous"
end
```
Add the following to new.html.erb and edit.html.erb under app/views/devise/registrations folder under the email field:
```ruby
<div class="form-group">
  <div class="col-md-6 no-padding">
    <%= f.label :first_name %>
    <%= f.text_field :first_name, class: 'form-control' %>
  </div>
  <div class="col-md-6 no-padding left-padding">
    <%= f.label :last_name %>
    <%= f.text_field :last_name, class: 'form-control' %>
  </div>
</div>
```
Add the two classes to custom.css.scss:
```css
.no-padding {
    padding: 0 !important;
}

.left-padding {
    padding-left: 15px !important;
}
```
Now create a new folder called user under controllers and within the user folder create a file called registrations_controller.rb and fill it in:
```ruby
class User::RegistrationsController < Devise::RegistrationsController
  before_filter :configure_permitted_parameters

  protected
  def configure_permitted_parameters
    devise_parameter_sanitizer.for(:sign_up).push(:first_name, :last_name)
    devise_parameter_sanitizer.for(:account_update).push(:first_name, :last_name)
  end
end
```
Now we have to update the routes to allow for this registrations controller update, so update the route for devise in the routes.rb file:

    devise_for :users, :controllers => { :registrations => "user/registrations" }