# Домашнее задание к занятию «SQL. Часть 2» - Кощеев Иван

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Ответ

<details>
  
```

SELECT 
    s.first_name AS staff_first_name,
    s.last_name AS staff_last_name,
    c.city,
    COUNT(cu.customer_id) AS customer_count
FROM store st
JOIN staff s ON st.manager_staff_id = s.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cu ON cu.store_id = st.store_id
GROUP BY st.store_id, s.first_name, s.last_name, c.city
HAVING COUNT(cu.customer_id) > 300;

```

![image1](https://github.com/SirSeoPro/11-04/blob/main/1.png)

</details>
  
### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Ответ

<details>
  
```

SELECT COUNT(*) AS films_longer_than_avg
FROM film
WHERE length > (SELECT AVG(length) FROM film);

```

![image2](https://github.com/SirSeoPro/11-04/blob/main/2.png)

</details>
  
### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
### Ответ

<details>
  
```

WITH monthly_payments AS (
    SELECT 
        DATE_FORMAT(payment_date, '%Y-%m') AS month,
        SUM(amount) AS total_amount
    FROM payment
    GROUP BY DATE_FORMAT(payment_date, '%Y-%m')
),
monthly_rentals AS (
    SELECT 
        DATE_FORMAT(rental_date, '%Y-%m') AS month,
        COUNT(*) AS rental_count
    FROM rental
    GROUP BY DATE_FORMAT(rental_date, '%Y-%m')
),
combined AS (
    SELECT 
        mp.month,
        mp.total_amount,
        mr.rental_count
    FROM monthly_payments mp
    JOIN monthly_rentals mr ON mp.month = mr.month
)
SELECT *
FROM combined
ORDER BY total_amount DESC
LIMIT 1;


```

![image3](https://github.com/SirSeoPro/11-04/blob/main/3.png)

</details>
