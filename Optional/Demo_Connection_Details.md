## 1. external connections
### 1.2 AWS S3 
```py linenums="1"
- Name: Amazon S3 Connection
- Description: Amazon S3 bucket containing archived/historic customer data.
- Bucket: cpd-outcomes-s3
- Endpoint URL: https://s3.us-east-2.amazonaws.com
- Region: us-east-2
- Authentication method: Basic credentials
- Access key: AKIA2EJTDLF7QCA4EK4P
- Secret key: 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 (this field will only be visible once the access key has been entered)
```

### 1.3 Postgres
```py linenums="1"
- Name: Third Party Data
- Description: Database that contains third party data needed by the business for analytics and AI.
- Additional properties: CryptoProtocolVersion=TLSv1.2
- Database: 3RDPARTY
- Hostname or IP address: 85331fa6-6b56-4355-935e-290f3ac8aa8c.8117147f814b4b2ea643610826cd2046.databases.appdomain.cloud
- Port: 31128
- Username: cpdemo
- Password: C!oudP@k4DataDem0s
- Port is SSL-enabled: <Checked>
- SSL certificate: <Empty> (default after checking that the port is SSL-enabled)
```

### 1.4 Db2 Warehouse
```py linenums="1"
- Name: Data Warehouse
- Description: Database that contains enterprise data needed by the business for analytics and AI.
- Database: BLUDB
- Hostname or IP address: db2w-ovqfeqq.us-south.db2w.cloud.ibm.com
- Port: 50001
- Authentication method: Username and password
- Username: cpdemo
- Password: C!oudP@k4DataDem0s
- Port is SSL-enabled: <Checked> (default)
- SSL certificate: <Empty> (default)
```

## 2. Tables details

- CUSTOMER: Main customer details, including name, address, and credit card information. This table is hosted in a Db2 Warehouse relational database.
- CUSTOMER_LOYALTY: Customer loyalty and sales information from 2016 onward. This table is hosted in a PostgreSQL relational database.
- CUSTOMER_LOYALTY_HISTORY: Archived customer loyalty information from 2010-2015. This data is stored in a CSV file on Amazon S3 object storage.
	- QUANTITY_SOLD: Change type to INTEGER
	- SHIPPING_DAYS: Change type to INTEGER
	- SATISFACTION_RATING: Change type to INTEGER

- CUSTOMER & CUSTOMER_LOYALTY = CUSTOMER_SUMMARY_V
	- LOYALTY_NBR column of the CUSTOMER table and drag it to the corresponding LOYALTY_NBR row for the CUSTOMER_LOYALTY table. 

## 3. Join Tables in SQL
```py linenums="1"
SELECT "LOYALTY_NBR", "ORDER_YEAR", "QUARTER", "MONTHS_AS_MEMBER", "LOYALTY_STATUS", "PRODUCT_LINE", "COUPON_RESPONSE", "COUPON_COUNT", "QUANTITY_SOLD", "UNIT_SALE_PRICE", "UNIT_COST", "REVENUE", "PLANNED_REVENUE", "SHIPPING_DAYS", "CUSTOMER_LIFETIME_VALUE", "LOYALTY_COUNT", "BACKORDER_STATUS", "SATISFACTION_RATING", "SATISFACTION_REASON", "CUSTOMER_ID", "FIRST_NAME", "LAST_NAME", "CUSTOMER_NAME", "COUNTRY", "STATE_NAME", "STATE_CODE", "CITY", "LATITUDE", "LONGITUDE", "POSTAL_CODE", "GENDER", "EDUCATION", "LOCATION_CODE", "INCOME", "MARITAL_STATUS", "CREDIT_CARD_TYPE", "CREDIT_CARD_NUMBER", "CREDIT_CARD_CVV", "CREDIT_CARD_EXPIRY"
  FROM "ALEXANDER" . "IBMAS-CUSTOMER_VIEW";
```


```py linenums="1"
SELECT lh."LOYALTY_NBR", lh."ORDER_YEAR", lh."QUARTER", lh."MONTHS_AS_MEMBER", lh."LOYALTY_STATUS", lh."PRODUCT_LINE", lh."COUPON_RESPONSE", lh."COUPON_COUNT", lh."QUANTITY_SOLD", lh."UNIT_SALE_PRICE", lh."UNIT_COST", lh."REVENUE", lh."PLANNED_REVENUE", lh."SHIPPING_DAYS", lh."CUSTOMER_LIFETIME_VALUE", lh."LOYALTY_COUNT", lh."BACKORDER_STATUS", lh."SATISFACTION_RATING", lh."SATISFACTION_REASON", v."CUSTOMER_ID", v."FIRST_NAME", v."LAST_NAME", v."CUSTOMER_NAME", v."COUNTRY", v."STATE_NAME", v."STATE_CODE", v."CITY", v."LATITUDE", v."LONGITUDE", v."POSTAL_CODE", v."GENDER", v."EDUCATION", v."LOCATION_CODE", v."INCOME", v."MARITAL_STATUS", v."CREDIT_CARD_TYPE", v."CREDIT_CARD_NUMBER", v."CREDIT_CARD_CVV", v."CREDIT_CARD_EXPIRY"
FROM "ALEXANDER"."IBMAS-CUSTOMER_LOYALTY_HISTORY_csv" lh
JOIN "ALEXANDER"."IBMAS-CUSTOMER_VIEW" v ON lh."LOYALTY_NBR" = v."LOYALTY_NBR";
```
