---
description: P4 Warehouse
---

# Products

This configuration page features Expiry Format (default is yyyyMMdd for Year-Month-Day), Allow Expired Product (Yes/No toggle, default: No), Print Packsize Labels on Consolidate (Yes/No toggle, default: Yes), Decimal Unit of Measure (unit of measure for decimal controlled products; default is kilograms Kg), Decimal Error Margin (acceptable margin of error for decimal controlled products default; 1.5). After modifying any entry, select the “Save” button to the lower right.

![P4 Warehouse General Product Setup](<../../.gitbook/assets/image (20).png>)

{% hint style="danger" %}
Setting Allow Expired Product to True will permit the system to ship expired products. Use this setting with caution.
{% endhint %}

Setting the option "Allow moving allocated stock" will allow the direct move of products. Sales orders with these products will be automatically re-allocated to the new product location.

Setting the option "Allow attribute change" will allow for the product to be reconfigured, this will ONLY function when a specific product has zero inventory. Use this option with caution.

{% hint style="danger" %}
It is critical that your products are configured properly from the start, accurate product setup is **vital** to having good history and smooth functionality in the processes.
{% endhint %}

Dimensional Data

![P4 Warehouse Dimensional data setup](<../../.gitbook/assets/image (31).png>)

In the image above this sets the defaults of how dimensional data is captured and displayed. Report Weight and Report Length are used on the packslip to display the dimensional data. in the image above the Weight will be in pounds and the Length will be in Cubic feet.

Capture Weight and Capture Length are the units used by the PDT user to capture total pallet size and weight, in the image above the PDT user will use inches for the pallet dimensions and pounds for the pallet weight.
