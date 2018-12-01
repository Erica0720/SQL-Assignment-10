# SQL-Assignment-10
use sakila;
SELECT  first_name, last_name
FROM actor;

SELECT concat( first_name, ' ', last_name) as "Actor Name"	
FROM actor;

SELECT actor_id
FROM actor
WHERE first_name = 'JOE';

SELECT*
FROM actor
WHERE last_name LIKE '%GEN%';

SELECT*
FROM actor
WHERE last_name LIKE '%LI%'
order by last_name;

select country, country_id
from country 
where country in ('Afghanistan', 'Bangladesh', 'China');

ALTER TABLE actor
ADD COLUMN description BLOB;

ALTER TABLE actor
DROP description; 

SELECT last_name, COUNT(last_name)
FROM actor
GROUP BY last_name; 

SELECT last_name, COUNT(last_name)
FROM actor
GROUP BY last_name
HAVING COUNT(last_name) > 1;

UPDATE actor 
SET first_name =
REPLACE( first_name, "GROUCHO", "HARPO");

UPDATE actor 
SET first_name =
REPLACE( first_name, "HARPO", "GROUCHO");

SELECT first_name, last_name, address
FROM address a
JOIN staff s 
ON (s.address_id = a.address_id);


SELECT staff.first_name, staff.last_name, SUM(payment.amount) AS 'Total Payment', payment.payment_date
FROM staff
INNER JOIN payment ON
staff.staff_id = payment.staff_id
WHERE payment.payment_date LIKE '%2005_08%';

SELECT film.title, film_actor.actor_id, COUNT(film_actor.actor_id) AS "number of actors"
FROM film
INNER JOIN film_actor ON film.film_id = film_actor.film_id
GROUP BY film.title;

SELECT COUNT(*)
FROM inventory
WHERE film_id
 IN(
	SELECT film_id
	FROM film 
	WHERE title = "Hunchback Impossible");

SELECT customer.last_name, customer.first_name, SUM(payment.amount)
FROM customer
INNER JOIN payment ON customer.customer_id = payment.customer_id
GROUP BY customer.last_name; 

SELECT * FROM  film WHERE language_id = 1 AND film.title  LIKE  'Q%' OR film.title LIKE 'K%'; 


SELECT last_name, first_name
FROM actor
WHERE actor_id in
	(SELECT actor_id FROM film_actor
	WHERE film_id in 
		(SELECT film_id FROM film
		WHERE title = "Alone Trip"));


SELECT country, last_name, first_name, email
FROM country c
LEFT JOIN customer cu
ON c.country_id = cu.customer_id
WHERE country = 'Canada';


SELECT title, category
FROM film_list
WHERE category = 'Family';


SELECT i.film_id, f.title, COUNT(r.inventory_id)
FROM inventory i
INNER JOIN rental r
ON i.inventory_id = r.inventory_id
INNER JOIN film_text f 
ON i.film_id = f.film_id
GROUP BY r.inventory_id
ORDER BY COUNT(r.inventory_id) DESC;


SELECT store.store_id, SUM(amount)
FROM store
INNER JOIN staff
ON store.store_id = staff.store_id
INNER JOIN payment p 
ON p.staff_id = staff.staff_id
GROUP BY store.store_id
ORDER BY SUM(amount);


SELECT s.store_id, city, country
FROM store s
INNER JOIN customer cu
ON s.store_id = cu.store_id
INNER JOIN staff st
ON s.store_id = st.store_id
INNER JOIN address a
ON cu.address_id = a.address_id
INNER JOIN city ci
ON a.city_id = ci.city_id
INNER JOIN country coun
ON ci.country_id = coun.country_id;
WHERE country = 'CANADA' AND country = 'AUSTRAILA';


SELECT name, SUM(p.amount)
FROM category c
INNER JOIN film_category fc
INNER JOIN inventory i
ON i.film_id = fc.film_id
INNER JOIN rental r
ON r.inventory_id = i.inventory_id
INNER JOIN payment p
GROUP BY name
LIMIT 5;


CREATE VIEW top_five_grossing_genres AS

SELECT name, SUM(p.amount)
FROM category c
INNER JOIN film_category fc
INNER JOIN inventory i
ON i.film_id = fc.film_id
INNER JOIN rental r
ON r.inventory_id = i.inventory_id
INNER JOIN payment p
GROUP BY name
LIMIT 5;


SELECT * FROM top_five_grossing_genres;


DROP VIEW top_five_grossing_genres
