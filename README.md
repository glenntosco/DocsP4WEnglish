---
description: P4 Warehouse Change Log
cover: .gitbook/assets/shutterstock_64776388.jpg
coverY: 196.0352422907489
---

# P4 Warehouse Change Log

**2.23.1**

* Added a configuration which controls login behaviour when license count has been exceeded
* Added additional Log entry when a user attempts to login but no licenses are available
* Made Truckload, Master Truckload (bol) and Supplement page available for editing
* Updated Product Move handheld screen to provide better visibility of assigned tasks&#x20;

**2.22.1**

* Added a new handheld screen which allows an easier Pallet to Dock movement
* Made Backorder button dependant on Data Entry configuration
* Enabled Comment fields on various document types (Pick ticket, Purchase order, Customer return)
* Added a section in Allocation screen to apply tags while allocating
* Added a new set of configs which allow batching orders during waving into multiple waves
* Added a configuration which enforces PRO number to be entered on Truckload before shipping

**2.21.2**

* Added a new configuration which controls how BOL aggregates customer order information
* Added a new configuration which allows automatically shipping and closing Small Parcel orders once moved to a Dockdoor
* Added Pro bill number to Pick ticket which carriers over to Truck load and Master Truck load when generated
* Added new consolidation rule based on Pick ticket Pro number&#x20;

**2.21.1**

* Added Allocation option to Small Parcel - One-Scan replenish screen
* Added configuration which controls whether Bulk Stage Picking is enabled and visible during Waving
* Added a way to designate Stage Bin to a wave at the time of Waving which enables Bulk Stage Picking handheld function
* Added Bulk Stage Picking handheld function which allows picking multiple lines from a wave to a single bin
* Added configuration which controls whether Bulk Stage should be auto picked to a tote/pallet or just moved as product
* Added a setting on a zone level which designates the zone as Staging Area&#x20;

**2.20.1**

* Added a way to print pallet labels for Pick tickets and truckloads from Staging
* Added a new handheld screen which allows shipping multiple truckloads at the same time by Tag

**2.19.1**

* Added two additional handheld screens in Staging that allow printing pallet labels for Pick tickets and truckloads
* Added prompting staging bins when doing tote merge and repackaging for pallet type totes
* In addition to Address, truck load generation now also obeys carrier
* Adjusted receiving on handheld for multiple lines having the same product. Now lines will deplete in sequence on Linenumber any over receiving will be added to the last line&#x20;

**2.18.2**

* Changed how Waving works. Now a dispatcher can decide how many carton and pallet labels to print during waving
* Waving screen has been simplified and made it more flexible&#x20;

**2.18.1**

* Introduced a configuration which controls whether Pick and Pack operators are allowed to use Full Pallet Picking feature
* Added a way to do Tote to Dock door in a mix mode of both Tote Pallets and LPNs
* Allowed picking Pallets to Carton totes (in Pallet picking mode)&#x20;

**2.17.2**

* Added 2 new flavors of picking: Designation picking and Pallet picking
* Added a facility to assign picker to partial picks of an order thus allowing multiple people to pick same order
* Introduced a requirement for Pallet type totes to have a staging location (similar to LPN) so that at any given time a Pallet has a location
* Modified Pick\&Pack and Wave picking, from now on, if Pallet Picking is enabled, it could only be executed via Pallet Picking conventional Picking and Wave picking will not offer an option to pick a pallet&#x20;

**2.17.1**

* Added a way on handheld to over-receive POs
* Added UserSessionId to AuditLog and refined how logins and logouts work on handheld
* Changed how orders picked during One-Scan shipping, now the oldest order is selected during One-Scan shipping
* Added Driver Name to Handheld shipping screen
* Added additional 'guidance' prompts during picking to help pickers identify next steps&#x20;

**2.16.3**

