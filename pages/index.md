---
title: FashionCo Business Pulse
---

```kpis
select
    sum(oi.sale_price) as total_revenue,
    count(distinct o.order_id) as total_orders,
    sum(oi.sale_price) / count(distinct o.order_id) as aov,
    count(distinct u.id) as total_customers
from
    ecommerce_demo.orders o
    join ecommerce_demo.order_items oi on o.order_id = oi.order_id
    join ecommerce_demo.users u on o.user_id = u.id
```

```revenue_by_month
select
    date_trunc('month', o.created_at) as order_month,
    sum(oi.sale_price) as monthly_revenue
from
    ecommerce_demo.orders o
    join ecommerce_demo.order_items oi on o.order_id = oi.order_id
group by
    1
order by
    1
```

```customer_status
with customer_first_order as (
    select
        user_id,
        min(created_at) as first_order_date
    from
        ecommerce_demo.orders
    group by
        1
)
select
    date_trunc('month', o.created_at) as order_month,
    case
        when o.created_at = cfo.first_order_date then 'New'
        else 'Returning'
    end as customer_type,
    count(distinct o.user_id) as customer_count
from
    ecommerce_demo.orders o
    join customer_first_order cfo on o.user_id = cfo.user_id
group by
    1, 2
order by
    1, 2
```

## Key Metrics

<Grid cols=4>
<div>
<BigValue
    data={kpis}
    value=total_revenue
    fmt=usd
/>
</div>
<div>
<BigValue
    data={kpis}
    value=total_orders
/>
</div>
<div>
<BigValue
    data={kpis}
    value=aov
    fmt=usd
/>
</div>
<div>
<BigValue
    data={kpis}
    value=total_customers
/>
</div>
</Grid>

## Revenue Over Time

<LineChart
    data={revenue_by_month}
    x=order_month
    y=monthly_revenue
/>

## New vs. Returning Customers

<BarChart
    data={customer_status}
    x=order_month
    y=customer_count
    series=customer_type
/>
