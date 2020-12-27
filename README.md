# Online-Retail---Customer-Segmentation

## **Goal of Project**

The goal of this project is learn and perform customer segmentation on the online retail dataset, hopefully come up with important insights as to how the company can use this data to increase profits. In addition to that, understand the dataset and perform other analysis on the dataset along the way.

## **About the dataset**
The dataframe contains 8 attributes:
* Invoice: Invoice number. Nominal. A 6-digit integral number uniquely assigned to each transaction. If this code starts with the letter 'c', it indicates a cancellation.
* StockCode: Product (item) code. Nominal. A 5-digit integral number uniquely assigned to each distinct product.
* Description: Product (item) name. Nominal.
* Quantity: The quantities of each product (item) per transaction. Numeric.
* InvoiceDate: Invice date and time. Numeric. The day and time when a transaction was generated.
* Price: Unit price. Numeric. Product price per unit in sterling (Â£).
* CustomerID: Customer number. Nominal. A 5-digit integral number uniquely assigned to each customer.
* Country: Country name. Nominal. The name of the country where a customer resides.

## **Datacleaning**
For this project, the data cleaning portion is quite interesting as I have not done data cleaning on an retail dataset before. Therefore, the approach that I used in data cleaning is quite different here than other projects that I have done.

1) Checking for missing data
2) Duplicate entries 
3) **Negative Values**

Surprisingly, quantity has a negative number.

    negquantity = df[df['Quantity'] < 0]
    negquantity.head(5)
    

|  | Invoice  | StockCode | Description | Quantity | InvoiceDate | Price | Customer ID | Country |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 178 | C489449 | 22087 | PAPER BUNTING WHITE LACE | -12 | 2009-12-01 10:33:00 | 2.95 | 16321.0 | Australia |
| 179 | C489449 | 85206A | CREAM FELT EASTER EGG BASKET | -6 | 2009-12-01 10:33:00 | 1.65 | 16321.0 | Australia |
| 180 | C489449 | 21895 | POTTING SHED SOW 'N' GROW SET | -4 | 2009-12-01 10:33:00 | 4.25 | 16321.0 | Australia |
| 181 | C489449 | 21896 | POTTING SHED TWINE | -6 | 2009-12-01 10:33:00 | 4.25 | 16321.0 | Australia |
| 182 | C489449 | 22083 | PAPER CHAIN KIT RETRO SPOT | -12 | 2009-12-01 10:33:00 | 2.95 | 16321.0 | Australia |

We can observe that for negative quantity numbers, the invoice number contains the letter "C", which indicates a cancellation. Let's take alook closely at the cancelled  orders

4) **Cancelled Orders**

        Number of orders cancelled: 18390/797885 (2.30%) 
        
As you can see from the table above, we can see that the cancellations are made by the same person (Customer ID: C489449). I decide to see whether there are orders bought before from this customer with matching order details with exception of invoice number and invoiceDate. However, I realised that the previous orders for this particular customer may not be present in the dataset as the cancellation is made on 2009-12-01 which means that the order is most likely to be bought previously on a date earlier but the dataset only starts at 2009-12-01. The hypothesis proves to be the correct as the previous order is not found.

Despite this, we can still verify the cancellations that are made later than 2009-12-01. I looked at the 200th-240th wrong matching entries. The stock codes for the wrong matching are quite unexpected. Most of the StockCodes are expected(contains all numbers) but some of the stockCodes contains alphabets e.g. A, B, C, D, E, G, L, J, P, S, W which are located at the end of the StockCode. Upon closer inspection at the description, the alphabets are related to the product. For example, if you look at 233th-240th mismatch invoices, they all contain alphabets at the end of their stockCode and the alphabets in this case refers to the letters on the "BLING KEY RING".

    ----------Does not match----------
    StockCode                         90214G
    Description    LETTER "G" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15651, dtype: object
    ----------Does not match----------
    StockCode                         90214P
    Description    LETTER "P" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15652, dtype: object
    ----------Does not match----------
    StockCode                         90214E
    Description    LETTER "E" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15653, dtype: object
    ----------Does not match----------
    StockCode                         90214J
    Description    LETTER "J" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15654, dtype: object
    ----------Does not match----------
    StockCode                         90214S
    Description    LETTER "S" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15655, dtype: object
    ----------Does not match----------
    StockCode                         90214C
    Description    LETTER "C" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15656, dtype: object
    ----------Does not match----------
    StockCode                         90214A
    Description    LETTER "A" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695
    Name: 15657, dtype: object
    ----------Does not match----------
    StockCode                         90214L
    Description    LETTER "L" BLING KEY RING
    Quantity                              -6
    InvoiceDate          2009-12-07 14:37:00
    Price                               1.25
    Customer ID                        14695

