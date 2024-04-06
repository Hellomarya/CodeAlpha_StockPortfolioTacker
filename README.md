# CodeAlpha_StockPortfolioTacker
import requests

class StockPortfolio:
    def __init__(self):
        self.stocks = {}

    def add_stock(self, symbol, quantity):
        if symbol in self.stocks:
            self.stocks[symbol] += quantity
        else:
            self.stocks[symbol] = quantity

    def remove_stock(self, symbol, quantity):
        if symbol in self.stocks:
            self.stocks[symbol] -= quantity
            if self.stocks[symbol] <= 0:
                del self.stocks[symbol]
        else:
            print("Stock not found in portfolio.")

    def get_portfolio_value(self):
        total_value = 0
        for symbol, quantity in self.stocks.items():
            price = self.get_stock_price(symbol)
            total_value += price * quantity
        return total_value

    def get_stock_price(self, symbol):
        api_key = 'YOUR_API_KEY'
        url = f'https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol={symbol}&apikey={api_key}'
        response = requests.get(url)
        data = response.json()
        if 'Global Quote' in data:
            return float(data['Global Quote']['05. price'])
        else:
            print("Error fetching stock price.")
            return 0

# Example usage
portfolio = StockPortfolio()
portfolio.add_stock('AAPL', 10)
portfolio.add_stock('GOOGL', 5)
print("Portfolio value:", portfolio.get_portfolio_value())
