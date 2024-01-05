
Consider a real-world application of a stock market analysis system. This system processes large sets of stock data to calculate various statistics and indicators in real-time. Initially, the system isn't designed with [[Concurrency Principles|concurrency]] in mind, leading to slow and inefficient processing. We'll refactor it to use concurrency, highlighting the benefits of this principle.

## Before Applying Concurrency Principles

### Initial System:

- `StockAnalyzer`: Processes stock data sequentially.
- `DataLoader`: Loads stock data.
- `StatisticsCalculator`: Computes statistics based on the stock data.

#### stock_analysis.py (Before)

```python
import time

class DataLoader:
    def load_data(self, symbol):
        print(f"Loading data for {symbol}")
        time.sleep(2)  # Simulate a delay in data loading
        return [10, 20, 30, 40, 50]  # Simplified data

class StatisticsCalculator:
    def calculate_average(self, data):
        if not data:
            return 0
        return sum(data) / len(data)

class StockAnalyzer:
    def __init__(self):
        self.loader = DataLoader()
        self.calculator = StatisticsCalculator()

    def analyze_stocks(self, symbols):
        for symbol in symbols:
            data = self.loader.load_data(symbol)
            average = self.calculator.calculate_average(data)
            print(f"Average for {symbol} is {average}")

# Usage
symbols = ["AAPL", "GOOGL", "MSFT"]
analyzer = StockAnalyzer()
start_time = time.time()
analyzer.analyze_stocks(symbols)
print(f"Time taken: {time.time() - start_time} seconds")
```

In this non-concurrent design, `StockAnalyzer` processes each stock sequentially, which is slow and inefficient for large datasets or a high number of stocks.

## After Applying Concurrency Principles

### Refactored System:

- Utilize Python's `threading` module to process stock data concurrently.
- Each stock is processed in a separate thread, improving the processing time.

#### stock_analysis.py (After)

```python
import time
import threading

class DataLoader:
    def load_data(self, symbol):
        print(f"Loading data for {symbol}")
        time.sleep(2)  # Simulate a delay in data loading
        return [10, 20, 30, 40, 50]  # Simplified data

class StatisticsCalculator:
    def calculate_average(self, data):
        if not data:
            return 0
        return sum(data) / len(data)

class StockAnalyzer:
    def __init__(self):
        self.loader = DataLoader()
        self.calculator = StatisticsCalculator()

    def analyze_stock(self, symbol):
        data = self.loader.load_data(symbol)
        average = self.calculator.calculate_average(data)
        print(f"Average for {symbol} is {average}")

    def analyze_stocks_concurrently(self, symbols):
        threads = []
        for symbol in symbols:
            thread = threading.Thread(target=self.analyze_stock, args=(symbol,))
            threads.append(thread)
            thread.start()

        for thread in threads:
            thread.join()

# Usage
symbols = ["AAPL", "GOOGL", "MSFT"]
analyzer = StockAnalyzer()
start_time = time.time()
analyzer.analyze_stocks_concurrently(symbols)
print(f"Time taken: {time.time() - start_time} seconds")
```

## Conclusion

In the refactored example:

- `StockAnalyzer` processes each stock in a separate thread, allowing for concurrent processing.
- The overall processing time is reduced significantly, especially for large numbers of stocks or large datasets.
- The system now adheres to concurrency principles, enhancing its efficiency and responsiveness.

This concurrent design significantly improves the performance and responsiveness of the stock market analysis system. It allows for faster processing of large datasets and can handle a higher number of stocks in real-time. The example emphasizes the benefits of adhering to concurrency principles in a real-world application, leading to a more efficient, responsive, and scalable system.