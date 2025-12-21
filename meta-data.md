### DATABASE CONSTRAINTS AND TABLE DESIGNS

In databases, it is generally required that the `name` attribute of all relations (tables) allow `NULL`. This is because, logically, when a tuple is deleted, the operation doesn't physically remove the tuple from the database but sets its `name` attribute (and any related foreign keys) to `NULL`. When searching, these `NULL` values are ignored, and when a new entry is created, the system first checks for tuples with the smallest `id` where the `name` is `NULL` and overwrites this data as part of the new entry. If no such tuple exists, new tuples are created sequentially by `id`.

---

## AUTHORS (Authors Table)

- **Table Name and Purpose**: `authors`, stores basic information about book authors.
    
- **Fields (with data types)**:
    
    - `author_id` PK INTEGER UNIQUE (auto-incremented sequence), the primary key to distinguish authors.
        
    - `author_name` VARCHAR, the author's name.
        
    - `author_info` TEXT, a description of the author.
        
    - `author_sex` VARCHAR, the author's gender, typically "male" or "female".
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `author_id`
        
    - **FK**: None (but referenced by `books.author_id`).
        
- **Important Constraints**:
    
    - `author_id` **NOT NULL** (Primary Key).
        

---

## BOOKS (Books Table)

- **Table Name and Purpose**: `books`, stores essential information about library books (title, author, category, publisher, supplier, stock, status, etc.).
    
- **Fields (with data types)**:
    
    - `book_id` PK INTEGER UNIQUE (auto-incremented sequence).
        
    - `title` VARCHAR, the book title.
        
    - `author_id` INTEGER, foreign key representing the author's id.
        
    - `category_id` INTEGER, foreign key representing the category's id.
        
    - `publisher_id` INTEGER, foreign key representing the publisher's id.
        
    - `supplier_id` INTEGER, foreign key representing the supplier's id.
        
    - `isbn` VARCHAR, the unique ISBN code for each book. In cases where multiple copies of the same book exist, even though the primary key differs, the same ISBN can still uniquely identify the book. Therefore, no unique constraint is applied to `isbn`.
        
    - `status` BOOLEAN, a boolean to indicate the book's availability (available/borrowed).
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `book_id`
        
    - **FK**:
        
        - `author_id` → `authors(author_id)`
            
        - `category_id` → `categories(category_id)`
            
        - `publisher_id` → `publishers(publisher_id)`
            
        - `supplier_id` → `suppliers(supplier_id)`
            
- **Important Constraints**:
    
    - `book_id` **NOT NULL** (Primary Key).
        
    - `quantity` **NOT NULL**.
        
    - `status` **NOT NULL**.
        

---

## CATEGORIES (Categories Table)

- **Table Name and Purpose**: `categories`, stores book categories (category name and description).
    
- **Fields (with data types)**:
    
    - `category_id` INTEGER (auto-incremented sequence), the primary key distinguishing each category.
        
    - `name` VARCHAR, the name of the category.
        
    - `description` TEXT, a description of the category.
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `category_id`
        
    - **FK**: None (but referenced by `books.category_id`).
        
- **Important Constraints**:
    
    - `category_id` **NOT NULL** (Primary Key).
        
    - `name` **NOT NULL**.
        

---

## PUBLISHERS (Publishers Table)

- **Table Name and Purpose**: `publishers`, stores information about publishers.
    
- **Fields (with data types)**:
    
    - `publisher_id` INTEGER (auto-incremented sequence).
        
    - `publisher_name` VARCHAR.
        
    - `publisher_info` TEXT.
        
    - `publisher_add` VARCHAR.
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `publisher_id`
        
    - **FK**: None (but referenced by `books.publisher_id`).
        
- **Important Constraints**:
    
    - `publisher_id` **NOT NULL** (Primary Key).
        
    - `publisher_name` **NOT NULL**.
        

---

## SUPPLIERS (Suppliers Table)

- **Table Name and Purpose**: `suppliers`, stores information about suppliers (name, address, status, date, etc.).
    
