# Automated Weekly Sales & Subscription Report (n8n Capstone Project)
<img width="1536" height="1024" alt="ziti logo" src="https://github.com/user-attachments/assets/cd903b0f-051f-4a64-8017-9678a55ccdd5" />

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

4: Merge Node – Combines both datasets into a single JSON output.

5: Email Node – Sends an HTML report to stakeholders.

## Data Flow Diagram



[Trigger (Cron)]
      ↓
[Sales Data] → [Sales Function]
      ↓
[Subscription Data] → [Subscription Function]
      ↓
         [Merge Node]
              ↓
          [Email Node]
          
## KPIs Generated
##### Sales KPIs

- Total Revenue – Quantity × UnitPrice

- Total Units Sold

- Top Product

- Top Region

##### Subscription KPIs

- Total Revenue

- Active Customers

- Canceled Customers

- Top Subscription Type

## Email Report Template (HTML)
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

<img width="1385" height="760" alt="email-result" src="https://github.com/user-attachments/assets/3cade43a-7b2c-49dc-8a8c-c3ea45f82f8b" />


## Error Handling & Automation Design
Component	Purpose
Error Trigger Workflow	Sends alert email/Slack message if the main workflow fails
IF Nodes	Gracefully handle missing or invalid data (defaults to “N/A”)
Cron Scheduling	Automatically runs every Monday 8 AM
Try/Catch in Function Nodes	Prevents crashes and ensures fallback values

(Insert screenshot of your error-handling branch)


## Technical Stack

- Tool: n8n

- Logic Language: JavaScript (Function nodes)

- Data Sources: Local/remote Sales & Subscription tables (API or CSV)

- Output: Automated HTML Email Report

- Optional Integrations: Google Sheets | Notion | Slack for storage or alerts

## Example Output (Sample)

(Insert a screenshot of your real email result here)


Sales KPIs  
Total Revenue: $10,587,500.00  
Total Units Sold: 345,000  
Top Product: Shoes  
Top Region: South  

Subscription KPIs  
Total Revenue: $149,819,686.00  
Active Customers: 41,250  
Canceled Customers: 33,750  
Top Subscription Type: Basic

## Future Enhancements

📊 Add visual charts in the email using HTML/CSS.

📁 Store weekly results in Google Sheets for historical tracking.

🔔 Send Slack or Discord notifications when a report is delivered.

🧩 Integrate marketing or support KPIs for a complete business dashboard.





