---
description: P4 Warehouse Zone Setup
---

# Zones

### Adding Zone

Zones are a key feature in P4 Warehouse, zones allow a level of control not commonly included in Warehouse Management Systems. As the configuration will show there are very powerful options in the zone setup screens.&#x20;

On the warehouse edit screen, click the  :new:  button in the Zones section of the screen.

{% hint style="info" %}
Discuss Zone Setup with your P4 Warehouse Partner, it is always best to have a plan and not create Zones at random.
{% endhint %}

![](<../../.gitbook/assets/add zone.gif>)

After clicking new you will get the following screen, complete the zone code, mask, description and Product Handling Type then click submit.

{% hint style="info" %}
Product Handling types are By Product or By LPN. If you are not using LPN's then do not select LPN as your handling type.
{% endhint %}

### New Zone Screen

![P4 warehouse New Zone Screen](<../../.gitbook/assets/new zone.gif>)

{% hint style="info" %}
Zones can have choose; you choose, however they cannot be duplicated. If you are configuring multiple warehouses keep in mind you cannot duplicate the zones.
{% endhint %}

Now you will see your new zone in the zone list below the warehouse setup screen.

![](<../../.gitbook/assets/edit zone 2.gif>)

Open your new Zone by clicking the link in the Code column. Here you will setup the details for the Zone.

![P4 Warehouse Zone Setup](<../../.gitbook/assets/image (135).png>)

Setup the dimensions for your bins, this will let the P4 Warehouse system tell you where product will fit. Without this information P4 Warehouse will not know what product fits into what size of bin location.

In the zone settings screen, you will encounter various detailed settings for the zone behavior.

{% hint style="info" %}
Setting a zone to quarantine will prevent you from allocating the inventory in this zone as well this inventory will not show in the inventory availability report.
{% endhint %}

Zone Mask is an especially important feature, this is best managed by the implementation team. In a brief overview zone mask will assist you in creating hundreds or thousands of bin locations.&#x20;

Mask Structure: Mask segments are inside of { } each segment is numbered. Below are a couple samples with the results.

* 01-A-{0:00}-(1:0} = 01-A-12-A&#x20;
  * This format is aisle, zone, bin, level
* A-{0:00}-{1:0} = A-03-A
  * This format is Zone, Bin, Level

Zone masking all depends on your physical warehouse layout, use this feature with care as you can accidently create hundreds of thousands of bin locations.

{% hint style="info" %}
> It is not required to have Zone in your label format, zone can be included as a separate field on the bin label.
{% endhint %}

{% hint style="info" %}
Setting the Zone to dedicated product bins is for exceptional use cases and is not recommended for most warehouses or distribution centers.
{% endhint %}

{% hint style="info" %}
Setting the Zone to one bin per product is for exceptional use cases and is not recommended for most warehouses or distribution centers.
{% endhint %}

When designing your zones keep in mind, Staging Zones, Cross Dock Zones, Receiving Zones, Refrigerated Zones, Frozen Zones, Hazardous Materials Zones, etc.&#x20;

### Zones

There are four types of Zones: Product Zone, LPN Zone, Quarantine Zone and Production Zone.

Product Zone is for units and box storage and movement, this is typically referred to as a pick zone.

LPN Zone is for Pallet quantity products often referred to as Overstock zone.

Quarantine Zone is for broken, damaged or expired product, this inventory does not show as available to pick for sales orders.

Production Zone is for manufacturing products, a production zone typically is a series of Production Lines. Raw goods are delivered to the production area, finished good are removed from the production area.

### Allow Multiple Products per Bin

This setting is normally turned off for most bins, placing various  products in one bin can cause slow-downs in the warehouse processes.

