# Client Cohort Analysis

This repository contains the code and dataset used to build a client cohort chart for analyzing customer transactions. The dataset was generated using R and the cohort chart was created using a combination of R and Excel.

## Table of Contents
- [Dataset Generation](#dataset-generation)
- [Cohort Chart Creation](#cohort-chart-creation)
- [How to Use](#how-to-use)
- [Contributing](#contributing)
- [License](#license)

## Dataset Generation

The dataset was generated using R. The following script creates a dataset of 30,000 random transactions with customer IDs, transaction dates, and transaction amounts.

### Code to Generate Dataset

```r
# Load necessary libraries
library(dplyr)

# Set working directory to your desired path
setwd("C:/path/to/your/directory")

# Set seed for reproducibility
set.seed(42) 

# Function to generate random dates
random_dates <- function(start_date, end_date, n) {
  seq_dates <- seq.Date(as.Date(start_date), as.Date(end_date), by="day")
  sample(seq_dates, n, replace = TRUE)
}

# Parameters
n_records <- 30000 
start_date <- as.Date('2018-01-01')
end_date <- as.Date('2022-12-31')

# Generate random transaction dates
transaction_dates <- random_dates(start_date, end_date, n_records)

# Generate random customer IDs
customer_ids <- sample(1:5000, n_records, replace = TRUE)

# Generate random transaction amounts
transaction_amounts <- round(runif(n_records, 5, 500), 2)

# Create data frame
df <- data.frame(
  transaction_id = 1:n_records,
  customer_id = customer_ids,
  transaction_date = transaction_dates,
  transaction_amount = transaction_amounts
)

# Save to CSV
write.csv(df, "customer_transaction_data.csv", row.names = FALSE)

# Display first few rows of the dataframe
head(df)
