**Apex Classes, Triggers, Asynchronous Apex**

**VehicleOrderTriggerHandler Class :**

This Apex class manages vehicle order validation and inventory updates in a Salesforce application. It
is triggered during insert and update operations on Vehicle_Order__c records.

Rule 1: Prevent Order if Vehicle is Out of Stock

- Function: preventOrderIfOutOfStock(...)
- Trigger Context: before insert/update
- Purpose: Stops order placement if the vehicle’s Stock_Quantity__c is zero or less.
- Logic: Checks stock and throws an error using order.addError() if out of stock.

Rule 2: Update Stock on Order Confirmation

- Function: updateStockOnOrderPlacement(...)
- Trigger Context: after insert/update
- Purpose: Reduces the stock count of the vehicle by 1 when an order is Confirmed.
- Logic: Fetches stock, decrements it, and updates the Vehicle__c record.

This handler ensures that:

- No orders are placed for unavailable vehicles.
- Stock is automatically managed upon confirmed orders.

**Code:**

public class VehicleOrderTriggerHandler {

public static void handleTrigger(List<Vehicle_Order__c> newOrders, Map<Id,
Vehicle_Order__c> oldOrders, Boolean isBefore, Boolean isAfter, Boolean isInsert, Boolean
isUpdate) {

if (isBefore && (isInsert || isUpdate)) {

preventOrderIfOutOfStock(newOrders);

}

if (isAfter && (isInsert || isUpdate)) {

updateStockOnOrderPlacement(newOrders);

}

}

private static void preventOrderIfOutOfStock(List<Vehicle_Order__c> orders) {

Set<Id> vehicleIds = new Set<Id>();

for (Vehicle_Order__c order : orders) {


if (order.Vehicle__c != null) {

vehicleIds.add(order.Vehicle__c);

}

}

if (!vehicleIds.isEmpty()) {

Map<Id, Vehicle__c> vehicleStockMap = new Map<Id, Vehicle__c>(

[SELECT Id, Stock_Quantity__c FROM Vehicle__c WHERE Id IN :vehicleIds]

);

for (Vehicle_Order__c order : orders) {

Vehicle__c vehicle = vehicleStockMap.get(order.Vehicle__c);

if (vehicle != null && vehicle.Stock_Quantity__c <= 0) {

order.addError('This vehicle is out of stock. Order cannot be placed.');

}

}

}

}

private static void updateStockOnOrderPlacement(List<Vehicle_Order__c> orders) {

Set<Id> vehicleIds = new Set<Id>();

for (Vehicle_Order__c order : orders) {

if (order.Vehicle__c != null && order.Status__c == 'Confirmed') {

vehicleIds.add(order.Vehicle__c);

}

}

if (!vehicleIds.isEmpty()) {

Map<Id, Vehicle__c> vehicleStockMap = new Map<Id, Vehicle__c>(

[SELECT Id, Stock_Quantity__c FROM Vehicle__c WHERE Id IN :vehicleIds]

);


List<Vehicle__c> vehiclesToUpdate = new List<Vehicle__c>();

for (Vehicle_Order__c order : orders) {

Vehicle__c vehicle = vehicleStockMap.get(order.Vehicle__c);

if (vehicle != null && vehicle.Stock_Quantity__c > 0) {

vehicle.Stock_Quantity__c -= 1;

vehiclesToUpdate.add(vehicle);

}

}

if (!vehiclesToUpdate.isEmpty()) {

update vehiclesToUpdate;

}

}

}

}

**Batch Class: VehicleOrderBatch**

This batch class automates the processing of pending vehicle orders.

```
 start() : Fetches all Vehicle_Order__c records where Status__c is 'Pending'.
 execute() : For each order:
o Checks if the related vehicle is in stock.
o If available, updates the order status to 'Confirmed' and decreases vehicle stock by 1.
 finish() : Logs a message after batch completion.
```
**Purpose** : Ensures vehicle orders are confirmed only if stock is available, and updates inventory
accordingly.

**Code:**

global class VehicleOrderBatch implements Database.Batchable<sObject> {

global Database.QueryLocator start(Database.BatchableContext bc) {

return Database.getQueryLocator([

SELECT Id, Status__c, Vehicle__c


FROM Vehicle_Order__c

WHERE Status__c = 'Pending'

]);

}

global void execute(Database.BatchableContext bc, List<Vehicle_Order__c> orderList) {

Set<Id> vehicleIds = new Set<Id>();

for (Vehicle_Order__c order : orderList) {

if (order.Vehicle__c != null) {

vehicleIds.add(order.Vehicle__c);

}

}

if (!vehicleIds.isEmpty()) {

Map<Id, Vehicle__c> vehicleStockMap = new Map<Id, Vehicle__c>(

[SELECT Id, Stock_Quantity__c FROM Vehicle__c WHERE Id IN :vehicleIds]

);

List<Vehicle_Order__c> ordersToUpdate = new List<Vehicle_Order__c>();

List<Vehicle__c> vehiclesToUpdate = new List<Vehicle__c>();

for (Vehicle_Order__c order : orderList) {

if (vehicleStockMap.containsKey(order.Vehicle__c)) {

Vehicle__c vehicle = vehicleStockMap.get(order.Vehicle__c);

if (vehicle.Stock_Quantity__c > 0) {

order.Status__c = 'Confirmed';

vehicle.Stock_Quantity__c -= 1;

ordersToUpdate.add(order);

vehiclesToUpdate.add(vehicle);

}

}

}


if (!ordersToUpdate.isEmpty()) {

update ordersToUpdate;

}

if (!vehiclesToUpdate.isEmpty()) {

update vehiclesToUpdate;

}

}

}

global void finish(Database.BatchableContext bc) {

System.debug('Vehicle order batch job completed.');

}

}

**Scheduler Class: VehicleOrderBatchScheduler**

This Apex scheduler automates the periodic execution of the VehicleOrderBatch job.

```
 execute() : Instantiates the VehicleOrderBatch class and starts the batch job with a
batch size of 50.
```
**Purpose** : Enables scheduled processing of pending vehicle orders (e.g., daily, weekly),
ensuring timely order confirmation and stock updates without manual intervention.

**Code:**

global class VehicleOrderBatchScheduler implements Schedulable {

global void execute(SchedulableContext sc) {

VehicleOrderBatch batchJob = new VehicleOrderBatch();

Database.executeBatch(batchJob, 50); // 50 is the batch size

}

}

**VehicleOrderTrigger:**

This Apex trigger runs on Vehicle_Order__c for:

```
 before insert, before update, after insert, after update
```

It calls VehicleOrderTriggerHandler.handleTrigger() to:

```
 Prevent orders if vehicle is out of stock
 Reduce stock on confirmed orders
```
**Code:**

trigger VehicleOrderTrigger on Vehicle_Order__c (before insert, before update, after insert,
after update) {

VehicleOrderTriggerHandler.handleTrigger(Trigger.new, Trigger.oldMap, Trigger.isBefore,
Trigger.isAfter, Trigger.isInsert, Trigger.isUpdate);

}

**Resulting Benefits**

```
 Reduced manual errors through validation and automation.
 Faster customer response due to auto-dealer assignment and emails.
 Real-time stock tracking and alerts for out-of-stock situations.
 Improved user experience by enforcing logical and error-free workflows.
 Scalability ensured by designing loosely coupled Apex classes and reusable flows.
```
