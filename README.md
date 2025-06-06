-- Вибірка з усіх таблиць з INNER JOIN
SELECT 
    od.order_id,
    o.date,
    c.name AS customer_name,
    p.name AS product_name,
    cat.name AS category_name,
    e.first_name,
    s.name AS shipper_name,
    sup.name AS supplier_name
FROM order_details od
JOIN orders o ON od.order_id = o.id
JOIN customers c ON o.customer_id = c.id
JOIN products p ON od.product_id = p.id
JOIN categories cat ON p.category_id = cat.id
JOIN employees e ON o.employee_id = e.employee_id
JOIN shippers s ON o.shipper_id = s.id
JOIN suppliers sup ON p.supplier_id = sup.id;

-- P4_1: Підрахунок кількості рядків
SELECT COUNT(*) AS total_rows
FROM order_details od
JOIN orders o ON od.order_id = o.id
JOIN customers c ON o.customer_id = c.id
JOIN products p ON od.product_id = p.id
JOIN categories cat ON p.category_id = cat.id
JOIN employees e ON o.employee_id = e.employee_id
JOIN shippers s ON o.shipper_id = s.id
JOIN suppliers sup ON p.supplier_id = sup.id;

-- P4_2: LEFT JOIN замість INNER JOIN
SELECT COUNT(*) AS total_rows_with_left
FROM order_details od
LEFT JOIN orders o ON od.order_id = o.id
LEFT JOIN customers c ON o.customer_id = c.id
LEFT JOIN products p ON od.product_id = p.id
LEFT JOIN categories cat ON p.category_id = cat.id
LEFT JOIN employees e ON o.employee_id = e.employee_id
LEFT JOIN shippers s ON o.shipper_id = s.id
LEFT JOIN suppliers sup ON p.supplier_id = sup.id;

-- Відповідь: З LEFT JOIN ми отримаємо більше (або принаймні не менше) рядків, бо він залишає всі записи з лівої таблиці навіть без відповідності.

-- P4_3: Фільтр по employee_id > 3 AND ≤ 10
SELECT 
    od.order_id,
    o.date,
    e.employee_id,
    e.first_name,
    c.name AS customer_name
FROM order_details od
JOIN orders o ON od.order_id = o.id
JOIN employees e ON o.employee_id = e.employee_id
JOIN customers c ON o.customer_id = c.id
WHERE e.employee_id > 3 AND e.employee_id <= 10;

-- P4_4: Групування за категорією, підрахунок кількості та середньої кількості товарів
SELECT 
    cat.name AS category_name,
    COUNT(*) AS total_orders,
    AVG(od.quantity) AS avg_quantity
FROM order_details od
JOIN products p ON od.product_id = p.id
JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name;

-- P4_5: Тільки ті, де середня кількість товарів > 21
SELECT 
    cat.name AS category_name,
    COUNT(*) AS total_orders,
    AVG(od.quantity) AS avg_quantity
FROM order_details od
JOIN products p ON od.product_id = p.id
JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name
HAVING AVG(od.quantity) > 21;

-- P4_6: Сортування за кількістю записів (DESC)
SELECT 
    cat.name AS category_name,
    COUNT(*) AS total_orders
FROM order_details od
JOIN products p ON od.product_id = p.id
JOIN categories cat ON p.category_id = cat.id
GROUP BY cat.name
ORDER BY total_orders DESC;

-- P4_7: Пропустити перший запис, вивести 4 наступних
SELECT 
    od.order_id,
    o.date,
    c.name AS customer_name
FROM order_details od
JOIN orders o ON od.order_id = o.id
JOIN customers c ON o.customer_id = c.id
LIMIT 1, 4;
