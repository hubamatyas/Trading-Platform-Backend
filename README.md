# Trading-Platform-Backend
Backend framework for logging and querying millions of transactions.

To store information contained in a transaction and to efficiently implement the TradingBackend API a combination of non-linear and linear data structures and algorithms were used. The final application, as shown below, consists of three data structure layers to logically distribute the millions of transactions every day.

![Data structures chart](https://github.com/hubamatyas/Trading-Platform-Backend/blob/main/chart.png)
 
### Hash Table - Layer 1
Drawing from the divide and conquer technique, a Hash Table (company_transactions), where keys are company names and values are binary search trees, is used first to separate transactions based on the twelve companies. Although a Hash Table does not guarantee it, built-in Python, and other extensively tested hash functions and collision management techniques provide constant look-up and insertion time in most cases. Ο(1) insertion also makes the application easily extendable by simply adding a new company-tree pair to the Hash Table. Additionally, because neither guaranteed constant look-up time nor ordered operations on company names are required on the stock trading platform, a Hash Table was seen as the most appropriate data structure for the first layer.

### Red-Black BST - Layer 2
The second layer (i.e., values in company_transactions), where the algorithms supporting the API functions are realised, consists of Left-leaning Red Black Binary Search Trees (TransactionTree). Each company has its own TransactionTree. As a result of functions defined in the brief only requiring transactions to be ordered based on their trade values and thanks to transactions being already categorised by company_transactions, the design decision was made to set the trade values as keys. Values in the tree are the list of transactions with the same trade value As a self-balancing BST, TransactionTree supports every ordered operation (e.g., floor, ceiling) outlined in the brief by recursively halving the number of remaining keys to be looked at until a certain key is found. This allows for guaranteed logarithmic time complexity, ideal for handling hundreds of thousands of transactions.

### List - Layer 3
The third and final layer (i.e., values in TransactionTree) is a simple list of transactions. A list is required instead of storing single transactions as it is possible to have multiple transactions with the same trade value for a particular company. In other words, keys in TransactionTree may not be unique, however, this anomaly can be supported by the use of lists as values in the tree. It can not only be supported but is an essential part of the API that ensures every transaction is returned when two or more transactions with the same trade value for the same company are recorded. To implement the ideas of encapsulation and information hiding, each transaction is stored as a Transaction object. The objects store the information (name, price, quantity, date) in instance variables and are accessible via getter functions.

# Framework Testing on Colab
```
Execution time to load 1000000 transactions: 29.1113

Mean execution time sorted_transactions: 0.057768446

Mean execution time min_transactions: 0.0

Mean execution time max_transactions: 0.0

Mean execution time floor_transactions: 0.0

Mean execution time ceiling_transactions: 0.0

Mean execution time range_transactions: 0.036049755
```
