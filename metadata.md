
注意，数据库中，一般所有关系的name属性都是要求允许null的，因为我们设定的，在逻辑上删除某个元组的操作不是在数据库层面将这个元组删除，而是将其name项（若含有外键等引用，也同样）设置为null值，而在搜索的时候会略过这些null值，同时，新建操作会先搜索id号最小的，name项为空的元组，将信息覆盖其上作为新建操作，如果么有，则直接沿着id顺延创建

## AUTHORS（作者表）

- **表名与用途**：`authors`，存储图书作者的基本信息。
    
- **字段（含数据类型）**
    
    - `author_id` PK INTEGER UNIQUE（自增序列），用于区分作者的主键id
    - `author_name` VARCHAR 作者姓名，用字符串表示
    - `author_info` TEXT  作者的介绍，用文本段保存
    - `author_sex` VARCHAR 作者的性别，使用字符串表示，但是一般情况下，只有male 和female
- **主键/外键关系**
    - **PK**：`author_id`
    - **FK**：无（但被 `books.author_id` 引用）
- **重要约束**
    
    - `author_id` **NOT NULL**（主键）
        

---

## BOOKS（图书表）

- **表名与用途**：`books`，存储馆藏图书的核心信息（书名、作者、分类、出版社、供应商、库存、状态等）。
    
- **字段（含数据类型）**
    
    - `book_id` PK INTEGER UNIQUE自增序列）
        
    - `title` VARCHAR 记录书名
        
    - `author_id` INTEGER 外键，表示作者id
        
    - `category_id` INTEGER外键，表示类型id
        
    - `publisher_id` INTEGER 外键，表示出版社id
        
    - `supplier_id` INTEGER 外键，表示供应商id
        
    - `isbn` VARCHAR 每个书独特的isbn码，在存在多本同样书的情况下，尽管主键不同，但还能通过相同的isbn分辨出同样的书，所以我们没有给isbn设置unique约束
        
    - `status` BOOLEAN 布尔值来表示这个书籍的可用性（可用/被借）
        
- **主键/外键关系**
    
    - **PK**：`book_id`
        
    - **FK**：
        
        - `author_id` → `authors(author_id)` 
            
        - `category_id` → `categories(category_id) `
            
        - `publisher_id` → `publishers(publisher_id)`
            
        - `supplier_id` → `suppliers(supplier_id)`
            
- **重要约束**
    
    - `book_id` **NOT NULL**（主键）
        
    - `quantity` **NOT NULL**
        
    - `status` **NOT NULL**
        

---

## CATEGORIES（分类表）

- **表名与用途**：`categories`，存储图书分类（类别名称与描述）。
    
- **字段（含数据类型）**
    
    - `category_id` INTEGER（自增序列） 用于区分每个类型的独特主键 id
        
    - `name` VARCHAR 这个类型的名称
        
    - `description` TEXT  这个类型的描述文本
        
- **主键/外键关系**
    
    - **PK**：`category_id`
        
    - **FK**：无（但被 `books.category_id` 引用）
        
- **重要约束**
    
    - `category_id` **NOT NULL**（主键）
        
    - `name` **NOT NULL**
        

---

## PUBLISHERS（出版社表）

- **表名与用途**：`publishers`，存储出版社信息。
    
- **字段（含数据类型）**
    
    - `publisher_id` INTEGER（自增序列）
        
    - `publisher_name` VARCHAR
        
    - `publisher_info` TEXT
        
    - `publisher_add` VARCHAR
        
- **主键/外键关系**
    
    - **PK**：`publisher_id`
        
    - **FK**：无（但被 `books.publisher_id` 引用）
        
- **重要约束**
    
    - `publisher_id` **NOT NULL**（主键）
        
    - `publisher_name` **NOT NULL**
        

---

## SUPPLIERS（供应商表）

- **表名与用途**：`suppliers`，存储供应商信息（名称、地址、状态、日期等）。
    
