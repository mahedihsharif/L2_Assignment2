**1. What is PostgreSQL?**
Answer: PostgreSQL হল একটি শক্তিশালী, open-source, object-relational database management system. এটি ডেটা সংরক্ষণ, পরিচালনা এবং পুনরুদ্ধার করার জন্য ব্যবহৃত হয়। এটি ACID প্রপার্টি  Atomicity, Consistency, Isolation, Durability রুলস অনুসরণ করে, যার মানে হলো এটি অনেক বেশি নির্ভরযোগ্য এবং নিরাপদ।
এটি Oracle, MySQL, SQL Server-এর মতোই একটি RDBMS, কিন্তু অনেক বেশি feature-rich এবং customizable.

ডিটেইলস এর বর্ননা করছিঃ
ধরি, আপনি একটি Bookstore Website বানাচ্ছেন যেখানে বিভিন্ন বইয়ের তথ্য সংরক্ষণ করতে হবে।
PostgreSQL এর টেবিল তৈরি করা যাকঃ

CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(100),
    price DECIMAL(6, 2),
    published_date DATE
);

books নামের একটি টেবিল বানানো হয়েছে।

প্রতিটি বইয়ের একটি id, title, author, price এবং published_date আছে।

কিছু বই টেবিলে এর মধ্য ইন্সার্ট করলামঃ 
INSERT INTO books (title, author, price, published_date)
VALUES ('Think Like a Monk', 'Jay Shetty', 15.99, '2020-09-08');

সব বই দেখার কোয়েরিঃ
SELECT * FROM books;

এইভাবে ডিলেট, আপডেটও করা যায়।
