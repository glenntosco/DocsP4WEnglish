---
description: P4 Warehouse
---

# Allocation

If Pick ticket is in a draft state it will need to be released to Floor to begin the processes.

{% hint style="info" %}
Releasing to Floor moves the Order from Draft (editable) to a live document.
{% endhint %}

{% hint style="warning" %}
In the event your instance of P4 Warehouse is connected via integration with your ERP, orders will not show draft status and cannot be edited from the WMS, edits must take place from the ERP.
{% endhint %}

![P4 Warehouse Sales Order Screen](<../.gitbook/assets/Pick ticket screen-01.png>)

Release to Floor can be accomplished by single order or in groups.

{% hint style="info" %}
Auto processing for different stages of fulfilment can be enabled or disabled in Setup->System->Configuration->Business->Fulfilment->Automation
{% endhint %}

![](<../.gitbook/assets/image (191).png>)

Pick ticket status is now unallocated and new processes are available:&#x20;

![](<../.gitbook/assets/image (50).png>)

Pick ticket Line section is also updated with more information for review:

![](<../.gitbook/assets/image (159).png>)

If all information is correct, the user is ready to allocate using system rules and user selections by pressing the Allocate button. As you can see in the two images below this screen (most screens in P4 Warehouse) are dynamic, meaning the screen only shows the options that are available for this order or group of orders. The first photo below does not have items with Lot Numbers or Expiry therefore the screen does not display the options that you see on the second photo below.

{% hint style="success" %}
P4 Warehouse has the strongest algorithm in the industry. The process of Allocation is the system scanning and deciding what needs to be picked based on the rules in the system and the settings on the allocation screen.
{% endhint %}

![P4 Warehouse Allocation Screen for Packsize Products](<../.gitbook/assets/image (243).png>)

![P4 Warehouse Allocation screen for Expiry and Lot Controlled products](<../.gitbook/assets/image (263).png>)

Pick Ticket now shows a state of "Ready to Pick' and percent of Allocation along with more available processes.

Following Process buttons are enabled:

* Unallocate -&#x20;
* Print Product Barcodes -&#x20;
* Wave -&#x20;
* Cartonize -&#x20;
* Suspend -&#x20;
* Proforma Invoice -&#x20;

![](<../.gitbook/assets/image (219).png>)

The Pick ticket Lines section now show allocation information:

![P4 Warehouse Allocated Order Screen](<../.gitbook/assets/image (155).png>)

{% hint style="info" %}
Power Feature, with P4 Warehouse you can un-allocate 1 line on the order and or you can lower the quantity in the middle of picking.&#x20;
{% endhint %}

Print Product Barcodes:

{% tabs %}
{% tab title="1) select items to print" %}
![](<../.gitbook/assets/image (286).png>)
{% endtab %}

{% tab title="2) select printer and qty" %}
![](<../.gitbook/assets/image (130).png>)
{% endtab %}
{% endtabs %}

Cartonize Orders based on preset system rules or manual override:

![](<../.gitbook/assets/image (262).png>)

Suspend: places orders on hold. Once confirmed Pick ticket state is changed to Suspended and will need to be unsuspended to proceed.

![](<../.gitbook/assets/image (139).png>)

Proforma Invoice opens in new browser tab and allows for downloading.

{% file src="../.gitbook/assets/Proforma Invoice sample.pdf" %}
Proforma Invoice Example
{% endfile %}

Wave:

![](<../.gitbook/assets/image (171).png>)

Pick ticket is now in Waved state and a Totes section is now visible:

![](<../.gitbook/assets/image (180).png>)

![](<../.gitbook/assets/image (203).png>)

Tote information can be accessed by clicking on the SSCC-18 hyperlink. As picks are processed the content section will be populated and Content labels can be printed

* Tote labels can be reprinted using the Tote label button
* Content labels can be printed using the content label button

![](<../.gitbook/assets/image (141).png>)



