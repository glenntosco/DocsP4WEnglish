---
description: P4 Warehouse Handheld Receiving Setup
---

# Handhelds

On this screen you will see setting that directly affects what the Handheld user sees during the receiving process.

![](<../../../.gitbook/assets/image (136).png>)

Options:

* Show expected Quantity: This is sometime also called Blind Receiving, depending on your Companys policies as how this is option is set. In a typical warehouse it is often better to force the receiver to count the products that actually have arrived versus the receiver simply accepting the expected quantity.
* Allow Handheld to close: This setting displays a close button for the handheld user, allowing them to close a Purchase Order when all the product for the Purchase Order has been received.&#x20;
  * **This setting is off by default as it can cause irreversible errors if the Purchase Order is closed before receiving the product.**&#x20;
* Collection weight and dimensions: This is a vital setting if product weight and dimensions are important to your operation. If activated on receiving if the product is missing weight and dimensions the handheld will ask the receiver to input, the correct data.
  * This information will not be requested if the product master is already populated.
* Collect Barcode: This setting is for collecting the correct barcode data, this setting will ask the receiver to scan the barcodes for the product / packsizes if the barcode data is missing from the system.
  * This information will not be requested if the product master is already populated.
* Print Labels, this setting will ask the receiver if they want to print labels after receiving a purchase order line. This is a very powerful tool when you have product arriving without barcode labels.
  * The handheld will first ask the receiver if they need to print labels, when the receiver answers yes, the system will prompt the receiver for how many labels to print.
* Display Product Locations, this setting will show the receiver on the handheld where the product is currently stored in your warehouse, or where the product was last stored in your warehouse. Otherwise the receiver will locate the product in a bin location that is empty.
