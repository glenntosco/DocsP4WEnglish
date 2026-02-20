---
description: P4 Warehouse User creation
---

# Create Users

To create a [new user](../permissions/add-users.md), navigate to Setup > System > Users

The Users menu lists the various user accounts created. As mentioned in the previous section, user accounts can be of type Administrator and type User. New users, can be created by selecting the “New” button from the top right or existing users can be deleted by selecting the black “X” on the right side of each listed user (additional new Administrator accounts can be deleted but the initial Administrator account cannot). To examine or edit an existing user, simply select the Username from the list.

![P4 Warehouse user list](<../.gitbook/assets/image (276).png>)

![P4 Warehouse user options](<../.gitbook/assets/image (194).png>)

From the P4 Warehouse users list you can accomplish several tasks:

* Add New User&#x20;
* Activate a deactivated user
* Deactivate a user
* Delete a user

Also, you can impersonate (click the link icon in the photo below) a user to verify permissions and help in screen layouts.

![P4 Warehouse impersonate user](<../.gitbook/assets/image (166).png>)



On the User List screen, you can open and edit an existing user by clicking the link in user column or you can click the :new: button to add a new user.

![](../.gitbook/assets/usersettings.gif)

In the User Setup Screen there are several important sections that must be configured.

**Login credentials** - In this section of the screen you will configure the basic settings for the user such as username, password, language, Default Warehouse, core permissions.

**Assigned Zones** - In this area set the zones the user is allowed to work in. This is used to prevent users from working outside of the assigned warehouse or zone.

**Personal information** - First Name, Last Name and Time Zone and photo of the user.

**Handheld** - This configuration should only be used for company owned vehicles that are doing company deliveries.

**Modules** - This is where the permissions are set based on the users roll in the warehouse.

![P4 Warehouse Printer Assignments](../.gitbook/assets/Printer-Assignments.gif)

**Clients** - This setting is for a user that works for a specific 3PL. This setting will limit the user to only see customers, vendors, sales orders, purchase order from the one 3PL client.

**Printers -** In the section you will choose the printer(s) the user is allowed to use. After selecting the proper printer(s) select one print as default. In the lower right corner, there is an Advanced button. Click this button to assign different printers to different labels.

Default Menu - This determines the screen that is shown on login, for example /demolatam/main/pickTicketList/true will open the pending orders screen for the Tenant Demolatam.

{% hint style="danger" %}
**Use this with caution, putting invalid data here can lock the user out of the system!!!**
{% endhint %}



