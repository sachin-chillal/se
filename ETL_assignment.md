## ETL Assignments

- 4 Easy Assignments
- 3 Medium Assignments
- 3 Hard Assignments

**`raw_event_data.json`**

This file contains unprocessed event logs, which is the perfect starting point for an ETL pipeline.

***

### ## ETL Assignment Ideas (Beginner to Advanced) 🚀

These assignments are designed to test various ETL skills by tackling the specific complexities within the provided JSON data.

### ### 🧑‍💻 Beginner: Basic Extraction and Cleaning

The goal here is to load the data and perform initial, high-level transformations.

1.  **Load and Parse**: Write a script that loads the JSON file into memory. For each event, parse the `raw_payload` string into a structured object or dictionary.
2.  **Standardize Keys**: Notice that some events use the key `source` while others use `source_system`. Create a new, clean dataset where this key is standardized to **`source_system`** for all records. Similarly, standardize `user_id` and `userId` to a single key.
3.  **Basic Data Counts**: After loading, perform simple aggregations:
    * Count the total number of events.
    * Count the number of events for each `event_type`.
    * Count how many records have a `null` value for `account_status`.
4.  **Flatten a Simple Nested Object**: Create a new version of the data where the `metadata` object is flattened. The keys `session_id`, `client_version`, and `tags` should become top-level fields in each event record.

***

### ### 👷 Intermediate: Data Normalization and Enrichment

This level focuses on creating clean, consistent, and usable data from the complex nested structures.

1.  **User Profile Normalization**: Create a function that processes the `user_data` object and returns a clean, flat user profile.
    * **Standardize Names**: Handle the two different name formats. If a record has a `name` field, split it into `first_name` and `last_name`. If it already has `firstName` and `lastName`, simply standardize the key names.
    * **Standardize Dates**: Parse the `dob` field, which has multiple formats (`YYYY-MM-DD`, `DD/MM/YYYY`, `Month Day, YYYY`), and convert them all to a single, standard format like **`YYYY-MM-DD`**.
    * **Validate and Clean**: Validate the `email` field using a regular expression. Add a new boolean field called `is_email_valid`.
    * **Type Casting**: Ensure the `postal_code` is always a string to handle cases where it's stored as a number.
2.  **Event Details Unpacking**: The structure of `event_details` changes based on the `event_type`. Write a transformation that creates a flat table of events. For the details, create specific columns like `product_id`, `price_amount`, `price_currency`, `page_url`, `login_ip_address`, etc. These columns will be `null` if the event is not of the relevant type.
3.  **Timestamp Conversion**: Parse the `timestamp` field and convert all timestamps to a standard **UTC** format. Create new columns for `event_date` and `event_time`.

***

### ### 🕵️ Advanced: Data Modeling and Loading for Analytics

This assignment simulates a real-world scenario where data is prepared and loaded into a data warehouse for business intelligence.

1.  **Design a Star Schema**: Model the data into a star schema suitable for analytics. This would involve creating separate, clean tables for **dimensions** and a central **fact** table.
    * **`dim_users` (Dimension)**: A table with one record per unique user. It should contain all the cleaned user profile information from the intermediate assignment (e.g., `user_key`, `user_id`, `first_name`, `last_name`, `email`, `dob`, `full_address`, `city`, `country_code`, `account_status`).
    * **`dim_date` (Dimension)**: A table with one record per day. It should contain columns like `date_key`, `full_date`, `year`, `month`, `day_of_week`.
    * **`dim_products` (Dimension)**: A table with unique products extracted from `purchase` events (`product_key`, `product_id`, `product_name`).
    * **`fct_events` (Fact Table)**: The central table containing one record per event. It would hold foreign keys to the dimension tables (`user_key`, `date_key`, `product_key`) and measures (numeric values) like `purchase_amount`, `quantity`, and `page_view_duration_seconds`.
2.  **Build the Full ETL Pipeline**: Write a script that performs all the necessary transformations and loads the data from the raw JSON file into the tables you designed in the star schema. This will involve generating unique keys (`user_key`, `date_key`, etc.).
3.  **Answer Analytical Questions**: Using your newly created schema, write queries to answer business questions:
    * What is the total purchase amount by users in each `country_code`?
    * Who are the top 5 most active users based on the number of events?
    * What is the average `page_view_duration_seconds` for users on `WebApp` versus `MobileApp_iOS`?
    * Identify users who have made a purchase but have an `invalid-email` address listed.
