---
description: P4 Warehouse Automation Configuration
---

# Automation

Automation Configuration

![P4 Warehouse Automation Configuration](../../../.gitbook/assets/P4_Warehouse_Automation.jpg)

1. Reset User On Change - This setting is to reset the assigned user on an order when the order status changes from being picked to rating. This allows the dispatcher to assign the order to a shipping person.
2. Auto Allocate on Release to floor - This is used in smaller operations and/or ecommerce warehouses where you have an abundance of inventory and want orders shipped as the orders arrive. This will cause the order to auto allocate.
3. Auto Cartonize on Allocation - This setting puts the order in a cartonized format, this is used only when you are using the cartonization functionality of P4 Warehouse.
4. Allow wave on Allocation - This is used in smaller operations and/or ecommerce warehouses where you have an abundance of inventory and want order shipped as the orders arrive. this will cause the pick label to automatically print.
5. Auto Wave On Letdown - This will automatically print the pick label once the product letdowns are completed.&#x20;
6. Auto Wave On Packsize Breakdown - This setting will cause the pick label to automatically print after packsize breakdown is completed.
7. Auto Wave On Production -This will cause a production order to automatically print the production pick label in the event a production order is required to fill a sales order, or a manual production order is entered.
8. Auto Ship On Last Pick - This will cause the order to auto ship on last pick, this will bypass the shipping process.
9. Auto Close On Ship - This will cause the sales order to move to a closed status once the shipping process is completed.

{% hint style="info" %}
For line 8 & 9 this is override in the event you have recount on ship, recount on delivery, sign on ship or sign on delivery selected in the sales order.&#x20;
{% endhint %}

