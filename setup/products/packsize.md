---
description: P4 Warehouse Product Packsize Configuration
---

# Packsize

## Packsize configuration and management is a power function!

P4 Warehouse can have unlimited packsizes per product. Each packsize should be identified by a unique barcode for the packsize. If each packsize has a unique barcode P4 Warehouse will not ask what packsize you are processing, the software will simply ask for the quantity of packs.

{% hint style="info" %}
An example would be if I have a product setup to the following packsizes, each, inner pack, outer pack, pallet packsize, and I am receiving a container with 20 pallets I would scan the pallet barcode and receive 20 pallets in one operation.
{% endhint %}

Once the product is in the warehouse, You can sell the product in any packsize as the software will ask to break the pallet quantity to any lesser packsize (Pallets to boxes for example). As demonstrated in the photo each packsize has a unique barcode.

![P4 Warehouse Multi Pack Product](../../.gitbook/assets/packsize.jpg)

{% hint style="warning" %}
Serialized Products can only be managed in units.&#x20;
{% endhint %}

