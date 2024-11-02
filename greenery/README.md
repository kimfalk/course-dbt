Welcome to your new dbt project!

### Using the starter project

Try running the following commands:
- dbt run
- dbt test

## Project 1 answers:

### On average, how many orders do we receive per hour? 
on average there are 8 orders pr hour

```
/* 
    select avg(dev_pr_hour)
    from (Select created_hour, count(1) as dev_pr_hour
    from 
    (Select TO_VARCHAR(CREATED_AT, 'dd-mon-yyyy hh') AS created_hour, order_id
    from dev_db.dbt_kimfalkjorgensengmailcom.stg_postgres__orders)
    group by created_hour) 
    limit 10
*/
```
### On average, how long does an order take from being placed to being delivered? 
On average 33, but it varies a lot between 1 and 718 days. 

```
/* 
select avg(delivery_time), min(delivery_time), max(delivery_time)
from (
Select order_id, created_at, estimated_delivery_at, DATEDIFF(day, created_at::DATE, estimated_delivery_at::DATE) as delivery_time
from dev_db.dbt_kimfalkjorgensengmailcom.stg_postgres__orders )
limit 10
*/
```
### How many users have only made one purchase? Two purchases? Three+ purchases? 


|1	|   25|
|2  |	28|
|3  |	34|
|4  |	20|
|5  |	10|
|6  |	2|
|7  |	4|
|8  |	1|


```
*/
```
select num_orders, count(1)
from (
Select u.user_id, count(order_id) as num_orders
from dev_db.dbt_kimfalkjorgensengmailcom.stg_postgres__users u
join dev_db.dbt_kimfalkjorgensengmailcom.stg_postgres__orders o on u.user_id = o.user_id
group by u.user_id)
group by num_orders
order by num_orders
limit 10
*/
```

-- Note: you should consider a purchase to be a single order. In other words, if a user places one order for 3 products, they are considered to have made 1 purchase.

### Resources:
- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](https://community.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
