Now that we have the id's and the form that submits via ajax, we want to handle the return of that ajax action so we create a stocks.js file in app/assets/javascripts folder
```javascript
var init_stock_lookup;

init_stock_lookup = function() {
    $('#stock-lookup-form').on('ajax:before', function(event, data, status){
        show_spinner();
    });
    $('#stock-lookup-form').on('ajax:after', function(event, data, status){
        hide_spinner();
    });
    $('#stock-lookup-form').on('ajax:success', function(event, data, status){
        $('#stock-lookup').replaceWith(data);
        init_stock_lookup();
    });
}


$(document).ready(function() {
    init_stock_lookup();
});
```
You need to add the init_stock_lookup(); again since the listeners are gone once you replace with the data that's returned so you have to re-initialize it.

Now go to my_portfolio URL from the browser and test out a few stock symbols

Next create a custom.css.scss file under app/assets/stylesheets folder and add some styling:
```css
.results-block {
    display: inline-block;
}
```
Now next thing we want to handle are errors, add the following to stocks.js file above the closing } for the init_stock_lookup = function()

```javascript
$('#stock-lookup-form').on('ajax:error', function(event, xhr, status, error){
    $('#stock-lookup-results').replaceWith('');
    $('#stock-lookup-errors').replaceWith('Stock was not found.');
});
```
Add the following to lookup partial after the end statement and above the last div:

    <div id="stock-lookup-errors"></div>