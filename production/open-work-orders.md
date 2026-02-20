---
description: P4 Warehouse Open Work Orders - Production
---

# Open Work Orders

In this screen you will find the list of work orders

![P4 Warehouse work Orders](<../.gitbook/assets/image (77).png>)

There are several options for this screen where you can add a new work order or process an existing.&#x20;

Click the data entry button and select New Work Order to create a new Production order.

![P4 Warehouse New Production Order](<../.gitbook/assets/image (143).png>)

{% hint style="info" %}
For a "Product to build" to show in the drop-down list you first need to create the [Bill of Material](../setup/products/bom-bill-of-materials.md).
{% endhint %}

Select the proper warehouse, select the product you plan to produce, enter the quantity you plan to produce, Work Order number field can be left blank and P4 Warehouse will generate the next sequence work order number. Click Submit!

After clicking Submit on the new work order screen you will see the Work Order details similar to the screen below.

![P4 Warehouse Production Order Details.](<../.gitbook/assets/image (231).png>)

Next review the details are correct, assign the order to a production line.

![P4 Warehouse Production Area List](<../.gitbook/assets/image (251).png>)

Now you can click the "Release to Floor" button in the top right of the screen, this will move the Production Order from Draft to a Live order. Once released to the floor click the Allocate Button.

![P4 Warehouse Production Order Allocation](<../.gitbook/assets/image (245).png>)

In the Allocation screen verify the settings are correct and click the allocate button.

![P4 Warehouse Allocated Production Order](<../.gitbook/assets/image (118).png>)

Once allocated you will see various new data parts on the screen. The next step is for the Handheld user to pick the raw goods and move them to the assigned production area, once the raw goods are all in the production area you can begin the production process.

See handheld work order picking for the rest of the process.

{% hint style="info" %}
If the order is produced 100%, the production order will close automatically. If the order is not produced 100% a web dispatcher will need to manually close the order.
{% endhint %}





