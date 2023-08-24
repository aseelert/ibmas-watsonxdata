## 1. Demo Data 
## 1.1 CSV Demo data
https://ibm.ent.box.com/v/data-cars-csv
```sql linenums="1"
CREATE TABLE "cars" (
	"Car" VARCHAR,
	"MPG" INT,
	"Cylinders" INT,
	"Displacement" INT,
	"Horsepower" INT,
	"Weight" INT,
	"Acceleration" INT,
	"Model" INT,
	"Origin" VARCHAR
);
``````
## 1.2 Parquet data
https://ibm.ent.box.com/v/ontime-aircraft-id

## 2. external connections
### 2.1 AWS S3 
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

### 2.2 Postgres
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

### 2.3 Db2 Warehouse
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

## 3. Tables details

- (Db2) CUSTOMER: Main customer details, including name, address, and credit card information. This table is hosted in a Db2 Warehouse relational database.
- (Postgres) CUSTOMER_LOYALTY: Customer loyalty and sales information from 2016 onward. This table is hosted in a PostgreSQL relational database.
- (AWS S3) CUSTOMER_LOYALTY_HISTORY: Archived customer loyalty information from 2010-2015. This data is stored in a CSV file on Amazon S3 object storage.
	- QUANTITY_SOLD: Change type to INTEGER
	- SHIPPING_DAYS: Change type to INTEGER
	- SATISFACTION_RATING: Change type to INTEGER
