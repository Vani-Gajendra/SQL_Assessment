# SQL_Assessment

/* Top 25 Schools */
SELECT email_domain, COUNT(user_id) as no_of_users
FROM users
GROUP BY email_domain
ORDER BY no_of_users DESC
LIMIT 25;

/* Number of Users located in New York City */
SELECT city, COUNT(user_id) as no_of_users FROM users
WHERE city = "New York";
 
 /* Number of users using mobile_app */
SELECT mobile_app, COUNT(user_id) as no_of_users FROM users WHERE mobile_app == 'mobile-user';

/* Sign up counts for each hour */
SELECT strftime('%H', sign_up_at), COUNT(*)
FROM users
GROUP BY 1;

 /* Differnt schools offering different courses */

WITH cpp AS (
SELECT u.email_domain, COUNT(*) AS cpp
FROM users u
JOIN progress p
ON u.user_id = p.user_id
WHERE p.learn_cpp = 'started' OR p.learn_cpp = 'completed'
GROUP BY 1
ORDER BY 1), 

sql AS ( 
SELECT u.email_domain, COUNT(*) AS sql
FROM users u
JOIN progress p
ON u.user_id = p.user_id
WHERE p.learn_sql = 'started' OR p.learn_sql = 'completed'
GROUP BY 1
ORDER BY 1),

html AS (
SELECT u.email_domain, COUNT(*) AS html
FROM users u
JOIN progress p
ON u.user_id = p.user_id
WHERE p.learn_html = 'started' OR p.learn_html = 'completed'
GROUP BY 1
ORDER BY 1),

js AS (
SELECT u.email_domain, COUNT(*) AS js
FROM users u
JOIN progress p
ON u.user_id = p.user_id
WHERE p.learn_javascript = 'started' OR p.learn_javascript = 'completed'
GROUP BY 1
ORDER BY 1),

java AS (
SELECT u.email_domain, COUNT(*) AS java
FROM users u
JOIN progress p
ON u.user_id = p.user_id
WHERE p.learn_java = 'started' OR p.learn_java = 'completed'
GROUP BY 1
ORDER BY 1)

SELECT c.email_domain, c.cpp, s.sql, h.html, j.js, i.java
FROM cpp c
JOIN sql s
ON c.email_domain = s.email_domain
JOIN html h
ON c.email_domain = h.email_domain
JOIN js j
ON c.email_domain = j.email_domain
JOIN java i
ON c.email_domain = i.email_domain;

/* Courses New York Students and Chicago Students Taking */

WITH students AS 
(
SELECT *
FROM users u
JOIN progress p
ON u.user_id = p.user_id
),

cpp AS 
(
SELECT city, COUNT(*) AS cpp FROM students WHERE (learn_cpp = 'completed' OR learn_cpp = 'started') AND (city = 'New York' OR city = 'Chicago') GROUP BY 1),

sql AS 
(
SELECT city, COUNT(*) AS sql FROM students WHERE (learn_sql = 'completed' OR learn_sql = 'started') AND (city = 'New York' OR city = 'Chicago') GROUP BY 1),

html AS
(
SELECT city, COUNT(*) AS html FROM students WHERE (learn_html = 'completed' OR learn_html = 'started') AND (city = 'New York' OR city = 'Chicago') GROUP BY 1),

js AS 
(
SELECT city, COUNT(*) AS js FROM students WHERE (learn_javascript = 'completed' OR learn_javascript = 'started') AND (city = 'New York' OR city = 'Chicago') GROUP BY 1),

java AS
(
SELECT city, COUNT(*) AS java FROM students WHERE (learn_java = 'completed' OR learn_java = 'started') AND (city = 'New York' OR city = 'Chicago') GROUP BY 1)

SELECT c.city, c.cpp, s.sql, h.html, j.js, jv.java
FROM cpp c
JOIN sql s
ON c.city = s.city
JOIN html h
ON c.city = h.city
JOIN js j
ON c.city = j.city
JOIN java jv
ON c.city = jv.city; 





