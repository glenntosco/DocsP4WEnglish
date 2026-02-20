---
description: P4 Warehouse 3PL Billing Profile
---

# 3PL Billing Profiles

Creating 3PL Billing profiles is an extremely arduous task in most Warehouse management Systems, however in P4 Warehouse it is wizard based to simplify the process.

By creating different billing profiles, you can invoice the 3PL client correctly the first time! there can be various profiles for the same task, in the event you invoice 3PL clients differently based on volume or type of products.&#x20;

{% hint style="warning" %}
All storage fees are calculated by the **hour**, we have found through our experience and research this works best for the 3PL Provider as well as the 3PL Client.
{% endhint %}

![P4 Warehouse Billing Profiles](<../.gitbook/assets/image (215).png>)

On the screen above you will see the list of profiles, also you can create new profiles.

Click the New button to follow along with the example below, for this example we will create a profile for picking. Each unit picked will cost the 3PL client .002 cents, on the invoice this will show a total for the billing period.

![P4 Warehouse Billing Profile Step #1](<../.gitbook/assets/image (181).png>)

On the screen above select Fulfillment

![P4 Warehouse Billing Profile Step #2](<../.gitbook/assets/image (190).png>)

On the screen above enter a name for the profile this will be the SKU or sometimes called a billing code, next enter the description, this is the description that will show on the invoice, next set the precedence to 1 (in the event you have various profile for the same warehouse function assigned to the same client you can change the precedence to let the system decide how to invoice for this task), set the charge type in this case set it to Picking.

![P4 Warehouse Billing Profile Step #3](<../.gitbook/assets/image (122).png>)

On the screen above chose each, this will invoice the 3PL client .002 cents for each unit picked. As the image above shows there are assorted options to choose from depending on the agreement you have with your client.

{% hint style="danger" %}
If you chose weight or Cube as your billing calculation method, it is critical you setup the dimensions correctly in the products screen.
{% endhint %}

![P4 Warehouse Billing Profile Step #4](<../.gitbook/assets/image (126).png>)

On the screen above enter .002 for the rate, in the additional charges field this is for discounts, taxes or miscellaneous charges.

![P4 Warehouse Billing Profile Step #5](<../.gitbook/assets/image (241).png>)

On the screen above select Cumulative this will consolidate all the picking across various orders into one line on the invoice. Then click finish this will create the billing code, this code will now appear in the 3PL client setup screen and allow you to assign the profile to the client.

{% hint style="info" %}
There is a detailed excel workbook that is sent via email with the PDF invoice, this excel workbook contains detailed information for each line charge, no matter what your selection in step #5 is.&#x20;

Step #5 is for not having thousands of lines on the actual invoice.
{% endhint %}

