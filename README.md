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
For this project, the data cleaning portion is quite interesting as I have not done data cleaning on an retail dataset before. Therefore, the approach that I used in data cleaning is quite different here.

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

![image](https://user-images.githubusercontent.com/42713212/103103526-94a97080-465c-11eb-9866-4d17cad7fdb0.png)

![image](https://user-images.githubusercontent.com/42713212/103103532-9d9a4200-465c-11eb-9708-1d27a57cfcc6.png)

![image](https://user-images.githubusercontent.com/42713212/103103537-a559e680-465c-11eb-87d4-8f93e68ef383.png)

![image](https://user-images.githubusercontent.com/42713212/103103541-ad198b00-465c-11eb-862e-c1306380ef61.png)

![image](https://user-images.githubusercontent.com/42713212/103103550-ba367a00-465c-11eb-822b-8c70e3bbd43c.png)

![image](https://user-images.githubusercontent.com/42713212/103103557-c1f61e80-465c-11eb-835a-2afa47fc6532.png)

![image](https://user-images.githubusercontent.com/42713212/103103562-ca4e5980-465c-11eb-8de3-01dc1cea2d19.png)

![image](https://user-images.githubusercontent.com/42713212/103103582-e9e58200-465c-11eb-867f-0e164e0bae6e.png)
