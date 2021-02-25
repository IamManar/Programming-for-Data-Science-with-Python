/* Qeury 1 */
SELECT first_name ||' '|| last_name AS actor_name,        
       COUNT(film_actor.film_id) AS number_of_films
FROM actor
JOIN film_actor 
ON actor.actor_id = film_actor.actor_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10;


/*Qeury 2 */
WITH avg_customer_amount AS (SELECT
                             customer.customer_id,
                             country.country AS country,
                             AVG(payment.amount) AS avg
                             FROM payment
                             JOIN customer
                             ON customer.customer_id = payment.customer_id
                             JOIN address 
                             ON customer.address_id = address.address_id
                             JOIN city
                             ON address.city_id = city.city_id
                             JOIN country ON city.country_id = country.country_id 
                             GROUP BY 1 , 2 )
SELECT country , AVG(avg) AS average_amount
FROM avg_customer_amount
GROUP BY 1
ORDER BY 2
Limit 10;

/*Query 3 */ 
WITH films_sales AS (SELECT store.store_id AS store, 
                  SUM(payment.amount) AS sales, 
                  inventory.film_id , 
                  film.title AS film_title
                  FROM store
                  JOIN staff 
                  ON store.store_id = staff.store_id
                  JOIN payment
                  ON payment.staff_id = staff.staff_id
                  JOIN rental 
                  ON rental.rental_id = payment.rental_id 
                  JOIN inventory 
                  ON inventory.inventory_id = rental.inventory_id
                  JOIN film 
                  ON inventory.film_id = film.film_id
                  GROUP BY 1 , 3 , 4)
SELECT film_title , sales
FROM films_sales
WHERE store = 1 
GROUP BY 1 , 2 
ORDER BY 2 DESC 
LIMIT 10; 

/* Query 4 */
SELECT payment_date,
       amount,
       LEAD (amount) over (order by payment_date) - amount AS Lead_difference
FROM payment
ORDER BY 1
LIMIT 100;
