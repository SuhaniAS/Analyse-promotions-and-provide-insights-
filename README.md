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
### 3. Generate a report that displays each campaign along with the total revenue generated before and after the campaign? (Display the values in millions)
#### Code:
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
![Screenshot 2024-05-15 122217](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/e7804c22-060b-49cd-aaaf-eb5f18cd2494)
#
### 4. Produce a report that calculates the Incremental Sold Quantity(ISU%) for each category during the Diwali campaign. Additionally, provide rankings for the categories based on their ISU%. This information will assist in  assessing the category-wise success and impact of the Diwali  campaign on  incremental sales.
####  Code:

#### create table `isu%`
#### select category,campaign_id, 
#### (sum(`quantity_sold(after_promo)`)-sum(`quantity_sold(before_promo)`))*100/sum(`quantity_sold(before_promo)`) as `isu%`
#### from fact_events
#### join dim_products using (product_code)
#### where campaign_id='CAMP_DIW_01'
#### group by category;

#### select campaign_id,category,`isu%`,rank() over(order by `isu%` desc) as `rank_isu%`
#### from `isu%`;
![Screenshot 2024-05-17 210715](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/08a27201-7b50-4aa7-874c-c95e0d9aeff5)

 
#
### 5. Create a report featuring the top 5 products, ranked by Incremental Revenue Percentage(IR%),across all campaigns. The report will provide essential information including proudct name, category, and ir%. This analysis helps identify the most successful products in terms of incremental revenue across our campaigns, assisting in product optimization.
#### Code:
#### create table ir
#### select fact_events.product_code,dim_products.product_name,dim_products.category,
#### sum(fact_events.base_price*fact_events.`quantity_sold(before_promo)`) as ir_before_promo,
#### sum(
#### case
#### when promo_type='25% OFF' then base_price*(1-0.25)*`quantity_sold(after_promo)`
#### when promo_type='33% OFF' then base_price*(1-0.33)*`quantity_sold(after_promo)`
#### when promo_type='50% OFF' then base_price*(1-0.5)*`quantity_sold(after_promo)`
#### when promo_type='BOGOF' then base_price*`quantity_sold(after_promo)`
#### when promo_type='500 Cashback' then (base_price-500)*`quantity_sold(after_promo)`
#### end
#### ) as ir_after_promo
#### from retail_events_db.fact_events
#### join dim_products using (product_code)
#### group by product_code;

#### select*from `ir%`;

#### create table `ir%`
#### select product_name, category, (ir_after_promo-ir_before_promo)*100/ir_before_promo as `ir%`
#### from ir;

#### select product_name, category,`ir%`, rank() over(order by `ir%` desc) as `rank_ir%`
#### from `ir%`;
![Screenshot 2024-05-17 202018](https://github.com/SuhaniAS/Analyse-promotions-and-provide-insights-/assets/137792301/92c415f2-dfb3-4e4a-98b1-48f2ffdf8a8e)
