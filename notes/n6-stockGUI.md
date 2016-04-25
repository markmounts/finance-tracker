First add a route to your config/routes.rb file to search stocks:

    get 'search_stocks', to: 'stocks#search'

Now we add a stocks_controller.rb file under app/controllers and within it we define a search method. We are putting the search method in the stocks_controller because we want to have an easy way to search for stocks from our view, so we'll call it from there:
```ruby
class StocksController < ApplicationController

  def search
    if params[:stock]
      @stock = Stock.find_by_ticker(params[:stock])
      @stock ||= Stock.new_from_lookup(params[:stock])
    end

    if @stock
      #render json: @stock
      render partial: 'lookup'
    else
      render status: :not_found, nothing: true
    end
  end
end
```
To temporarily view output from the browser, code out the 

    render partial: 'lookup' 

line and add in the following line under it:

    render json: @stock

Then append the following to your root URL to view the results:

    search_stocks?stock=AAPL

    search_stocks?stock=GOOG

Now we will create the lookup partial, create a stocks folder under app/views folder and within it create a file named _lookup.html.erb

build the form_tag
```ruby
<div id="stock-lookup">
  <h3>Search for Stocks</h3>
  <%= form_tag search_stocks_path, remote: true, method: :get, id: 'stock-lookup-form' do %>
      <div class="form-group row no-padding text-center col-md-12">
        <div class="col-md-10">
          <%= text_field_tag :stock, params[:stock], placeholder: "Stock ticker symbol",
                              autofocus: true, class: 'form-control search-box input-lg' %>
        </div>
        <div class="col-md-2">
          <%= button_tag(type: :submit, class: "btn btnplg btn-success") do %>
              <i class="fa fa-search"></i> Look up a stock
          <% end %>
        </div>
      </div>
  <% end %>
</div>
```
Go to my_portfolio.html.erb under app/views/users and add the following line just to show the form:

    <%= render 'stocks/lookup' %>

Now lets add the rest of the code to the lookup partial under app/views/stocks folder.

So if we have @stock instance variable then we want to do something with it, add the code below under the end statement and above the last div tag:
```ruby
<% if @stock %>
    <div id="stock-lookup-results" class="well results-block">
      <strong>Symbol:</strong> <%= @stock.ticker %>
      <strong>Name:</strong> <%= @stock.name %>
      <strong>Price:</strong> <%= @stock.price %>
    </div>
<% end %>
```