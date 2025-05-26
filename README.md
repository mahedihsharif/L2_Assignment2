## **1. What is PostgreSQL?** <br>
**Answer:**<br> PostgreSQL হল একটি শক্তিশালী, open-source, object-relational database management system. এটি ডেটা সংরক্ষণ, পরিচালনা এবং পুনরুদ্ধার করার জন্য ব্যবহৃত হয়।
এটি ACID প্রপার্টি  Atomicity, Consistency, Isolation, Durability রুলস অনুসরণ করে, যার মানে হলো এটি অনেক বেশি নির্ভরযোগ্য এবং নিরাপদ।
এটি Oracle, MySQL, SQL Server-এর মতোই একটি RDBMS, কিন্তু অনেক বেশি feature-rich এবং customizable.

**ডিটেইলস এর বর্ননা করছিঃ**
ধরি, আপনি একটি Bookstore Website বানাচ্ছেন যেখানে বিভিন্ন বইয়ের তথ্য সংরক্ষণ করতে হবে।
PostgreSQL এর টেবিল তৈরি করা যাকঃ

`CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(100),
    price DECIMAL(6, 2),
    published_date DATE
);`<br>

books নামের একটি টেবিল বানানো হয়েছে, প্রতিটি বইয়ের একটি id, title, author, price এবং published_date আছে।

**কিছু বই টেবিলে এর মধ্য ইন্সার্ট করলামঃ**
`INSERT INTO books (title, author, price, published_date)
VALUES ('Think Like a Monk', 'Jay Shetty', 15.99, '2020-09-08');`<br>

সব বই দেখার কোয়েরিঃ
`SELECT * FROM books;`<br>

এইভাবে ডিলেট, আপডেটও করা যায়।



## **2. What is the purpose of a database schema in PostgreSQL?**<br>
**Answer:**<br> PostgreSQL এ Database Schema এর উদ্দেশ্য কী নিচে তা সুন্দর করে বর্ননা করা হলোঃ<br>
Schema হল একটি কাঠামো বা গঠন যেখানে আপনি টেবিল, ভিউ, ফাংশন, ইনডেক্স ইত্যাদি গুচ্ছভাবে সংগঠিত করে রাখতে পারেন। এটি PostgreSQL এ ডেটাবেইসকে সুন্দর ভাবে সংগঠিত এবং পৃথকভাবে পরিচালনা করার সুযোগ দেয়।

আরো সহজভাবে যদি বুজিয়ে বলিঃ <br>
ধরি, আপনি একটি University Management System তৈরি করছেন। সেখানে ৩টি আলাদা ডিপার্টমেন্ট আছে: Science, arts, commerce সব ডিপার্টমেন্টে students নামে টেবিল আছে।

যদি আপনি Schema ব্যবহার না করেন, তাহলে একই নামের টেবিল students ৩ বার তৈরি করা যাবে না। কিন্তু Schema ব্যবহার করলে আপনি আলাদা আলাদা students টেবিল রাখতে পারবেন এইভাবে: science.students
arts.students commerce.students

এটি সম্ভব হয়েছে কারণ প্রতিটি Schema আলাদা namespace তৈরি করে।

PostgreSQL Schema ব্যবহারের উদ্দেশ্য: <br>
1. ডেটাবেইসের টেবিল, ফাংশন, ভিউ ইত্যাদি গ্রুপ করে রাখা যায়।
2. প্রতিটি Schema-এর আলাদা Permission সেট করা যায়।
3. একই ডেটাবেসে একই নামের টেবিল/ভিউ রাখা সম্ভব হয়।
4. এক ডেটাবেজে একাধিক ক্লায়েন্টের ডেটা আলাদা আলাদা Schema-তে রাখা যায়।

Schema তৈরি:
`CREATE SCHEMA sales;`<br>

 Schema-এর মধ্যে টেবিল তৈরি:
`CREATE TABLE sales.customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);`<br>

ডেটা ইনসার্ট করা:
`INSERT INTO sales.customers (name, email)
VALUES ('Rahim', 'rahim@example.com');`<br>

ডাটা দেখার SCHEMA
`SELECT * FROM sales.customers;`<br>


## 3. Explain the Primary Key and Foreign Key concepts in PostgreSQL?<br>
   Answer:<br> PostgreSQL এ Primary Key এবং Foreign Key এই দুটি খুব গুরুত্বপূর্ণ ধারণা, বিশেষ করে ডেটাবেজ ডিজাইনে, নিচে বিস্তারিত আলোচনা করছি:

 **Primary Key কীঃ**<br>
 Primary Key হল এক বা একাধিক কলামের সমন্বয়ে গঠিত একটি constraint যা টেবিলের প্রতিটি row কে অন্য record থেকে আলাদা করে। এটি অবশ্যই ইউনিক হবে এবং null হওয়া চলবে না।