- **字段（含数据类型）**
    
    - `supplier_id` INTEGER（自增序列）  区分供应商的主键 id
        
    - `supplier_add` VARCHAR 供应商的地址
        
    - `supplier_name` VARCHAR 供应商姓名
        
    - `supplier_status` VARCHAR 供应商的状态 主要是是否合作等
        
    - `supplier_date` CHAR 供应商加入的日期，用八位数字表示
        
- **主键/外键关系**
    
    - **PK**：`supplier_id`
        
    - **FK**：无（但被 `books.supplier_id` 引用）
        
- **重要约束**
    
    - `supplier_id` **NOT NULL**（主键）
        
    - `supplier_name` **NOT NULL**
        

---

## MEMBERS（会员表）

- **表名与用途**：`members`，存储图书馆会员信息与登录信息（用户名/密码等）。
    
- **字段（含数据类型）**
    
    - `member_id` INTEGER（自增序列） 区分每个会员的主键 id
        
    - `name` VARCHAR会员名称
        
    - `email` VARCHAR 会员邮箱
        
    - `phone` VARCHAR 会员的电话号码
        
    - `membership_date` CHAR 加入会员的时间
        
    - `membership_status` VARCHAR 当前会员的状态：是否有效
        
    - `username` VARCHAR 会员登录时的用户名，要求unique，因为要作为账号登录
        
    - `pwd` VARCHAR 会员的登录密码
        
- **主键/外键关系**
    
    - **PK**：`member_id` 
        
    - **FK**：无（但被 `loans.member_id` 引用）
        
- **重要约束**
    
    - `member_id` **NOT NULL**（主键）
        
    - `membership_date` **NOT NULL**
        
    - `membership_status` **NOT NULL**
        
    - `email` **UNIQUE**
        
    - `username` **UNIQUE**
        
        

---

## LOANS（借阅记录表）

- **表名与用途**：`loans`，记录会员借书/还书的业务流水（借阅日期、到期日期、归还日期、状态等）。
    
- **字段（含数据类型）**
    
    - `loan_id` INTEGER（自增序列） 用于标记每个独特借阅关系的主键 id
        
    - `book_id` INTEGER 外键，引用借阅的书的id
        
    - `member_id` INTEGER 外键，引用借阅的会员的id
        
    - `loan_date` CHAR 借阅日期 8位数字
        
    - `due_date` CHAR 应归还日期，8位数字在用户借阅界面，默认是借阅日期的两周后
        
    - `return_date` CHAR 归还日期8位数字，开始为空，当用户归还后，设置为系统时间
        
    - `status` boolean 当前借阅的状态，布尔值表示是否归还
        
- **主键/外键关系**
    
    - **PK**：`loan_id`
        
    - **FK**：
        
        - `book_id` → `books(book_id)`
            
        - `member_id` → `members(member_id)`
            
- **重要约束**
    
    - `loan_id` **NOT NULL**（主键）
        
    - `loan_date` **NOT NULL**
        
    - `due_date` **NOT NULL**
        
    - `status` **NOT NULL**
        
    - `return_date` 可为空（未归还时为空）
        

---

## STAFF（员工表）

- **表名与用途**：`staff`，存储系统工作人员信息（角色、联系方式等）。
    
- **字段（含数据类型）**
    
    - `staff_id` INTEGER（自增序列） 每个员工的独特id 主键
        
    - `name` VARCHAR 员工姓名
        
    - `role` VARCHAR 员工的职责
        
    - `email` VARCHAR 员工的邮箱
        
    - `phone` VARCHAR 员工的电话号码
        
- **主键/外键关系**
    
    - **PK**：`staff_id`
        
    - **FK**：无（当前结构未与其他表建立外键）
        
- **重要约束**
    
    - `staff_id` **NOT NULL**（主键）
        
    - `role` **NOT NULL**
        
    - `email` **UNIQUE**（允许为空，但填写后必须唯一）