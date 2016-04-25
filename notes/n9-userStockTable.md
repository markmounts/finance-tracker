So now we will create a many to many association between users and stocks because a stock can have many users tracking it and and a user can track many stocks, run the following command to create this:

    rails generate scaffold UserStock user_id:integer stock_id:integer

Verify the migration file then run rake db:migrate to create the user_stocks table:

    rake db:migrate

in the user.rb model file add:

    has_many :user_stocks
    has_many :stocks, through: :user_stocks

in the stock.rb model file add:

    has_many :user_stocks
    has_many :users, through: :user_stocks

in the user_stocks.rb model file add:

    belongs_to :user
    belongs_to :stock

In your config/routes.rb file ensure you have the resources :user_stocks line, move it below devise_for :users and add in the following:

    resources :user_stocks, except: [:show, :edit, :update]

Now go to lookup partial and add the ability to add a stock to the user_stocks so they can be saved, add the following under the display of @stock.price (Price) within the if @stock branch:
```ruby
<%= link_to "Add to my Stocks", user_stocks_path(user: current_user, stock_ticker: @stock.ticker,
stock_id: @stock.id ? @stock.id : ''), class: 'btn btn-xs btn-success', method: :post %>
```
Add the ability to create a new UserStock for an existing stock or a new one, add this to the create action in user_stocks controller
```ruby 
def create

  if params[:stock_id].present?
    @user_stock = UserStock.new(stock_id: params[:stock_id], user: current_user)
  else
    stock = Stock.find_by_ticker(params[:stock_ticker])
    if stock
      @user_stock = UserStock.new(user: current_user, stock: stock)
    else
      stock = Stock.new_from_lookup(params[:stock_ticker])
      if stock.save
        @user_stock = UserStock.new(user: current_user, stock: stock)
      else
        @user_stock = nil
        flash[:error] = "Stock is not available"
      end
    end
  end

  respond_to do |format|
    if @user_stock.save
      format.html { redirect_to my_portfolio_path, 
                    notice: "Stock #{@user_stock.stock.ticker} stock was successfully added" }
      format.json { render :show, status: :created, location: @user_stock }
    else
      format.html { render :new }
      format.json { render json: @user_stock.errors, status: :unprocessable_entity }
    end
  end

end
```