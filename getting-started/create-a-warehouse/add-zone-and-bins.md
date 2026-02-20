---
description: P4 Warehouse Add Zone & Bins
---

# Add Zone & Bins

### Add Zone

On the warehouse edit screen, click the  :new:  button in the Zones section of the screen.

{% hint style="info" %}
For a more detailed information of [Zones](../../setup/warehouse/zones.md).
{% endhint %}

![](<../../.gitbook/assets/add zone.gif>)

After clicking new you will get the screen in the image below, complete the zone code, mask, description, and Product Handling Type then click submit.

{% hint style="info" %}
Product Handling types are By Product or by LPN. If you are not using LPN's then do not select LPN as your handling type.
{% endhint %}

### New Zone Screen

![](<../../.gitbook/assets/new zone.gif>)

Now you will see your new zone in the zone list below the warehouse setup screen.

![](<../../.gitbook/assets/edit zone 2.gif>)

Open your new Zone by clicking the link in the Code column. Here you will setup the details for the Zone.

![P4 Warehouse Zone Configuration](<../../.gitbook/assets/image (85).png>)

In the zone settings screen, you will encounter various detailed settings for the zone behavior.

{% hint style="info" %}
Setting a zone to quarantine will prevent you from allocating the inventory in this zone
{% endhint %}

Zone Mask is an especially important feature, this is best managed by the implementation team. In a brief overview zone mask will assist you in creating hundreds or thousands of bin locations.&#x20;

Mask Structure: Mask segments are inside of { } each segment is numbered. Below are a couple samples with the results.

* 01-A-{0:00}-(1:0} = 01-A-12-A&#x20;
  * This format is aisle, zone, bin, level
* A-{0:00}-{1:0} = A-03-A
  * This format is Zone, Bin, Level

Zone masking all depends on your physical warehouse layout, use this feature with care as you can accidently create hundreds of thousands of bin locations.

> It is not required to have Zone in your label format, zone can be included as a separate field on the bin label.

Setting the Zone to dedicated product bins is for exceptional use cases and is not recommended for most warehouses or distribution centers.

Setting the Zone to one bin per product is for exceptional use cases and is not recommended for most warehouses or distribution centers.

{% hint style="success" %}
It is important to configure the pick ticket printer in each zone, this is vital if you plan to use Zone picking.&#x20;

Dimensions for Zones/Bins are **vital** for several purposes.
{% endhint %}

When designing your zones keep in mind, Staging Zones, Cross Dock Zones, Receiving Zones, Refrigerated Zones, Frozen Zones, Hazardous Materials Zones, etc.&#x20;