* Added Productivity report
* Added an ability to assign Small Parcel carriers to dock doors
* Added configuration which requires Small Parcel pallets to be scanned to dock doors
* Added Pallet Tie and Pallet Height to product details and packsizes&#x20;

**2.16.2**

* Added a facility to decorate states in grids for better visibility
* Reworked Cycle Count audit report to surface not only the variances but also zero variance counts
* Changed look and feel of grids for better visibility&#x20;

**2.16.1**

* Added support Android version 14 with additional scanning requirements&#x20;

**2.15.9**

* Reworked One scan replenishment screen to give a better visibility of orders roll up and Inventory availability
* Introduced a surrogate Api Key for Integration API Gateway
* Added a new configuration both on a global and client level which controls whether entities should automatically upload on closure
* Added limited support for User defined fields. Currently available for Pickticket header, Purchase order header and Customer return header
* Added support for grid sticky columns&#x20;

**2.15.8**

* Added additional screens that allows for easier replenishment of One Scan e-comm orders
* Changed menu structure to consolidate E-Comm processing into a single submenu for clarity
* Failed One Scan e-comm orders will automatically suspend if a shipping label fails to print
* Added a way to create folder structure within Custom dashboards by separating folders with '|' character
* Added a menu search control&#x20;

**2.15.7**

* Added Redis Caching for increased performance
* Optimized Grids to retrieve less data which improves performance of the system
* Removed Excel export feature for certain grids which may result in excessive reads from the database

**2.15.6**

* Added a way to pre-rate one line orders to optimize small parcel shipping
* Added One Scan shipping for small parcel/e-comm orders
* Added a new Configurations section that controls various flavours of One Scan shipping

**2.15.5**

* Added Driver name to Pick ticket while shipping for private fleet/external
* Allowed assigning carrier on Pick ticket
* Added Tax Id to Carrier
* Extended precision of Rate on Billing Rule to allow small fractional rates (6 decimal points)
* Added a new feature which allows holding specific inventory from allocation to a user

**2.15.4**

* Added a way to assign LPNs to Totes on Shipping Station screen for various carriers without having to do that on Handheld
* Added a way to use @Rate variable on 3PL Billing Profiles
* Some advanced 3PL Billing profile screens were hidden by default to prevent users from making unintended changes
* Added detailed inventory report which surfaces quantity and detailed information like lots, serials and packsizes
* Surfaced attribute control visibility on product list
* Added a way to designate Client to Custom Action

**2.15.3**

* Added new Daily inventory snapshot report that records inventory for the passed 60 days
* Added configuration which allows failed upload document to be re-uploaded again for a certain number of attempts

**2.15.2**

* Added Email notification settings to a customer level. Emailing Packslips, BOLs, RMAs

**2.15.1**

* Added support for latest Android SDK version
* Updated Handheld API to allow new permissions for Photo capture

**2.14.5**

* Added Password policy support
* Added automated password expiration and recovery

**2.14.4**

* Added new settings to Customer/Client/Global which control whether Carton content labels are required
* Added new settings to Customer/Client/Global which control whether to print Packslip upon generating a Carrier label
* Enhanced security by adding Access controls to module details screen
* Added failed login throttling

**2.14.3**

* Added Pallet printing template and ability to print pallets from Staging/Truckload/Pickticket views
* Added configuration which controls aggregator for pallet numbering
* Added configuration on customer/client/global levels which control whether pallets are required to be printed
* Added expiry inbound and outbound allowances on a product level
* Added UoM column to expiry grid

**2.14.2**

* Added additional dropship information on Pickticket header and line level
* Reworked Webhook, multiple webhooks are not supporting including Client specific Webhooks
* Added additional check to document level to prevent duplicated document numbering

**2.14.1**

* Changed allocation precedence in bulk bins from LPN name to LIFO
* Added Gauges to reporting
* Extended Dashboard filter capabilities to support multiple data types (date, string, number)
* Added support for additional columns in grids (Currency, Percentage)
* Added additional Handheld screen - Tote content

