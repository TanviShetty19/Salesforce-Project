# Salesforce-Project
# 🚗 WhatsNext Vision Motors – Salesforce CRM Automation Project

##  Project Description

**WhatsNext Vision Motors**, a leader in automotive innovation, has launched a transformative Salesforce CRM initiative to enhance the **customer experience** and optimize internal **operational workflows**. This project addresses common industry challenges such as **dealer allocation**, **stock availability**, and **bulk order management** — all using the power of **Salesforce automation, Apex programming**, and **declarative tools**.

The project's goal is to modernize the **vehicle ordering process** by ensuring seamless interactions between customers, dealers, and the company. With features like real-time stock validation, automated dealer assignment, and scheduled updates, the system brings agility, accuracy, and transparency to operations.

##  Key Objectives

1. **Improve Customer Experience**  
   - Suggest nearest dealer based on customer address.  
   - Send proactive communication like test drive reminders and order confirmations.

2. **Ensure Stock Accuracy**  
   - Prevent ordering vehicles that are out of stock.  
   - Automatically update stock availability via scheduled processes.

3. **Streamline Operations**  
   - Automate order assignment and status updates.  
   - Reduce manual overhead through background Apex processes.

4. **Enable Scalable Architecture**  
   - Use modular code structure (trigger handlers, utility classes).  
   - Ensure code reusability, maintainability, and alignment with best practices.

##  Features Implemented

### 🔧 1. Salesforce CRM Implementation

- **Custom Object Model**:  
  - `Vehicle__c`: Stores vehicle details and stock quantity.  
  - `Dealer__c`: Stores dealer locations and contact information.  
  - `Order__c`: Tracks customer orders and their status.  
  - `TestDrive__c`: Tracks test drive requests and schedules.

- **Dealer Locator Logic**:  
  Automatically identifies and links the closest dealer to a customer's shipping address using geolocation logic or a zip code-based matching approach.

- **Order Lifecycle Tracking**:  
  Custom fields and automation manage order statuses from creation to fulfillment.

- **Service Requests and Test Drive Handling**:  
  Customers can book services and test drives, and the system keeps track of confirmations, reminders, and dealer involvement.

###  2. Process Automation (Using Flows & Workflows)

- **Record-Triggered Flows**:  
  - On `Order__c` creation:
    - Validate stock availability of the selected vehicle.
    - Auto-assign the nearest dealer based on address.
    - Set initial order status to "Confirmed" or "Pending" accordingly.

- **Scheduled Flow / Time-Based Workflows**:  
  - For `TestDrive__c`:
    - Sends email reminders 24 hours before the scheduled time.

- **Validation Rules**:  
  - Prevent users from selecting vehicles that are out of stock.

###  3. Apex & Triggers (Backend Business Logic)

- **Apex Triggers**:
  - **OrderTrigger**:  
    - Before insert: checks vehicle stock, prevents invalid orders.
    - After insert: assigns the nearest dealer and updates order status.

  - **Trigger Handler Architecture**:  
    - Implements separation of concerns by delegating logic to handler classes.
    - Example: `OrderTriggerHandler.cls` contains all business logic.

- **Reusable Apex Utility Classes**:  
  - `DealerLocatorService.cls`: Finds the nearest dealer.
  - `StockValidator.cls`: Checks if the vehicle is in stock.
  - `EmailService.cls`: Sends reminders and stock alerts.

###  4. Batch Jobs & Scheduled Apex

- **VehicleStockBatch.cls**:  
  - Runs on a schedule (e.g., nightly) to verify and refresh all vehicle stock levels.

- **OrderStatusBatch.cls**:  
  - Periodically updates the status of bulk orders:
    - Sets to `'Confirmed'` if in stock.
    - Sets to `'Pending'` if not in stock.

- **Scheduled Apex**:  
  - `Schedule_OrderStatusUpdate.cls`: Schedules the `OrderStatusBatch` to run every 6 hours.
  - `Schedule_StockAlert.cls`: Sends automated email to procurement if stock falls below threshold.

##  What You’ll Learn

| **Skill Area**             | **What You'll Practice**                                                                       |
|------------------------====|------------------------------------------------------------------------------------------------|
| **Data Modeling**          | Designing objects like `Vehicle__c`, `Dealer__c`, `Order__c`, and defining their relationships |
| **Fields & Relationships** | Using Lookup and Master-Detail relationships to link data appropriately                        |
| **Lightning App Builder**  | Creating custom record pages and dashboards for users and managers                             |
| **Flows**                  | Building record-triggered flows to automate dealer assignment and stock checks                 |
| **Apex Programming**       | Writing Apex triggers, classes, and SOQL queries to enforce complex business rules             |
| **Trigger Frameworks**     | Implementing best practices via handler patterns and reusable methods                          |
| **Batch Apex**             | Efficient processing of large datasets (orders, stocks) in chunks                              |
| **Scheduled Apex**         | Automating daily/weekly/monthly jobs for proactive system updates                              |

##  Suggested Folder Structure

```
WhatsNext-VisionMotors-Salesforce/
│
├── README.md
├── objects/
│   ├── Vehicle__c.object-meta.xml
│   ├── Dealer__c.object-meta.xml
│   ├── Order__c.object-meta.xml
│   └── TestDrive__c.object-meta.xml
│
├── apex/
│   ├── triggers/
│   │   ├── OrderTrigger.trigger
│   │   └── TestDriveTrigger.trigger
│   ├── handlers/
│   │   ├── OrderTriggerHandler.cls
│   │   └── TestDriveTriggerHandler.cls
│   ├── classes/
│   │   ├── DealerLocatorService.cls
│   │   ├── StockValidator.cls
│   │   └── EmailService.cls
│   ├── batch_jobs/
│   │   ├── VehicleStockBatch.cls
│   │   └── OrderStatusBatch.cls
│   └── schedulers/
│       ├── Schedule_StockAlert.cls
│       └── Schedule_OrderStatusUpdate.cls
│
├── flows/
│   ├── OrderDealerAssignment.flow-meta.xml
│   └── TestDriveReminder.flow-meta.xml
│
└── email_templates/
    ├── TestDriveReminderTemplate.email
    └── LowStockAlertTemplate.email
```

## Benefits & Impact
|----------------------------|-------------------------------------------------------------------------------------------|
| Area                       | Impact                                                                                    |
|----------------------------|-------------------------------------------------------------------------------------------|
| **Customer Satisfaction**  | Accurate order placement and clear communication reduce confusion and disappointment.     |
| **Operational Efficiency** | Automation removes manual effort in order assignment and stock checks.                    |
| **Data Accuracy**          | Prevents invalid orders and ensures real-time stock tracking.                             |
| **Scalability**            | Designed using best practices, modular logic, and reusable code structures.               |
| **Team Productivity**      | Frees up staff from repetitive tasks, allowing focus on strategy and customer engagement. |
|----------------------------|-------------------------------------------------------------------------------------------|

