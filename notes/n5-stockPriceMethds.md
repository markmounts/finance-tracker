Now we will add the two class level methods to the stock.rb model under app/models and a third price method:

    def self.find_by_ticker(ticker_symbol)

        where(ticker: ticker_symbol).first

    end

    def self.new_from_lookup(ticker_symbol)

        looked_up_stock = StockQuote::Stock.quote(ticker_symbol)

        return nil unless looked_up_stock.name

        new_stock = new(ticker: looked_up_stock.symbol, name: looked_up_stock.name)

        new_stock.last_price = new_stock.price

        new_stock

    end

    def price

        closing_price = StockQuote::Stock.quote(ticker).close

        return "#{closing_price} (Closing)" if closing_price

        opening_price = StockQuote::Stock.quote(ticker).open

        return "#{opening_price} (Opening)" if opening_price

        'Unavailable'

    end

We are adding the 

    self. 

prior to the method name, because these methods are not tied to any objects or object lifecycle, 
we need to be able to use them without having any instances of a stock.