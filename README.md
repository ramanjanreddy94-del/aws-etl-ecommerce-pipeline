import sys
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from pyspark.sql.functions import col

# Initialize Glue Context
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session

# Read data from S3
input_path = "s3://ecommerce-data-yourname /incoming/"
df = spark.read.json(input_path)

# Transform data
df = df.withColumn("total_price", col("price") * col("quantity"))

# Write to S3
output_path = "s3://ecommerce-data-yourname /processed/"
df.write.mode("overwrite").json(output_path)

print("ETL Job Completed Successfully")
