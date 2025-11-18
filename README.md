# BUSINESS SCENARIO
`CoreTelecoms`, a leading telecom company in the US, is facing a major customer retention crisis. They are struggling with managing customer complaints and are losing customers as a result of this. Every day, thousands of customers complain about issues like poor network, incorrect billing, or bad customer service. These complaints come through different channels: 
- Social media
- Call center log files
- Website forms.

The management is frustrated. Data is stored in different formats. The reporting team manually compiles spreadsheets. No single pipeline exists for this data flow. Reports are delayed. Teams work in silos, e.t.c.

You have been brought in as a `Data Engineer` to design and implement a `Unified Customer Experience Data Platform ` that solves these problems. If your platform works, you would help the company save millions in `Customer Churn`.

To solve this problem, you have already designed a conceptual end-to-end data platform to bring together data from multiple channels (social media, call center logs, website forms) into a unified data source for analytics, ML and insights

Now, you need to build a physical data platform combining various tools and technologies.

### Data Sources
You have been provided access to the source datasets as detailed below:

- Customers
  - `Description:` Customer data with attributes like customer id, name, gender, phone, location etc.
  - `Format:` CSV
  - `Data Location:` In an s3 Bucket on [AWS HERE](https://eu-north-1.console.aws.amazon.com/s3/buckets/core-telecoms-data-lake?region=eu-north-1&prefix=customers/&showversions=false)
  - `Frequency:` It's a Static Dataset

- Agents
  - `Description:` Lookup table for customer care agents.
  - `Format:` Google Spread Sheet
  - `Data Location:` In a spreadsheet [HERE](https://docs.google.com/spreadsheets/d/17IXo7TjDSSHaFobGG9hcqgbsNKTaqgyctWGnwDeNkIQ/edit?gid=0#gid=0)
  - `Frequency:` It's a Static Dataset

- Call Center Logs
  - `Description:` Daily customer call log with complaint type, agent ID, resolution status, and duration..
  - `Format:` CSV
  - `Data Location:` In an s3 Bucket on [AWS HERE](https://eu-north-1.console.aws.amazon.com/s3/buckets/core-telecoms-data-lake?region=eu-north-1&prefix=call+logs/&showversions=false)
  - `Frequency:` It's a Daily Populated Dataset
 
- Social Media
  - `Description:` Complaints across social media platforms, containing customer & agent id, platform, issue type etc.
  - `Format:` JSON
  - `Data Location:` In an s3 Bucket on [AWS HERE](https://eu-north-1.console.aws.amazon.com/s3/buckets/core-telecoms-data-lake?region=eu-north-1&prefix=social_medias/&showversions=false)
  - `Frequency:` It's a Daily Populated Dataset

 - Website Complaint Forms
   - `Description:` Customer-submitted forms with customer id, agent id, complaint type, and resolution status.
   - `Format:` Database Table
   - `Data Location:` In a Transactional Postgres Database
     - Database credentials located here in [SSM Paramter](https://eu-north-1.console.aws.amazon.com/systems-manager/parameters/?region=eu-north-1&tab=Table#list_parameter_filters=Name:Contains:%2Fcoretelecomms%2Fdatabase%2F) in AWS
   - `Frequency:` It's a Daily Populated Dataset into different table for each day.

### Note: 
- To retrieve all Data stored on AWS, use your existing credentials to login and ensure you are in the Stockholm Region.
  - However, the expectation is that your pipeline must be designed to extract the objects from s3.
  - Full Permissions on s3 and SSM has been given to your user and permission to create Access and Secret Key. DO NOT EXPOSE/PUSH THE KEYS ON GITHUB.
The Agents Dataset provided in the google sheet above `MUST` be moved to your own `PRIVATE` google sheet , this will allow you to set up all necessary `CREDENTIALS` like `SERVICE ACCOUNT` to be able to communicate with Google API to be able to programatically extract the data from your google spread sheet.


### PROJECT OBJECTIVES
Implement a `Production-Grade` solution that covers the following:
- Data Ingestion (Raw Layer)
  - Develop an extraction layer that reads data from each source.
  - Store raw, unprocessed data in Cloud Object Storage (e.g., S3, GCS, Azure Blob) in Parquet format.
  - Include metadata tracking like the load time.
  - `Pay Attention` to the messy state of the data like
    - Column naming
    - Customers Emails
    - Many more
   
### Data Storage and Modeling:
Design a suitable `Data Warehouse` of choice like Snowflake, Redshift, BigQuery e.t.c to house these data in a unified and central storage to serve downstream use cases.
Since the output of your work will be consumed by downstream users for Analytics, ML, etc. You `MUST` ensure the data is properly transformed and modeled using dbt for easier consumption and querying.

### Data Cleaning and Enrichment
- Perform data cleaning and standardization on the raw data where necessary.
- Enrich the datasets by performing joins, aggregation, categorization etc. based on your understanding of the datasets and as you deemed necessary.
- Make sure the data is of high quality and can be trusted.

### Orchestration:
- `Apache Airflow` must be used for orchestrating the entire workflow, which includes:
  - Extracting the data from the different data sources and writing to a data lake.
  - Transforming the data to ensure high quality and trust.
  - Modelling the data for easier consumption and high performant querying.
  - Testing and validating the accuracy of the data.
- You must also follow best practices like:
  - Idempotency
  - Incremental processing
  - Implementing retries.
  - Failure notification/alert.
  - Etc.

### Containerization:
For reproducibility across environments, 
- You need to package all your code as a docker image and push the image to a cloud based container registry (e.g docker hub).
- This includes all your Python codes and DAG.

### CI/CD Pipeline:
`Continuous Integration` and `Continuous Deployment` (CI/CD) should be integrated to your Github Repository. It should include:
- `CI` that carries out checks on code linting to ensure code written follows best practices.
- `CD` to carry out the Build and Push of your python codes to a Cloud based Container Registry.

### Infrastructure as Code (Terraform):
- All `Cloud Infrastructures` like IAM, Object storage, DB/DW Networking resource provisioning `MUST` be `Terraformed`.
- `Terraform State` File backend managed in the cloud using an Object Storage.

### BONUS (NOT MANDATORY)
Derive any insights from the Data Set, any recommendation on what `CoreTelecoms` can do as a result of the data available.


### SUBMISSION REQUIREMENTS
- A `GitHub Repository` containing a well documented `README` of the entire project. It should contain important information like the:
  - Project overview/background.
  - Project structure.
  - Architecture diagram.
  - Choice of tools and technology.
  - How to set up the project.
  - e.t.c.

A `Power Point` or something similar for presentation of the entire project to the management.
Project submission will close on the `4th December, 2025`.

Github link should be submitted in the link HERE.

###  PLEASE REACH OUT ON SLACK IF CLARIFICATION NEEDED.

