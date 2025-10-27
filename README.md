# Automated Weekly Sales & Subscription Report (n8n Capstone Project)
<img width="1536" height="1024" alt="ziti logo" src="https://github.com/user-attachments/assets/cd903b0f-051f-4a64-8017-9678a55ccdd5" />

## Table of Contents
- [Acknowledgments](#acknowledgments)
- [Project Overview](#project-overview)
- [Workflow Description](#workflow-description)
- [Data Flow Diagram](#data-flow-diagram)
- [Technical Stack](#technical-stack)
- [Error Handling and Automation Design](#error-handling-and-automation-design)
- [Future Enhancements](#future-enhancements)
- [Workflow Files](#workflow-files)


## Acknowledgments
I would like to express my deepest gratitude to the following individuals and organizations for their support and guidance throughout this project:
First and foremost, I acknowledge Liz Ajayi and Ted Mouyabi the founder of Ziti group for providing the platform and resources necessary to develop my skills in Artificial Intelligent. Their commitment to empowering people in technology is truly inspiring.
I would also like to extend my sincere appreciation to my facilitators, Tjin-Li Hoh and Ukachi Osisiogu their expertises, patiences, and dedications were instrumental in my success. Also their guidances were invaluable, and I am grateful for the opportunity to learn from them.

## Project Overview
##### Objective
This project automates the process of collecting, analyzing, and reporting key business KPIs from multiple data sources using n8n, eliminating manual work and providing decision-makers with weekly insights.

The workflow aggregates Sales and Subscription data, computes essential metrics, merges the results, and emails a professional summary automatically each week.

## Workflow Description
#### Key Processes


- Trigger Node (Cron) – Runs automatically every Monday 8 AM.

##### Sales Data Node – Pulls raw sales information from a CSV, API, or database.

- Sales Function Node – JavaScript computes:

1: Total Revenue

2: Total Units Sold

3: Top Product

4: Top Region

###### Subscription Data Node – Retrieves current subscription metrics.

- Subscription Function Node – JavaScript computes:

1: Total Subscription Revenue

2: Active & Canceled Customers

3: Top Subscription Type

-  Merge Node – Combines both datasets into a single JSON output.
-  Email Node – Sends an HTML report to stakeholders.
-  Slack Node: Posts summary to the Slack workspace.
- Google Sheets Log: Archives weekly metrics for history tracking.

###### Trigger: Cron node runs automatically every week.


## Data Flow Diagram

```
[Trigger (Schedule)]
        │
        ├──► [Sales Data] ─► [Compute Sales KPIs]
        │
        └──► [Customer Data] ─► [Compute Subscription KPIs]
                                   │
                                   ▼
                              [Merge Node]
                                   │
                                   ▼
                         [Message Compute KPI]
                                   │
                ┌──────────────────┼──────────────────┐
                ▼                  ▼                  ▼
           [Send Email]       [Send Slack]     [Weekly Report Log]

```
<img width="1599" height="703" alt="sales subscription workflow" src="https://github.com/user-attachments/assets/6a358d68-f725-4402-9901-d9b3a6c7d34c" />

### Node Explanation

- Trigger (Schedule)

          - Runs the workflow automatically every week (e.g., every Monday at 8 AM).

- Sales Data (Google Sheets node)

        - Reads raw weekly sales data.

        - Output goes into Compute Sales KPIs.

- Compute Sales KPIs (Function node)

        - Calculates metrics like total sales revenue, top product, and units sold.

- Customer Data (Google Sheets node)

        - Reads subscription/customer data.

        - Output goes into Compute Subscription KPIs.

- Compute Subscription KPIs (Function node)

        - Calculates metrics like active/canceled customers, subscription revenue, etc.

- Merge Node (Combine)

        - Combines outputs from both “Compute Sales KPIs” and “Compute Subscription KPIs”.

- Message Compute KPI (Function node)

        - Formats the merged data into a clean JSON object (this is where you paste your JSON output template).

- Send Email (Gmail node)

        - Sends the formatted weekly report via email.

- Send Slack (Slack node)

        - Posts the same report to a Slack channel.

- Weekly Report Log (Google Sheets node)

        - Appends the JSON summary as a new row into your log sheet.

## Technical Stack

- Tool: n8n
- Logic Language: JavaScript (Function nodes)
- Data Sources: Local/remote Sales & Subscription tables (CSV)
- Output: Automated HTML Email Report + Slack Summary
- Error Logs: Google Sheets
- Integrations: Gmail | Slack | Google Sheets

## Example Weekly Output
Metric	Value
Sales Revenue	$10,587,500.00
Units Sold	345,000
Top Product	Shoes
Top Region	South
Subscription Revenue	$149,819,686.00
Active Customers	41,250
Canceled Customers	33,750
Top Subscription Type	Basic

### Email Message
<img width="1593" height="731" alt="sales-email" src="https://github.com/user-attachments/assets/409b359a-8f5b-4662-bc53-e03844be6f51" />

### Slack Message
<img width="1506" height="550" alt="sales slack" src="https://github.com/user-attachments/assets/b4d3641b-d85b-4b82-b6c9-4d29393276fb" />

### Weekly Report Log
<img width="1599" height="317" alt="sales report log" src="https://github.com/user-attachments/assets/da88da7e-8a69-4fa9-b881-7727146dac54" />
          
## KPIs Generated
##### Sales KPIs

### Key Metrics Overview

| **Metric**          | **Description**                   |
|----------------------|-----------------------------------|
| **Total Revenue**    | Quantity × Unit Price             |
| **Total Units Sold** | Total sales volume                |
| **Top Product**      | Best-performing product           |
| **Top Region**       | Highest revenue region            |


### Subscription KPIs

| **Metric**                  | **Description**                     |
|------------------------------|-------------------------------------|
| **Total Subscription Revenue** | Sum of subscription income          |
| **Active Customers**         | Count of ongoing subscriptions      |
| **Canceled Customers**       | Count of terminated subscriptions   |
| **Top Subscription Type**    | Most subscribed plan                |

### Email Report Template (HTML)
<html>
  <body style="font-family: Arial, sans-serif; line-height: 1.6; color: #333;">
    <h2 style="color:#2a7ae2;">📊 Weekly Sales & Subscription Report</h2>
    <p>Hello Team,</p>
    <p>Here’s the automated summary for this week:</p>

    <h3 style="color:#2a7ae2;">Sales KPIs</h3>
    <table border="1" cellpadding="6" cellspacing="0" style="border-collapse:collapse; width:70%;">
      <tr><th align="left">Total Revenue</th><td>${{$json["salesRevenue"]}}</td></tr>
      <tr><th align="left">Total Units Sold</th><td>${{$json["totalUnits"] || "N/A"}}</td></tr>
      <tr><th align="left">Top Product</th><td>${{$json["topProduct"] || "N/A"}}</td></tr>
      <tr><th align="left">Top Region</th><td>${{$json["topRegion"] || "N/A"}}</td></tr>
    </table>

    <h3 style="color:#2a7ae2; margin-top:20px;">Subscription KPIs</h3>
    <table border="1" cellpadding="6" cellspacing="0" style="border-collapse:collapse; width:70%;">
      <tr><th align="left">Total Revenue</th><td>${{$json["subscriptionRevenue"]}}</td></tr>
      <tr><th align="left">Active Customers</th><td>${{$json["activeCustomers"]}}</td></tr>
      <tr><th align="left">Canceled Customers</th><td>${{$json["canceledCustomers"]}}</td></tr>
      <tr><th align="left">Top Subscription Type</th><td>${{$json["topSubscriptionType"]}}</td></tr>
    </table>

    <p>Best regards,<br><b>n8n Automation Bot 🤖</b></p>
  </body>
</html>
Example Email Output:
<img width="1385" height="760" alt="email-result" src="https://github.com/user-attachments/assets/3cade43a-7b2c-49dc-8a8c-c3ea45f82f8b" />



## Error Handling and Automation Design
Component	Purpose
Error Trigger Workflow	Detects failed execution
Slack Node	Sends alert to development channel
Gmail Node	Sends detailed error report
Google Sheets Log	Stores error details for audit
IF & Try/Catch Nodes	Ensures safe execution even when data is missing

<img width="1598" height="824" alt="error-workflow" src="https://github.com/user-attachments/assets/8e2b423a-5eed-48f8-97d2-19223f4ebabf" />


### Error Handler Workflow

Ensures the main automation never fails silently.
When an error occurs, this secondary workflow runs automatically to:

- Send a Slack alert

- Email the error details

-  Log the issue in Google Sheets

### Email Message
<img width="1586" height="744" alt="error-email" src="https://github.com/user-attachments/assets/6d31b758-fd41-43d8-91d7-a3a5e62b4bb8" />

### Slack Message
<img width="1598" height="818" alt="slack-error" src="https://github.com/user-attachments/assets/dd4f6b44-b2d8-452f-a19c-45ca2781a5cd" />

### Error Log Form
<img width="1598" height="342" alt="error-log form" src="https://github.com/user-attachments/assets/547cc9c1-8f92-4574-a577-c46d81539e64" />



## Future Enhancements

- Add visual charts in the email using HTML/CSS.

- Send Slack or Discord notifications when a report is delivered.

- Integrate marketing or support KPIs for a complete business dashboard.

## Workflow Files
File	Description
- sales_subscription_workflow.json	Main workflow (Sales + Subscription)
- error_handler_workflow.json	Error handling workflow