To check that the cancellation is matched with a past order, i looked at specifically customer ID == 14695 where the stockCode ends with the letter "L".

    df.loc[(df["Customer ID"]==14695) & (df["StockCode"]=="90214L") ]
    
|  | Invoice  | StockCode | Description | Quantity | InvoiceDate | Price | Customer ID | Country | order_cancelled |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 15658 | C490700 | 90214L | LETTER "L" BLING KEY RING | -6 | 2009-12-07 14:37:00 | 1.25 | 14695.0 | United Kingdom | 1 |

As you can see, there is only one row that is outputted, which meant that the cancellation was not matched. We need to take into account this problem.

5) Discounted Orders
Some of the negative quanity invoices are discounted. We can see this in the description == "Discount".

    df_discount=df[df["Description"]=="Discount"]
    df_discount[0]
    
|  | Invoice  | StockCode | Description | Quantity | InvoiceDate | Price | Customer ID | Country | order_cancelled |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 735 | C489535 | D | Discount | -1 | 2009-12-01 12:11:00 | 9.00 | 15299.0 | United Kingdom | 1 |   
    
6) **Data cleaning for cancellation**

Since some of the cancellations are not matched, we are going to do datacleaning specifically to only those that matched. It is also important to note that for some of the orders that are not matched, it is most likely due to the fact that the order is placed before 2009-12-01 which is the date that the dataset starts.

To handle data cleaning, I will be having a function that checks for two cases:
1. a cancel order exists without counterpart
2. there's at least one counterpart with the exact same quantity

        entry_to_remove: 16168 (2.03%)
        doubtful_entry: 1942 (0.24%)
        
We will be dropping the cancellation orders with counterpart as it is justified. Since the doubtful entries (cancellation without counterpart) covers a small percentage (0.24%), we will also drop those entries too.

7) Removal of outliers

We determine the cut-off for identifying outliers as more than 3 standard deviations from the mean.

    entry_to_remove: 56 (0.01%)