Primary Key --> Unique হয়, NULL value রাখা যায় না, প্রতিটি টেবিলে মাত্র একটি Primary Key থাকতে পারে,

**উদাহরণ:**<br>
`CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50)
);`
<br>

এখানে student_id কলামটি Primary Key, যার মানে হল প্রতিটি ছাত্রের একটি ইউনিক ID থাকবে এবং এই কলামে কখনো NULL value আসতে পারবে না।

**Foreign Key কীঃ**<br>
Foreign Key হল এমন একটি কলাম যা অন্য একটি টেবিলের Primary Key কে রেফারেন্স করে। এটি দুইটি টেবিলের মধ্যে সম্পর্ক তৈরি করে।

উদ্দেশ্য:<br>
দুটি টেবিলের মধ্যে রিলেশন তৈরি করা, ডেটা কনসিস্টেন্সি নিশ্চিত করা, এটি অন্য টেবিলের Primary Key বা Unique Key রেফার করে, ডেটা integrity রক্ষা করে, NULL থাকতে পারে

উদাহরণ:
`CREATE TABLE students (
    student_id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);`<br>


`CREATE TABLE results (
    result_id SERIAL PRIMARY KEY,
    student_id INT REFERENCES students(student_id),
    marks INT
);`<br>

এখানে results টেবিলের student_id কলামটি students টেবিলের student_id কে রেফার করছে। যে student_id গুলো students টেবিলে আছে, কেবল সেগুলোই results টেবিলে থাকতে পারবে।



## **4. What is the difference between the VARCHAR and CHAR data types?**<br>
   **Answer:**<br> VARCHAR এবং CHAR ডেটা টাইপের পার্থক্য নিচে বুজিয়ে বলছিঃ<br>
   

| বিষয় | CHAR | VARCHAR |
|:-----------|:------------:|------------:|
| দৈর্ঘ্য     |   পুরো নাম হলোঃ Character    |     পুরো নাম হলোঃ Variable Character  |
| স্পেস ব্যবহারে     |  স্থির দৈর্ঘ্য     |     পরিবর্তনশীল দৈর্ঘ্য  |
| পারফরম্যান্স     |   ছোট string দিলে পিছনে অতিরিক্ত স্পেস যোগ হয়     |    যতটুকু দরকার ততটুকুই জায়গা নেয়  |
| ডেটা স্টোরেজ     |   ছোট ফিক্সড-লেন্থ ফিল্ডে সামান্য ভালো পারফর্ম     |     সাধারণভাবে ভালো, বিশেষ করে বড় ডেটার জন্য  |
| ব্যবহার ক্ষেত্র     |   পুরো n পরিমাণ জায়গা দখল করে     |     যতটুকু লেখো, ঠিক ততটুকুই জায়গা নেয়।  |



**CHAR এর উদাহরণ:**<br>
`CREATE TABLE test_char (
    name CHAR(10)
);
`<br>
`INSERT INTO test_char (name) VALUES ('sharif');`<br>

**VARCHAR এর উদাহরণ:**<br>

`CREATE TABLE test_varchar (
    name VARCHAR(10)
);`<br>

`INSERT INTO test_varchar (name) VALUES ('sharif');`<br>


## **5. Explain the purpose of the WHERE clause in a SELECT statement?**<br>
**Answer:**<br> WHERE ক্লজ কী:
WHERE ক্লজ ব্যবহার করা হয় SQL SELECT statement এ এমনভাবে, যাতে ডেটাবেজ থেকে শুধু নির্দিষ্ট কিছু শর্ত পূরণ করা ডেটা বের করা যায়।
অর্থাৎ, WHERE হলো ফিল্টার করার একটি পদ্ধতি, যেটা বলে দেয় কোন শর্তে ডেটা আনা হবে। 
যেমনঃ

`SELECT column1, column2,
FROM table_name
WHERE condition;`<br>


**উদাহরণ:**<br> ১ টি নির্দিষ্ট নামের ব্যক্তি খুঁজে বের করাঃ

`SELECT * FROM person
WHERE name = 'Rahim';`<br>

এখানে personটেবিল থেকে শুধুমাত্র সেইসব রেকর্ড আনা হবে, যেখানে name হলো 'Rahim'।

উদাহরণ :<br> ৫০-এর বেশি মার্কস পাওয়া শিক্ষার্থীদের খুঁজে বের করা:

`SELECT * FROM results
WHERE marks > 50;`<br>

WHERE ক্লজ-এর উদ্দেশ্য:<br>
শর্ত অনুযায়ী ডেটা বের করা, প্রয়োজন ছাড়া অপ্রয়োজনীয় ডাটা না দেখানো, শুধু প্রয়োজনীয় ডেটা নিয়ে কাজ করলে কুয়েরি দ্রুত চলে, ডেটার নির্দিষ্ট অংশে গণনা করা সহজ হয়।