- **Fields (with data types)**:
    
    - `supplier_id` INTEGER (auto-incremented sequence), the primary key distinguishing each supplier.
        
    - `supplier_add` VARCHAR, the supplier's address.
        
    - `supplier_name` VARCHAR, the supplier's name.
        
    - `supplier_status` VARCHAR, the supplier's status, mainly whether they are currently cooperating.
        
    - `supplier_date` CHAR, the date the supplier joined, in 8-digit format.
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `supplier_id`
        
    - **FK**: None (but referenced by `books.supplier_id`).
        
- **Important Constraints**:
    
    - `supplier_id` **NOT NULL** (Primary Key).
        
    - `supplier_name` **NOT NULL**.
        

---

## MEMBERS (Members Table)

- **Table Name and Purpose**: `members`, stores information about library members and login details (username/password, etc.).
    
- **Fields (with data types)**:
    
    - `member_id` INTEGER (auto-incremented sequence), the primary key distinguishing each member.
        
    - `name` VARCHAR, the member's name.
        
    - `email` VARCHAR, the member's email.
        
    - `phone` VARCHAR, the member's phone number.
        
    - `membership_date` CHAR, the date the member joined.
        
    - `membership_status` VARCHAR, the current status of the membership: whether it's active or not.
        
    - `username` VARCHAR, the member's login username, must be unique as it is used for login.
        
    - `pwd` VARCHAR, the member's login password.
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `member_id`
        
    - **FK**: None (but referenced by `loans.member_id`).
        
- **Important Constraints**:
    
    - `member_id` **NOT NULL** (Primary Key).
        
    - `membership_date` **NOT NULL**.
        
    - `membership_status` **NOT NULL**.
        
    - `email` **UNIQUE**.
        
    - `username` **UNIQUE**.
        

---

## LOANS (Loan Records Table)

- **Table Name and Purpose**: `loans`, records the transactions of borrowing and returning books (loan date, due date, return date, status, etc.).
    
- **Fields (with data types)**:
    
    - `loan_id` INTEGER (auto-incremented sequence), the primary key for each loan transaction.
        
    - `book_id` INTEGER, foreign key referencing the borrowed book's id.
        
    - `member_id` INTEGER, foreign key referencing the borrowing member's id.
        
    - `loan_date` CHAR, the loan date in 8-digit format.
        
    - `due_date` CHAR, the due date, typically two weeks after the loan date.
        
    - `return_date` CHAR, the return date in 8-digit format, initially NULL until the book is returned.
        
    - `status` BOOLEAN, a boolean indicating the status of the loan (whether the book is returned or not).
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `loan_id`
        
    - **FK**:
        
        - `book_id` → `books(book_id)`
            
        - `member_id` → `members(member_id)`
            
- **Important Constraints**:
    
    - `loan_id` **NOT NULL** (Primary Key).
        
    - `loan_date` **NOT NULL**.
        
    - `due_date` **NOT NULL**.
        
    - `status` **NOT NULL**.
        
    - `return_date` can be NULL (if the book is not returned yet).
        

---

## STAFF (Staff Table)

- **Table Name and Purpose**: `staff`, stores information about system staff (roles, contact information, etc.).
    
- **Fields (with data types)**:
    
    - `staff_id` INTEGER (auto-incremented sequence), the primary key distinguishing each staff member.
        
    - `name` VARCHAR, the staff member's name.
        
    - `role` VARCHAR, the staff member's role.
        
    - `email` VARCHAR, the staff member's email.
        
    - `phone` VARCHAR, the staff member's phone number.
        
- **Primary Key / Foreign Key Relationships**:
    
    - **PK**: `staff_id`
        
    - **FK**: None (currently no foreign keys to other tables).
        
- **Important Constraints**:
    
    - `staff_id` **NOT NULL** (Primary Key).
        
    - `role` **NOT NULL**.
        
    - `email` **UNIQUE** (can be NULL, but must be unique if provided).
        

---

This translation retains the original structure and meaning of the content. Let me know if you need further adjustments or additional translations!