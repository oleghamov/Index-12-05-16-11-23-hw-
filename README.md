# Домашнее задание к занятию "Индексы" - `Хамов Олег`

### Задание 1

    select ROUND(SUM(index_length)*100/SUM(data_length)) 'size index in %'
    from INFORMATION_SCHEMA.TABLES

### Задание 2

В представленной оконной функции есть столбец «f.title» из таблицы, которая

присоединена без условий, это не правильно и не дает результата в выводе. Происходит

только увеличение времени обработки такого запроса. Если в «select» ничего не

добавлять, то таблицу «film» и «inventory» необходимо убрать. Тогда, такой запрос,

вернет то же количество строк, что и первый, но за более короткое время 42ms вместо

52000ms. Кроме того можно не использовать «distinct» при применении «groupby». 

Теперь добавим индекс по столбцу «payment_date» таблицы «payment»:

    CREATE index payment_date on payment(payment_date);

и выполним оптимизированный запрос:

    select concat(c.last_name, ' ', c.first_name), sum(p.amount)

    from payment p

    inner join rental r on r.rental_date = p.payment_date

    inner join customer c on c.customer_id = r.customer_id

    where p.payment_date between '2005-07-30 00:00:00' and '2005-07-30 23:59:59'

    group by c.last_name, c.first_name, c.customer_id
