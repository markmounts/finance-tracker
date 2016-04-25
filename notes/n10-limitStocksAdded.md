Add rules to not allow adding after 10 stocks have been added or if a stock has already been added, add the 3 methods below to user.rb file
```ruby
def can_add_stock?(ticker_symbol)
  under_stock_limit? && !stock_already_added?(ticker_symbol)
end

def under_stock_limit?
  (user_stocks.count < 10)
end

def stock_already_added?(ticker_symbol)
  stock = Stock.find_by_ticker(ticker_symbol)
  return false unless stock
  user_stocks.where(stock_id: stock.id).exists?
end
```
So now in the lookup file wrap the link_to "Add to my stocks" option in an if clause:
```ruby
<% if current_user.can_add_stock?(@stock.ticker) %>
  <%= link_to "Add to my Stocks", user_stocks_path(user: current_user, stock_ticker: @stock.ticker,
      stock_id: @stock.id ? @stock.id : ''), class: 'btn btn-xs btn-success', method: :post %>
<% end %>
```
now we need to display an else clause to show why the stock cannot be added: add the following before the end
```ruby
<% else %>
  <span class='label label-default'>
    Stock cannot be added because you have already added
    <% if !current_user.under_stock_limit? %>
      10 stocks
    <% end %>
    <% if current_user.stock_already_added?(@stock.ticker) %>
      this stock
    <% end %>
  </span>
<% end %>
```
In your user_stocks_controller.rb file, remove the second stock from the line below within the create action:

    notice: "Stock #{@user_stock.stock.ticker} was successfully added" }