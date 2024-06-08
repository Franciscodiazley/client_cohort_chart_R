# Client Cohort Analysis

This repository provides the steps and code to generate a random dataset for client transactions and perform a cohort analysis to visualize customer retention over time.

## Table of Contents
1. [Introduction](#introduction)
2. [Dataset Generation](#dataset-generation)
3. [Cohort Analysis](#cohort-analysis)
4. [Dependencies](#dependencies)
5. [Usage](#usage)
6. [License](#license)

## Introduction
Cohort analysis is a type of behavioral analytics that breaks down data into related groups, which usually share common characteristics or experiences within a defined time span. This analysis helps in understanding user behavior and how it changes over time.

## Dataset Generation
The dataset is generated using R, with random dates, customer IDs, and transaction amounts to simulate real transaction data.

### Code to Generate Dataset
```r
# Load necessary libraries
library(dplyr)

# Set seed for reproducibility
set.seed(42) # This ensures that the random numbers generated are the same each time the code is run

# Function to generate random dates
random_dates <- function(start_date, end_date, n) {
  seq_dates <- seq.Date(as.Date(start_date), as.Date(end_date), by="day")
  sample(seq_dates, n, replace = TRUE)
}

# Parameters
n_records <- 30000 # Number of records to generate
start_date <- as.Date('2018-01-01') # Start date for transaction data
end_date <- as.Date('2022-12-31') # End date for transaction data

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
```
## Client cohort chart
This section describes the steps and code to perform cohort analysis on the generated dataset.

### Code to Generate the cohort chart
```r
# Load necessary libraries
install.packages("dplyr")
install.packages("lubridate")
install.packages("ggplot2")
install.packages("readxl")
install.packages("tidyr")

library(dplyr)
library(lubridate)
library(ggplot2)
library(readxl)
library(tidyr)

# Step 2: Load the Data from Excel
data <- read_excel("customer_transaction_data.xlsx")

# Ensure transaction_date is in date format
data$transaction_date <- as.Date(data$transaction_date, format = "%d.%m.%Y")

# Create acquisition_date which is the first purchase date for each customer
data <- data %>%
  group_by(customer_id) %>%
  mutate(acquisition_date = min(transaction_date)) %>%
  ungroup()

# Create cohort_quarter based on acquisition_date
data$cohort_quarter <- paste0(year(data$acquisition_date), "-Q", quarter(data$acquisition_date))

# Create purchase_quarter based on transaction_date
data$purchase_quarter <- paste0(year(data$transaction_date), "-Q", quarter(data$transaction_date))

# Step 4: Create Cohort Data
cohort_data <- data %>%
  group_by(cohort_quarter, purchase_quarter) %>%
  summarise(customers = n_distinct(customer_id)) %>%
  ungroup()

# Calculate the quarters since acquisition
cohort_data <- cohort_data %>%
  mutate(quarters_since_acquisition = as.numeric(factor(purchase_quarter)) - as.numeric(factor(cohort_quarter)))

# Step 5: Create the Cohort Analysis Table
cohort_table <- cohort_data %>%
  pivot_wider(names_from = quarters_since_acquisition, values_from = customers)

# Print the cohort table
print(cohort_table)

# Visualize the Cohort Analysis with purchase_quarter on the x-axis and cohort_quarter on the y-axis
ggplot(cohort_data, aes(x = purchase_quarter, y = cohort_quarter, fill = customers)) +
  geom_tile(color = "white") +
  scale_fill_gradient(low = "white", high = "blue") +
  labs(title = "Customer Cohort Analysis by Quarters", x = "Purchase Quarter", y = "Cohort Quarter") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) # Rotate x-axis labels for better readability

```

## Dependencies
 To run the provided scripts, you need the following R packages:

- dplyr
- lubridate
- ggplot2
- readxl
- tidyr

## Usage

Clone this repository to your local machine.
Set your working directory to the location where you cloned the repository.
Run the R script to generate the dataset.
Run the cohort analysis script to create and visualize the cohort chart.
License
This project is licensed under the MIT License.
```csharp
You can copy and paste this entire content into a `README.md` file in your repository.
```