## **Interesting EDA**
Below shows the top words that appear in the description.
![newplot (2)](https://user-images.githubusercontent.com/42713212/103145619-851f4a00-4778-11eb-9cfd-c295082274d5.png)

We are able to see that "set", "red", "bag", "heart" and "pink" are the most common words that appear in the description. However, a better approach will be to look at the top words that appear for respective months since some months have special holidays that may alter the consumer's spending patterns. Below show the top words that appear in the description for each month.

![image](https://user-images.githubusercontent.com/42713212/103147272-cd496700-478e-11eb-8905-eb43d1cae897.png)

As observed from the graphs, the top words changes as the months progressed. For example, in september, the word "christmas" suddenly appeared in the top 5 and rise to top 2 during october and november. It then dropped to 6th place in December. This is surprising as christmas is on the month of December and it contradicts the hypothesis that that sales for christmas related products will be the highest during the month. Therefore, the company can use this data to start promoting christmas related products in september since customers will start buying christmas products then. 

## **Customer Segmentation - Insights on results**
Customer Segmentation is done using the RFM principle. RFM stands for Recency, Frequency and Monetary value respectively.

* Recency(R): Days since last purchase
* Frequency(F): Total number of purchases
* Monetary value (M): Total money this customer spent

RFM is a model based on customer segmentation and it helps to divide customers into various categories or clusters to identify customers who are more likely to respond to promotions and also for future personalization services.

<insert graphs>
  
Here we will start segregating based on the percentile of the attribute. The attibutes are min_recency, frequency and monetary_value for 1. Recency, 2. Frequency and 3. Monetary value respectively. Based on the respective percentile, we will be allocating a score from 1 to 4. 1 would be the best score and 4 would be the worst.

* For 1. Recency, the smaller the min_recency, the more recent the last purchase is, and hence it is better. 

        -> min_recency (0-25th:1, 26-50th:2, 51-75th:3, 76-100th:4 percentile)

* For 2. Frequency and 3. Monetary value, the larger their respective attributes the better.

        -> 0-25th:4, 26-50th:3, 51-75th:2, 76-100th percentile:1 
        
<insert graphs>
  
![image](https://user-images.githubusercontent.com/42713212/103103708-e7cff300-465d-11eb-8dd3-8c25e1a3c9fe.png)

Source: Blast Analytics Marketing

![image](https://user-images.githubusercontent.com/42713212/103103505-780d3880-465c-11eb-9230-e4e71375c4ef.png)

It seems like the sales made by good customers is increasing from 2010 to 2011 comparatively across all months, which is a good sign. The most noticable would be the increase in sales would be 9/2011, 10/2011 and 11/2011 compared to 9/2010, 10/2010 and 11/2010. This trend is not present for all the other customer segments. Working with the marketing team would be useful in determining the reason for this improvement in sales and this info could be use to further increase sales made by good customers for the following years.

![image](https://user-images.githubusercontent.com/42713212/103103526-94a97080-465c-11eb-9866-4d17cad7fdb0.png)

There seems to be a drop in average customer monthly sales from 2010 to 2011, especially in the decrease in sales for 10/2011 and 11/2011 when compared to 10/2010 and 11/2010.

![image](https://user-images.githubusercontent.com/42713212/103103532-9d9a4200-465c-11eb-9708-1d27a57cfcc6.png)

The bad customer monthly sale dropped drastically at 12/2010. Since these are bad customers who are of low value, it may be a waste of resources for the marketing team to target the segment to increase in sales and it would be more efficient to target higher value customers.

![image](https://user-images.githubusercontent.com/42713212/103103537-a559e680-465c-11eb-87d4-8f93e68ef383.png)

It seems like the sales made by best customers is increasing from 2010 to 2011 comparatively across all months, which is a good sign. The most noticable would be the increase in sales would be 9/2011, 10/2011 and 11/2011 compared to 9/2010, 10/2010 and 11/2010. This trend is not present for all the other customer segments. Working with the marketing team would be useful in determining the reason for this improvement in sales and this info could be use to further increase sales made by best customers for the following years. This is very important as these customers are of the highest value.

![image](https://user-images.githubusercontent.com/42713212/103103541-ad198b00-465c-11eb-862e-c1306380ef61.png)

The loyal customer monthly sale dropped alittle from 2010 to 2011. It is most likely due to the poor marketing campaign targetted at the customer segment.

![image](https://user-images.githubusercontent.com/42713212/103103550-ba367a00-465c-11eb-822b-8c70e3bbd43c.png)

It seems like the sales made by big spenders customers is increasing from 2010 to 2011 comparatively across all months, which is a good sign. The most noticable would be the increase in sales would be 9/2011, 10/2011 and 11/2011 compared to 9/2010, 10/2010 and 11/2010. This trend is not present for all the other customer segments. Working with the marketing team would be useful in determining the reason for this improvement in sales and this info could be use to further increase sales made by big spenders customers for the following years. 

![image](https://user-images.githubusercontent.com/42713212/103103557-c1f61e80-465c-11eb-835a-2afa47fc6532.png)

Similar to the bad customer graph, the number of sales for almost lost customers dropped sharply in 2011. These customers are valuable. This is because even though they have not purchase for some time, they purchase frequently and spends the most. Therefore, it is important to focus on increasing the number of sales back.

![image](https://user-images.githubusercontent.com/42713212/103103562-ca4e5980-465c-11eb-8de3-01dc1cea2d19.png)

There is no data available for 2011 for this lost customer segment.

![image](https://user-images.githubusercontent.com/42713212/103103582-e9e58200-465c-11eb-867f-0e164e0bae6e.png)

There is also no data available for 2011 for this lost cheap customer segment.

## **Conclusion - Practical Implementations**

Overall, based on the analysis made for the different customer segments, I think the company should focus on these 3 customer segments more:

1. loyal customers: the monthly sales did not increase but instead dropped alittle for some months. Since these customers buy the most frequently, the dropped in monthly sales would most likely stem from poor marketing targetted at the customer segment. Improving the quality and quantity of marketing would most likely lead to high rewards.

2. Big spenders

3. Almost lost customers
