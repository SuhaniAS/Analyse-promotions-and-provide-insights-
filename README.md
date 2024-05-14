# Analyse-promotions-and-provide-insights-to-sales-director
#
## Ad-hoc-requests
### 1. Provide a list of products with a base price greater than 500 and that are featured in promo type 'BOGOF' (Buy One Get One Free). This information will help us identify high-value products tath are currently being heavily discounnted, which can be useful for evaluating our pricing and promotion strateies.
#### Code: 
#### select fact_events.product_code, dim_products.product_name,fact_events.base_price
#### from fact_events
#### inner join dim_products
#### on fact_events.product_code = dim_products.product_code
#### where base_price>500 and promo_type='BOGOF'
#### GROUP BY product_code
#### order by product_code asc;