**2.13.1**

* Android 13 support (API level 33)
* Added stage location contents lookup

**2.12.3**

* Added Four wall report
* Added configurations that control which information is sycnrhonzed and shown on Four wall report

**2.12.2**

* Added enhanced production options

**2.12.1**

* -Added scannable Tracking number to Customer Returns
* \- Added a new Carrier Pickup handheld screen
* \- In addition to SKU and UPC, added a facility to scan BarcodeValue of a Product
* Upgraded to a newer version of OS (31)

**2.11.9**

* Added additional mapping for Receiving slip
* Improved UI when creating Scheduled Reports
* Opened Comments field for changes on PO closure
* Added Comments on Receiving slip
* Added additional mapping for Receiving slip
* Improved UI when creating Scheduled Reports
* Opened Comments field for changes on PO closure - Added Comments on Receiving slip

**2.11.8**

* Added 'Auto ship small parcel' flag on a user level

**2.11.7**

* Improved Ecom dashboard to support unallocated single unit parcels
* Added ability to set carton packsize in Ecom dashboard
* Improved Small parcel screen which now allows to scan/change Carton size

**2.11.6**

* Added Scheduled reports
* Added additional fields and configurations to enrich comercial invoice report

**2.11.5**

* Added history visibility to Packsizes
* Added a way to handle totes by carrier tracking number
* Added Ecom dashboard, a new way of processing single line, single unit SmallParcel orders in bulk
* Added actionable KPIs to E-comm dashboard and Pickticket list
* Improved Inventory visility and added Task generation controls on Product details screen

**2.11.4**

* Improved Malvern integration. Added address verification and rating for Residential deliveries
* Improved Tote staging processing while shipping using small parcel
* Added new report type: Commercial Invoice that is available for both Pick tickets and Totes
* Added automatic printing of Commercial invoices for international shipments if carrier cannot do electric submission
* Extended Packsize breakdown to allow breaking to inner packs

**2.11.3**

* Added a facility to import and export bundle components
* Improved UI for bundle components management

**2.11.2**

* Introduced configuration which controls whether BOL/MBOL could be shipping without charge terms
* Added precedence facility to shipping rules when a Pick ticket resolves to more than one rule
* Added infrastructure which allows pick ticket small parcel changes to propagate to totes

**2.11.1**

* Added collection of Dims/Weight on production
* Improved truck load shipping verifications
* Prevent changing shipping method after order has been shipped

**2.10.2**

* Removed excessive links for non Administrator users
* Changed Apply tags screen to be more user friendly
* Created restock user task automation for min/max bins

**2.10.1**

* Discontinued Kitting, Production should be used instead
* Reworked 3PL Billing
* Added a way to copy shipping settings to totes from pick ticket
* Added a way to create custom 3PL invoice templates
* Added a way to pick multiple Picktickets using Full Pack Picking at the same time

**2.09.2**

* Modified UI to aid with long running operations
* Improved performance of shipping of BOLs and Master BOLs
* Optimized and improved visibility of Auto picking for e-commerce orders

**2.09.1**

* Introduced a new step in Small Parcel shipping: Carrier pickup and manifest
* Removed redundant link between Truck Loads and Pick tickets
* Removed unused 'Mark for cycle count' option on Handheld skipping
* Added a new way to auto generate truck loads based on grouping algorithm

**2.08.6**

* Added 'History search' utility which allows searching deleted records
* Moved 'Export to XLSX' and 'Reset Grid' button further apart
* Moved ability to build packsizes duriong production
* Added ability to suspect orders that were shorted during picking
* Added two additional fields to pick ticket
* Appt number and Appt date
* Relaxed restriction on Production orders to have duplicate Consume/Produce lines

**2.08.5**

* Introduced a new configuration setting which controls whether shorted orders are allowed to be shipped
* Added Bill To information to Shipping rules

