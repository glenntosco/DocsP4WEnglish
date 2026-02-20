---
description: P4 Warehouse
---

# Database backups

P4 Warehouse has database backups available every 12 hours. The backups are in a Microsoft bacpac file format. Bacpac is easy to import into any Microsoft SQL version 2008 and up.

{% hint style="info" %}
[Microsoft bacpac instructions](https://docs.microsoft.com/en-us/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database?view=sql-server-ver15) (click)
{% endhint %}

Step one is to download your data from P4 Warehouse.

Step 2 is In SQL Manager (SSMS) right click on databases and select Import Data-tier Application

Step 3 select the bacpac file you downloaded, then give your database a name.

Step 4 click Finish, (The process normally will take 1-3 minutes to complete.).

![P4 Warehouse Database import in Microsoft SQL Server](<../../.gitbook/assets/image (108).png>)

![P4 Warehouse Database Tables](<../../.gitbook/assets/image (169).png>)



### With this database you can connect to PowerBI, run queries, generate reports as well as various other options.

You will find database backups under Setup/System/Database backups

![P4 Warehouse Database backups](<../../.gitbook/assets/image (90).png>)

By clicking the download button, you will save the backup to your local computer.

{% hint style="info" %}
This is a bacpac file that can be restored onto SQL 2008 R2 or newer.&#x20;
{% endhint %}



