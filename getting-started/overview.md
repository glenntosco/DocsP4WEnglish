---
description: P4 Warehouse Overview
---

# Overview

## User types

In P4 Warehouse there are two completely unique styles of users.

1. &#x20;**Web Dispatcher**, the web dispatcher uses a computer and a modern web browser to manage the flow in and out of the warehouse. The web dispatcher also controls the workflow of the PDT users. Think of the web dispatches as the control tower at an airport.
2. **PDT user**, the PDT user is the people out in the warehouse executing the instructions set to them from the Web Dispatcher. These are the people physically receiving Purchase Order and RMA's, picking and shipping outbound orders, Cycle Counting inventory. These users work from a mobile device typically with a barcode scanner.

You will need a good amount warehouse / WMS experience to use this document, contact your P4 Warehouse sales partner if you run into subjects, you do not fully understand.

[Here you can find the support contact information for your country.](../contact-us/support/)

## Android

Android Nuget and above will function with the P4 Warehouse System, P4W Handheld can be downloaded to an Android device from [https://play.google.com/store/apps/details?id=com.pro4soft.p4w\&hl=en\_US\&gl=US](https://play.google.com/store/apps/details?id=com.pro4soft.p4w\&hl=en_US\&gl=US)

It is recommended the Android device has a built-in barcode scanner, hardware from Zebra Technologies works best as the P4 Warehouse WMS is validated by Zebra to function correctly on their Android Mobile Computers.



## Definitions

There are specific words related to the logistics industry that are sometimes not clearly understood, below are some of the definitions of words you will find in this documentation:

**Web Dispatcher** — A user who works from a computer using a web browser to manage warehouse operations, create orders, configure the system, and monitor warehouse floor activities.

**PDT User** — Portable Data Terminal user; warehouse floor workers who use Android mobile devices with barcode scanners to execute warehouse tasks (receiving, picking, shipping, cycle counting).

**SKU** — Stock Keeping Unit; a unique product identifier used to track inventory.

**LPN** — License Plate Number; a unique identifier for a pallet, carton, or container that groups multiple products together for tracking and movement.

**Pick Ticket** — An order document that instructs warehouse workers which products to pick, quantities, and locations.

**Wave** — A group of pick tickets released together for picking, typically organized by zone, carrier, or ship date.

**Allocation** — The process of reserving inventory for specific orders before picking begins.

**Cartonization** — The automated process of determining optimal carton sizes and packing configurations for outbound orders.

**Putaway** — Moving received inventory from the dock to its designated storage location.

**Cycle Count** — A regular inventory counting process that verifies physical stock matches system records without requiring a full warehouse shutdown.

**Lot Control** — Tracking inventory by production batch or lot number, typically for traceability and quality control.

**Serial Control** — Tracking inventory at the individual unit level using unique serial numbers.

**Expiry Control** — Managing products with expiration dates to ensure FIFO/FEFO rotation and prevent shipping expired goods.

**Packsize** — P4 Warehouse's 5-level unit of measure hierarchy (Each, Inner Pack, Case, Pallet, Container) that can include decimal quantities for weight-based products.

**3PL** — Third-Party Logistics; a warehouse operator that stores and fulfills orders for multiple client companies.

**BOM** — Bill of Materials; a list of components required to manufacture a finished product (used in production, not fulfillment).

**Product Bundle** — A single SKU that represents multiple component products for fulfillment purposes (pick ticket shows components, not the bundle SKU).

**ASN** — Advanced Shipping Notice; electronic notification sent before a shipment arrives, containing product details and quantities.

**BOL** — Bill of Lading; a legal document between shipper and carrier detailing the shipment contents.

**Cross-Dock** — Receiving inventory and immediately shipping it out without putaway to storage, minimizing handling time.

**FIFO** — First In, First Out; inventory rotation method where oldest stock is picked first.

**FEFO** — First Expired, First Out; inventory rotation method where products with the earliest expiration dates are picked first.

**Reason Code** — A system-defined code used to document why an adjustment, return, or inventory change occurred.

**Tote** — A container used to consolidate picked items for an order during the picking and staging process.

**Dock Door** — A designated loading/unloading area where trucks are assigned for receiving or shipping.

**Zone** — A logical subdivision of warehouse space used for organizing inventory and optimizing workflows.

**Bin** — A specific storage location within a zone where inventory is stored (shelf, rack position, floor location).

For a complete glossary of supply chain terms, see [Supply Chain Glossary](supply-chain-glossary.md).

