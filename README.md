# Cybersecurity-Training-Labs-Splunk-with-Simulated-Banking-Logs
A hands-on Splunk training project for cybersecurity professionals using realistic banking application logs to teach threat detection, event correlation, and security monitoring.

Getting Started
- Upload the CSV to Splunk.
- Index it as `bns_logs`.
- Start exploring using the guide and examples.
- Build dashboards and alerts.

1. Introduction to the Logs
Columns Overview
- `timestamp`: When the event happened.
- `user_id`: Numeric user ID.
- `username`: Name of the user.
- `location`: User's city.
- `event_type`: Action performed (login, deposit, etc.).
- `status`: Success or failure.
- `transaction_amount`: Amount involved (if any).
- `account_balance`: Remaining balance.
- `ip_address`: Source IP.
- `message`: Event description.
- `event_id`: Unique event identifier.
- `event_category`: Broad classification.


2. Basic SPL Searches

index=bns_logs  #To see all the logs
index=bns_logs event_type=login #To view only Login events
index=bns_logs status=failure  #To view failed events

3. Field Extraction and Tables
   
index=bns_logs
| table timestamp username event_type status ip_address #This display only timestamp, event_type, and status.

5. Aggregation and Statistics
   
   "Count by event type"

index=bns_logs
| stats count by event_type   #To get table showing each event type and how many times it occurred

"Total transaction amount"

index=bns_logs event_category=transaction
| stats sum(transaction_amount)   #This returns a single number, showing the total of all transactions

"Total transaction by user"

index=bns_logs event_category=transaction
| stats sum(transaction_amount) by username #This returns the users and the total transactions by users.

"Time-Based Analysis"

index=bns_logs
| timechart count by event_type #This creates a time-based chart showing how many events occurred for each event type over time.

5. Security Monitoring and Threat Hunting

   "Failed logins by IP"
   index=bns_logs event_type=login status=failure
| stats count by ip_address   #This counts the number of failed login attempts from each IP address.

    "Password resets"

   index=bns_logs event_type=password_reset   #This retrieves all log entries where the event type is a password reset.

7. Alerts and Automation

   "Alert for >3 failed logins in 5 minutes"

   index=bns_logs event_type=login status=failure
| bucket span=5m _time
| stats count by _time, username
| where count > 3      #It detects usernames with more than 3 failed login attempts within any 5-minute time window — useful for identifying potential brute-force attacks. Create an alert for this