**2.08.4**

* Introduced a shipping options default facility based on shipping rules (client/customer/scac/zip)
* Per above, removed all shipping related default that used to be setup on the customer level
* Allowed multiple pick tickets (same customer/address) to be full pack picked to the same pallet
* Added Bin quarantine override for allocation
* QR Code scanning is always enabled for 'sa' user

**2.08.3**

* Added a way to create custom Packslip/Proforma/Receiving/RMA reports
* Above reports could be applied to Tenant -> Client -> Customer/Vendor

**2.08.2**

* Added LotNumber field to PO line to force receiving specific lots
* Added a set of configurations which controls allowing custom document numbering
* Upgraded to a newer version of EasyPost api

**2.08.1**

* Optimized performance of full pack picking
* Improved cartonization screen, added additional relevant columns
* Implemented new mobile app versioning to support Update notification

**2.07.1**

* Emergency fix to Handheld to supress new version check
* Extension to Production
* Surfaced additional fields on Production List to show finished/disassembled product

**2.06.1**

* Reworked Production module. Old production is now Kitting
* Added additional handheld screens to support new production
* Added additional configurations that drive production
* Move Workflows to Setup

**2.05.1**

* General cumulative fixes

**2.04.1**

* Enabled ability to do packsize breakdown to an LPN instead of a pickable bin

**2.03.1**

* Additional work to enable Malvern shipping
* Improved performance while handling large, cartonized orders

**2.02.1**

* Added Commercial Invoice generation for Small Parcel shipping
* Added a way to reprint Commercial invoice from Web UI 2.01.2
* Major version update
* Substantial changes to backend infrastructure
* Address change to Subdomains
* Added Direct Move Bin suggestion flag will propose bins on product moves

**1.45.1**

* Added configuration that controls maximum ordered quantity for full pack picking
* Improved performance of Pick Ticket details screen

**1.44.1**

* Added quoted weight fields to capture quoted shipping costs
* Added invoiced weight/cube fields to capture actual invoiced costs
* Added Print LPNs option on Staging LPNs screen
* Improved Cartonization performance for order needed a large number of boxes

**1.43.1**

* Extended Lot allocation capabilitiles on Pick Ticket line level
* Added Background allocation for Packsize breadkdown/LPN Letdown to improve performance

**1.42.1**

* Added a way to record an invoice number when capturing shipping costs on Truck load
* Added a configuration Client/Customer/Vendor which controls Back order creation for PO/Pick tickets

**1.41.1**

* Added additional fields to product master needed to generate customs documentation
* Added automatic Packslip and Truckload printing

**1.40.1**

* Added a facility to track quoted and actual shipping costs for Truck Loads
* Added a way to add additional shipping options (Residential, SOD, etc...) for small parcel

**1.39.1**

* Added expanded set of configuration for Small Parcel shipping
* Added 3rd Party shipping/billing options
* Added various payment types when shipping Small Parcel
* Added ability to support international shipping using Int. Tax Id

**1.38.1**

* Added Malvern small parcel shipping broker - Moved Malvern/EasyPost small parcel configuration to a client level

1.37.2

* Fixed Directed putaway query to consume different parameters
* Fixed Android GPS collection when screen goes/device goes to sleep

**1.37.1**

* Added an option to Unpick whole Pick tickets instead of just one Tote
* Added a facility to assign zones and bins to a client
* Added a facility to specify allowed product categories for zones/bins
* Added Receiving Put-a-way Bin suggestion flag - will propose bins during PO receiving
* Added Returns Put-a-way Bin suggestion flag - will propose bins during RMA receiving
* Added section to Configuration which contains SQL queries that drive put-a-way logic
* Added Production Workflow management
* Added Workflow designer and ability to assign workflows to BOM/Client

**1.36.1**

