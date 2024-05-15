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
![Screenshot 2024-05-14 095320](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/2ce17ddb-e02c-401c-980a-f2447c1508e7)
# 
### 2. Generate a report that provides an overview of the number of stores in each city. The results will be stored in descending order of store counts, allowing us to identify the cities with the highest store presence. The report includes two essential fields: city and store count, which will assist in optimizing our retail operations.
#### Code:
#### select city, count(store_id)
#### from dim_stores
#### group by city
#### order by count(store_id) desc; 
![Screenshot 2024-05-14 102209](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/17f45582-f5ba-41a7-8e08-005e056bff70)    gh
#
### 3. Generate a report that displays each campaign along with the total revenue generated before and after the campaign? (Display the values in millions)
#### Code
#### select campaign_id,campaign_name, sum(base_price*`quantity_sold(before_promo)`)/1000000 as `total_revenue_before_promotion(m)`,
#### sum(
#### case
#### when promo_type='25% OFF' then base_price*(1-0.25)*`quantity_sold(after_promo)`
#### when promo_type='33% OFF' then base_price*(1-0.33)*`quantity_sold(after_promo)`
#### when promo_type='50% OFF' then base_price*(1-0.5)*`quantity_sold(after_promo)`
#### when promo_type='BOGOF' then base_price*`quantity_sold(after_promo)`
#### when promo_type='500 Cashback' then (base_price-500)*`quantity_sold(after_promo)`
#### end
#### )/1000000 as `total_revenue_after_promotion(m)` from retail_events_db.fact_events join dim_campaigns 
#### using (campaign_id)
#### group by campaign_id;
#
### 4.
