Create a friends.js file under app/assets/javascripts folder and fill it in by copying over the stocks.js file and changing all the stock to friend:
```javascript
var init_friend_lookup;

init_friend_lookup = function() {
    $('#friend-lookup-form').on('ajax:before', function(event, data, status){
        show_spinner();
    })
    $('#friend-lookup-form').on('ajax:after', function(event, data, status){
        hide_spinner();
    })
    $('#friend-lookup-form').on('ajax:success', function(event, data, status){
        $('#friend-lookup').replaceWith(data);
        init_friend_lookup();
    });

    $('#friend-lookup-form').on('ajax:error', function(event, xhr, status, error){
        hide_spinner();
        $('#friend-lookup-results').replaceWith(' ');
        $('#friend-lookup-errors').replaceWith('person was not found.');
    });
}


$(document).ready(function() {
    init_friend_lookup();
})
```
Now add search and add_friend methods to users_controller.rb file:
```ruby
def search
    @users = User.search(params[:search_param])

    if @users
      @users = current_user.except_current_user(@users)
      render partial: "friends/lookup"
    else
      render status: :not_found, nothing: true
    end
  end

  def add_friend
    @friend = User.find(params[:friend])
    current_user.friendships.build(friend_id: @friend.id)

    if current_user.save
      redirect_to my_friends_path, notice: "Friend was successfully added"
    else
      redirect_to my_friends_path, flash[:error] = "There was an error with adding user as friend"
    end
  end
```