* Moved Audit records from Mongo DB to SQL for easier reporting
* Added ShipTo name in Pick ticket/Tote selection while building Truck loads
* Added Staging LPNs report for visibility into truck load planning
* Added a setting which controls whether a BOL document could be printed without signature or not .

**1.35.1**

**-** Added ability to restrict expiring product on PO and RMA Receiving

\- Added additional configuration settings that control expiry allowance

\- Added settings to Vendor and Customer that control expiry allowance

**1.34.1**

* Added Percentage picked to Pick ticket header
* Added a 'Back to Picking' function which allows taking shorted Rating orders back to picking
* Added new Handheld function LPN merge which allows consolidating staged LPNs .

**1.33.1**

* **Added new handheld function to Adjust Out full bin/LPN without scanning product**
* **Added ability to download most recent APK directly without Google Play Store**
* **Added Help button with Reseller's information**
* **Added additional information fields to pickticket detail screen to help with load planning**
* **Added additional information to packslip report**

1.32.1

* **Added facility to assign/unassign truck loads to pick tickets even before pick tickets are picked or allocated**
* **Added a separate setting for handheld weight/dims from default reporting UoM**

1.31.1 - **September 25th, 2021**

* **Added total number of cartons to Packslip report**
* **Cosmetic fixes to BOL report**
* **Added facility to create Master BOL for multiple customers**
* **Added a restriction for zone names. Now cannot create zones with the same name**
* **Added an option to do Bulk Updates to Pick tickets and Purchase orders**
* **Added facility to configure dimensions data to Bins/Zones**
* **Added new handheld screens to capture dimensions and weight of Totes and LPNs**

**1.30.1**

* **Added new handheld tag list screen to fulfillment**

1.29.1

* **Added facility to activate and deactivate users to temporarily prohibit access to the system**
* **Added Appointment date to pick tickets**
* **Added Must Arrive date to pick tickets**
* **Enabled printing tags from the pick ticket list**
* **Made comments editable past Draft state**
* **Surfaced comments in the pick ticket grid**
* **Added facility to run an import over existing tenants to retain previous settings (ex: integrations or print station)**

**1.28.1**

* **Decoupled Email sending to improve PDT performance when emails are enabled**
* **Improved Allocation performance on large orders**

**1.27.1**

* **Added facility to change allocated quantities and partially release reserved stock.**
* **Added facility to Unallocate individual lines.**
* **Carrier now propagates to Pick Ticket and Tote when shipping Truck loads.**

**1.26.1**

* **Added pick skipping to Production picking.**
* **Minor changes to component consumptions in Production when using By Recipe option.**
* **Added Handheld activity dashboard.**
* **Added facility to redirect handheld to an alternative cloud server on tenant selection.**

**1.25.1**

* **Added facility to maintain min/max inventory per Bin for product dedicated bins.**
* **Added a way to preview handheld screen of an active user session.**

**1.24.1**

* **Added ability to setup zone with Single Bin per Product (to support POS).**
* **Added ability to setup bins with dedicated products (to support POS).**

#### **1.23.1**

* **Extended support for older version of Android (Lollipop SDK v19)**
* **Added a configuration that controls whether to display Product locations on Product Move**
* **Added Skip button to Product Letdown handheld option**
* **Modified LPN Letdown to display all LPNs that are in the queue instead of just the first one**

#### **1.22.1**

* **Added facility to re-allocate orders that have already started being picked**
* **Added facility to move allocated stock (based on configuration)**
* **Added Quantity prompt on Packsize Break down handheld function**

**1.21.1**

* **Added additional Packsize handling option to Zones/Bins. Zones/Bins could be setup to auto break Packs to Eaches**
* **Added Letdown by product handheld function to avoid pulling down the whole pallet**

**1.20.1**

* **Introduced new Picking mode**
  * Carton Picking (pick individual cartonized boxes) - Renamed former 'Carton Pick' to 'Full pack picking'
  * Added additional allocation flavor for Packsize controlled items
