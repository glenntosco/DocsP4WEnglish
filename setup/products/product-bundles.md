---
description: P4 Warehouse Product Bundles
---

# Product Bundles

P4 Warehouse allows for product bundles. An example of a product bundle would be a sales promotion of "buy a Thermal Printer and get 5 rolls of ribbons included". In the order process, the salesperson simply enters the promotion SKU into the ordering system. At this point the WMS understands that it needs to pick various products and quantities to complete the order. This reduces the opportunity for the salesperson to make a keying error.

{% hint style="info" %}
Subcomponents must be created before the Product Bundle can be created.
{% endhint %}

![P4 Warehouse Product Bundle](../../.gitbook/assets/bundle.jpg)

In the image above, P4 Warehouse will send a picker to pick 1 printer and five (5) rolls of wax ribbon to complete the order. The two (2) line could be hand keyed into the sales order manually however the Product Bundle feature reduces the chance of error. If you are using SAP B1 this is called a sales kit.&#x20;

{% hint style="success" %}
Remember a product bundle and production are two (2) different processes.&#x20;

Production is when you convert one (1) or more products into a completely new SKU.
{% endhint %}

