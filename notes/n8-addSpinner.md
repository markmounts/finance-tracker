Test out the stock lookup functionality from your browser, if you're having difficulty pulling it up, you can try to disable turbolinks from the application.js file by simply removing the = from after the // and make it look like below:
```javascript
//= require jquery
//= require jquery_ujs
//= require twitter/bootstrap
// require turbolinks
//= require_tree .
```
Now lets add a spinner to make it look good while waiting to get search results

Add the following to the form inside the lookup partial right under the end statement and above the if @stock line:
```ruby
<%= render 'common/spinner' %>
```
Then under views folder create a folder named common and there create a _spinner.html.erb file with the following code:
```ruby 
<div id="spinner" class="row col-md-12 text-center spinner" style="display: none;">
   <i class='fa fa-spinner fa-spin fa-2x'></i> Processing...
</div>
```
Now you need to show the spinner using some javascript, so add the following code to application.js file under app/assets folder at the bottom
```javascript
var hide_spinner = function(){
    $('#spinner').hide();
}

var show_spinner = function(){
    $('#spinner').show();
}
```
Now we want to add code for show_spinner and hide_spinner in the stock.js file
```javascript
$('#stock-lookup-form').on('ajax:before', function(event, data, status){
    show_spinner();
});

$('#stock-lookup-form').on('ajax:after', function(event, data, status){
    hide_spinner();
});
```
You'll see that the spinner is spinning after you enter an invalid request, so add hide_spinner to your stocks.js file under the ajax:error event handler
```javascript
$('#stock-lookup-form').on('ajax:error', function(event, xhr, status, error){
    hide_spinner();
    $('#stock-lookup-results').replaceWith(' ');
    $('#stock-lookup-errors').replaceWith('Stock was not found.');
});
```
Now do a commit of your code, push to github and deploy to heroku and test out the stock lookup functionality