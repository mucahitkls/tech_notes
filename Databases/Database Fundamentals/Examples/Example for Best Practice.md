
Let's consider a simplified example scenario for designing a database schema for a basic online bookstore. This bookstore requires a system to manage books, authors, and customer orders. Let's first look at a naive initial design and then apply best practices to improve it.
### Initial Design:

#### Tables:
1. **Books**: Contains all details about the books including author names and prices.
2. **Orders**: Contains customer orders with a list of books ordered.

#### Example Records:

- **Books Table**:
  - BookID: 1, Title: "The Great Gatsby", Author: "F. Scott Fitzgerald", Price: 10
  - BookID: 2, Title: "1984", Author: "George Orwell", Price: 15

- **Orders Table**:
  - OrderID: 1, CustomerName: "John Doe", BooksOrdered: "1,2", OrderDate: "2021-07-21"

### Applying Best Practices:

1. **Normalization**:
   - Split the **Books** table into two: **Books** and **Authors** to reduce redundancy.
   - Normalize the **Orders** table to handle multiple books per order properly.

2. **Use Clear Naming Conventions**:
   - Ensure table names are plural and column names are descriptive.

3. **Define Relationships and Constraints Clearly**:
   - Define primary keys and foreign keys explicitly.
   - Add constraints like NOT NULL where appropriate.

4. **Plan for Scalability**:
   - Consider the growing size of data and index frequently queried columns.

5. **Document the Schema**:
   - Create documentation for the table structure and relationships.

### Revised Design:

#### Tables:

1. **Books**:
   - BookID (PK)
   - Title
   - AuthorID (FK)
   - Price

2. **Authors**:
   - AuthorID (PK)
   - AuthorName

3. **Orders**:
   - OrderID (PK)
   - CustomerName
   - OrderDate

4. **OrderDetails**:
   - OrderDetailID (PK)
   - OrderID (FK)
   - BookID (FK)
   - Quantity

#### Example Records:

- **Books Table**:
  - BookID: 1, Title: "The Great Gatsby", AuthorID: 1, Price: 10
  - BookID: 2, Title: "1984", AuthorID: 2, Price: 15

- **Authors Table**:
  - AuthorID: 1, AuthorName: "F. Scott Fitzgerald"
  - AuthorID: 2, AuthorName: "George Orwell"

- **Orders Table**:
  - OrderID: 1, CustomerName: "John Doe", OrderDate: "2021-07-21"

- **OrderDetails Table**:
  - OrderDetailID: 1, OrderID: 1, BookID: 1, Quantity: 1
  - OrderDetailID: 2, OrderID: 1, BookID: 2, Quantity: 1

### Explanation of Improvements:

- **Normalization**: Separating the authors and books eliminates redundancy. Creating an **OrderDetails** table allows for multiple books per order and better management of order data.
- **Clear Naming**: Names are descriptive and consistent, making the schema easier to understand.
- **Relationships and Constraints**: Primary keys (PK) and foreign keys (FK) are defined. NOT NULL constraints are assumed for all PKs and crucial fields.
- **Scalability**: The normalized structure supports scaling, and indexing can be applied on frequently accessed fields like `BookID` in **OrderDetails** and `AuthorID` in **Books**.
- **Documentation**: While not shown here, documentation would include details of each table and their relationships, the purpose of each field, and any constraints applied.

By applying these best practices, the database schema is more organized, scalable, and robust, leading to better performance and easier maintenance.