# Scalable E-commerce Data Processing with PySpark

This project simulates a real-world e-commerce data ecosystem and demonstrates how to process large-scale structured data using **PySpark**, **HDFS**, and **Spark SQL**.

It is designed to showcase **data engineering concepts** such as distributed processing, partitioning, joins, and performance optimization on multi-GB datasets.

---

## Overview

The dataset models a complete e-commerce workflow with multiple interconnected entities:

- Customers
- Orders
- Items
- Payments
- Shippings

The data is generated at multiple scales (from **1MB to 1.1GB**) to demonstrate how **Apache Spark handles distributed data processing** across different workloads.

---

## Tech Stack

- **PySpark (DataFrame API & Spark SQL)**
- **HDFS (Distributed Storage)**
- **Hive (Optional for querying)**
- **Python**

---

## Dataset Structure
ecommerce_data/
├── 1MB/
├── 10MB/
├── 150MB/
├── 300MB/
├── 500MB/
└── 1100MB/


Each folder contains:
- `customers.csv`
- `orders.csv`
- `items.csv`
- `payments.csv`
- `shippings.csv`

---

## Data Model

### Key Relationships:
- **Customers → Orders** (1:N)
- **Orders → Items** (1:N)
- **Orders → Payments** (1:1)
- **Orders → Shippings** (1:1)

This schema enables:
- Complex joins
- Aggregations (e.g., total revenue per customer)
- End-to-end pipeline simulation

---

## Key Features

### Distributed Data Processing
- Processed datasets up to **1.1GB** using Spark
- Demonstrated horizontal scaling with increasing data size

### Partitioning & Parallelism
- Leveraged **HDFS block size (128MB)** for partitioning
- Large files split into **multiple partitions (up to 8–9)** for parallel execution

### Spark Transformations
- Joins across multiple tables
- Aggregations (SUM, COUNT, GROUP BY)
- Filtering and data cleansing

### Real-world Simulation
- Modeled realistic customer distribution across India
- Simulated order lifecycle: placement → payment → shipping

---

## Sample PySpark Code

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("EcommerceDataProcessing") \
    .getOrCreate()

# Load datasets
customers = spark.read.option("header", "true").csv("path/to/customers.csv")
orders = spark.read.option("header", "true").csv("path/to/orders.csv")

# Join example
customer_orders = customers.join(orders, "customer_id")

# Aggregation example
customer_revenue = customer_orders.groupBy("customer_id") \
    .sum("total_amount")

customer_revenue.show()
