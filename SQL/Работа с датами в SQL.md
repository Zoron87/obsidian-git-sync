--извлекаем из даты год, квартал, месяц, неделя, день c помощью extract  
```sql
select   
order_date,  
extract(year from order_date),  
extract(quarter from order_date),  
extract(month from order_date),  
extract(week from order_date),  
extract(day from order_date)  
from orders o;
```

--извлекаем из даты год, квартал, месяц, неделя, день c помощью date_part  
```sql
select   
order_date,  
date_part('year', order_date),  
date_part('quarter', order_date),  
date_part('month', order_date),  
date_part('week', order_date),  
date_part('day', order_date)  
from orders o;
```

--trunc  
```sql
select   
order_date,  
date_trunc('year', order_date),  
date_trunc('quarter', order_date),  
date_trunc('month', order_date),  
date_trunc('week', order_date),  
date_trunc('day', order_date)  
from orders o;
```

--текущий день, текущее время, текущий день и время  
```sql
select current_date, current_time, current_timestamp, now();
```

--извлекаем часы/минуты  
```sql
select   
extract(hour from now()),  
extract(minute from now());
```

--cчитаем разницу дней между датами  
```sql
select shipped_date , order_date   
,shipped_date - order_date  
from orders o   
limit 100;
```
  
--Задача 1  
--Посчитать кол-во покупок по месяцам

--вариант 1  
```sql
select   
date_part('year', order_date) as y,  
date_part('month', order_date) as m,  
count(o.order_id)  
from orders o   
group by   
date_part('year', order_date),  
date_part('month', order_date)  
order by y, m;
```

--вариант 2  
```sql
select   
date_trunc('month', order_date) as m,  
count(o.order_id)  
from orders o   
group by   
date_trunc('month', order_date)  
order by m;
```

--Задача 2  
--оставить заказы у которых с момента заказа до момента доставки прошло не более 31 дня

```sql
select o.order_id , o.order_date , o.shipped_date   
from orders o;
```

```sql
select o.order_id , o.order_date , o.shipped_date ,o.shipped_date-o.order_date   
from orders o  
where (o.shipped_date-o.order_date)<=31;
```

```sql
select count(*)  
from orders o  
where (o.shipped_date-o.order_date)<=31;
```