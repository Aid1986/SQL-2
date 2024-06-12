# Сурмач Андрей Андреевич
# SQL-2
# Задание_1
```
SELECT
	s.store_id,
	st.first_name,
	st.last_name,
	ct.city,
	cn.number_of_customers
FROM
	store s
INNER JOIN
	staff st
ON
	s.store_id = st.store_id
INNER JOIN 
	address a
ON
	s.address_id = a.address_id
INNER JOIN
	city ct
ON
	a.city_id = ct.city_id
INNER JOIN
	(
	SELECT
		store_id,
		COUNT(*) AS number_of_customers
	FROM
		customer c
	GROUP BY
		store_id) AS cn
ON
	s.store_id = cn.store_id
WHERE
	number_of_customers > 300;
```

![1](https://github.com/Aid1986/SQL-2/blob/main/1.png)

# Задание_2
```
SELECT
	COUNT(*)
FROM
	film f
WHERE
	length > (
	SELECT
		AVG(`length`)
	FROM
		film);
```
![2](https://github.com/Aid1986/SQL-2/blob/main/2.png)


# Задание_3
Предположим, что имеется в виду месяц любого года.
```
SELECT
	mp.month,
	mp.month_payments,
	mr.number_of_rentals
FROM
	(
	SELECT
		SUM(amount) AS month_payments,
		MONTH(payment_date) AS `month`
	FROM
		payment p
	GROUP BY
		MONTH(payment_date)
) AS mp
INNER JOIN 
(
	SELECT
		COUNT(*) AS number_of_rentals,
		MONTH(rental_date) AS `month`
	FROM
		rental r
	GROUP BY
		MONTH(rental_date)
) as mr
ON
	mp.month = mr.month
ORDER BY
	mp.month_payments DESC
LIMIT 1
;
```

![3](https://github.com/Aid1986/SQL-2/blob/main/3.png)


Теперь предположим, что год важен
```
SELECT
	mp.month,
	mp.month_payments,
	mr.number_of_rentals
FROM
	(
	SELECT
		SUM(amount) AS month_payments,
		DATE_FORMAT(payment_date, '%Y-%M') AS `month`
	FROM
		payment p
	GROUP BY
		DATE_FORMAT(payment_date, '%Y-%M')
) AS mp
INNER JOIN 
(
	SELECT
		COUNT(*) AS number_of_rentals,
		DATE_FORMAT(rental_date, '%Y-%M') AS `month`
	FROM
		rental r
	GROUP BY
		DATE_FORMAT(rental_date, '%Y-%M')
) as mr
ON
	mp.month = mr.month
ORDER BY
	mp.month_payments DESC
LIMIT 1
;
```


![3-1](https://github.com/Aid1986/SQL-2/blob/main/3-1.png)
