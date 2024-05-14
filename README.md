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
![Screenshot 2024-05-14 102209](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/17f45582-f5ba-41a7-8e08-005e056bff70)
#
### 3. 
