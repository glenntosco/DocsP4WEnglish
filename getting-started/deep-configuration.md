# Deep Configuration

## Overview

This guide provides detailed configuration instructions for P4 Warehouse system settings, automation rules, and optimization strategies. These settings control how P4 Warehouse operates and can significantly impact efficiency and accuracy.

{% hint style="info" %}
**Important**: Configuration changes can affect all users and operations. Test changes in a non-production environment when possible. For complete documentation, visit `https://docs.p4.software`
{% endhint %}

## Table of Contents

* [Initial System Setup](deep-configuration.md#initial-system-setup)
* [Warehouse Configuration](deep-configuration.md#warehouse-configuration)
* [Zone & Bin Configuration](deep-configuration.md#zone--bin-configuration)
* [Product Configuration](deep-configuration.md#product-configuration)
* [User & Permission Management](deep-configuration.md#user--permission-management)
* [Automation Settings](deep-configuration.md#automation-settings)
* [Email & Notifications](deep-configuration.md#email--notifications)
* [Printing Configuration](deep-configuration.md#printing-configuration)
* [3PL/Multi-Company Settings](deep-configuration.md#3plmulti-company-settings)
* [Integration Configuration](deep-configuration.md#integration-configuration)
* [Advanced Settings](deep-configuration.md#advanced-settings)

***

## Initial System Setup

### Critical Setup Sequence

{% hint style="danger" %}
**CRITICAL**: Follow this exact sequence to avoid configuration issues. Some settings cannot be changed after transactions exist.
{% endhint %}

```
1. 3PL/Multi-Company (if applicable)
2. Warehouses
3. Zones
4. Bins
5. Products
6. Customers/Vendors
7. Users
8. Automation Rules
```

### System-Wide Settings

**Path**: `Setup > System > Configuration`

#### Business Settings

```
General:
├── Company Name: [Your Company]
├── Time Zone: [Select appropriate]
├── Date Format: MM/DD/YYYY or DD/MM/YYYY
├── Currency: USD (or local)
└── Language: English (default)

Operating Hours:
├── Start Time: 06:00
├── End Time: 18:00
├── Working Days: Mon-Fri
└── Holiday Calendar: [Import holidays]
```

#### Number Sequences

**Path**: `Setup > System > Configuration > Business > Number Sequences`

| Sequence Type  | Format            | Example       | Reset   |
| -------------- | ----------------- | ------------- | ------- |
| Pick Ticket    | PT-{YYYY}-{00000} | PT-2024-00001 | Yearly  |
| Purchase Order | PO-{YYYY}-{00000} | PO-2024-00001 | Yearly  |
| RMA            | RMA-{00000}       | RMA-00001     | Never   |
| License Plate  | LP{0000000}       | LP0000001     | Never   |
| Cycle Count    | CC-{YYYYMM}-{000} | CC-202401-001 | Monthly |

***

## Warehouse Configuration

### Warehouse Setup

**Path**: `Setup > System > Warehouses`

#### Essential Settings

```
Warehouse Information:
├── Warehouse Code: [Must match ERP if integrated]
├── Warehouse Name: [Descriptive name]
├── Type: Standard/Bonded/Temperature Controlled
└── Default Language: [For multi-language support]

Address:
├── Complete physical address
├── Time zone (if different from system)
├── Contact information
└── Operating hours
```

#### Warehouse-Specific Configurations

**Receiving Settings:**

* Default receiving zone
* Appointment scheduling: Enabled/Disabled
* Blind receiving: Yes/No
* Photo requirements: None/Optional/Required
* Cross-dock enabled: Yes/No

**Shipping Settings:**

* Default shipping zone
* Carrier assignments
* Document requirements
* Photo/signature requirements

***

## Zone & Bin Configuration

### Zone Types and Strategy

**Path**: `Warehouse > Edit > Zones`

#### Zone Type Configuration

| Zone Type      | Purpose             | Configuration                 |
| -------------- | ------------------- | ----------------------------- |
| **RECEIVING**  | Inbound processing  | Temporary storage, no picking |
| **STAGING**    | Order consolidation | No long-term storage          |
| **SHIPPING**   | Outbound processing | Final QC area                 |
| **PICKING**    | Active inventory    | Primary pick locations        |
| **BULK**       | Overstock/Reserve   | Replenishment source          |
| **RETURNS**    | RMA processing      | Inspection/quarantine         |
| **QUARANTINE** | Hold/damaged        | Restricted access             |

#### Zone Settings

```
Zone Configuration:
├── Zone Code: [A, B, C or RECV, SHIP, etc.]
├── Zone Type: By Product / By LPN
├── Temperature: Ambient/Cooler/Freezer
├── Security: Standard/Restricted/High Security
├── Pick Sequence: [Numeric order for routing]
└── Default Printer: [Zone-specific printer]

Special Attributes:
├── Hazmat Certified: Yes/No
├── Food Grade: Yes/No
├── Bonded: Yes/No
├── High Value: Yes/No
└── Flammable Storage: Yes/No
```

### Bin Configuration Strategy

#### Bin Naming Conventions

**Standard Format**: `[Zone]-[Aisle]-[Rack]-[Level]`

Examples:

* `A-01-01-A` (Zone A, Aisle 01, Rack 01, Level A)
* `RECV-01` (Receiving Zone, Position 01)
* `SHIP-STAGE-01` (Shipping Staging, Position 01)

#### Bin Mask Configuration

**Path**: `Zone > Edit > Bin Mask`

```
Common Patterns:
├── {0:00}-{1:00}-{2:A} → 01-01-A, 01-02-B
├── {0:A}-{1:00}-{2:0} → A-01-1, B-02-2
├── STAGE-{0:000} → STAGE-001, STAGE-002
└── {0:AA}{1:00}{2:0} → AA01-1, AB02-2
```

#### Bin Attributes

```
Physical Properties:
├── Height (inches/cm)
├── Width (inches/cm)
├── Depth (inches/cm)
├── Weight Capacity (lbs/kg)
└── Volume (cu ft/m³)

Restrictions:
├── Single SKU Only: Yes/No
├── Single Lot Only: Yes/No
├── Mixing Allowed: Yes/No
├── Packsize Handling: Auto-break/Maintain
└── Reserved For: [Specific product/customer]
```

***

## Product Configuration

### Product Master Settings

**Path**: `Setup > Products`

#### Required Fields Configuration

```
Mandatory Fields:
├── SKU: [Unique identifier]
├── Description: [Clear, concise]
├── Barcode: [Auto-generate or manual]
├── Client: [If multi-company]
└── Status: Active/Inactive

Tracking Options:
├── Lot Tracking: Yes/No
├── Serial Tracking: Yes/No
├── Expiry Tracking: Yes/No
├── Catch Weight: Yes/No
└── Decimal Quantities: Yes/No
```

#### Dimensional Settings

```
Standard Products:
├── Length × Width × Height
├── Weight (lbs/kg)
├── Units per Case
└── Cases per Pallet

Decimal Products:
├── Unit of Measure: KG/LB/L/GAL/M/FT
├── Minimum Sellable Qty: 0.01
├── Rounding Rule: 0.01/0.1/1
└── Variance Tolerance: 2%
```

### Product Categories

**Path**: `Setup > Products > Categories`

Organize products for reporting and slotting:

* Fast Moving / Slow Moving
* High Value / Low Value
* Hazmat / Non-Hazmat
* Temperature Requirements
* Customer Specific

***

## User & Permission Management

### User Roles Matrix

**Path**: `Setup > System > Users`

| Role              | Web Access | Mobile Access | Permissions                      |
| ----------------- | ---------- | ------------- | -------------------------------- |
| **Admin**         | ✓          | ✓             | Full system access               |
| **Manager**       | ✓          | ✓             | All operations, no system config |
| **Dispatcher**    | ✓          | ✗             | Order management, reporting      |
| **Receiver**      | ✗          | ✓             | Receiving, putaway               |
| **Picker**        | ✗          | ✓             | Picking, packing                 |
| **Cycle Counter** | ✗          | ✓             | Cycle count only                 |
| **Viewer**        | ✓          | ✗             | Read-only access                 |

### Module Permissions

```
Available Modules:
├── Receiving
│   ├── Create PO
│   ├── Receive
│   ├── Putaway
│   └── Print Labels
├── Fulfillment
│   ├── Create Orders
│   ├── Allocate
│   ├── Wave Management
│   └── Shipping
├── Inventory
│   ├── Adjustments
│   ├── Transfers
│   ├── Cycle Count
│   └── Quarantine
├── Reports
│   ├── View Reports
│   ├── Create Reports
│   └── Export Data
└── Administration
    ├── User Management
    ├── System Config
    └── Integration Setup
```

### Password & Security Settings

```
Password Policy:
├── Minimum Length: 8 characters
├── Complexity: Upper + Lower + Number + Special
├── Expiration: 90 days
├── History: Last 5 passwords
└── Lockout: 5 failed attempts

Session Settings:
├── Web Timeout: 30 minutes
├── Mobile Timeout: 8 hours
├── Concurrent Sessions: 1 per user
└── Force Logout: Daily at midnight
```

***

## Automation Settings

### Fulfillment Automation

**Path**: `Setup > System > Configuration > Business > Fulfillment > Automation`

```
Order Processing:
├── Auto-Allocate on Release: Yes/No
├── Allocation Method: FIFO/FEFO/LIFO
├── Auto-Wave After Allocation: Yes/No
├── Wave Size: 20-30 orders
├── Auto-Generate Backorders: Yes/No
└── Auto-Close After Ship: Yes/No

Pick Ticket Settings:
├── Auto-Print on Wave: Yes/No
├── Group by Zone: Yes/No
├── Sort by Location: Yes/No
└── Batch Pick Threshold: 5 orders
└── Cartonization: Auto/Manual
```

### Receiving Automation

```
PO Processing:
├── Auto-Create Bins: Yes/No
├── Auto-Assign Locations: Yes/No
├── Default Putaway Strategy: Directed/Suggested
├── Auto-Close PO: When fully received
├── Generate Labels: Automatic
└── Quality Hold: None/All/By Product

LPN Settings:
├── Auto-Generate LPN: Yes/No
├── LPN Format: LP{0000000}
├── Maintain on Putaway: Yes/No
└── Allow Mixing: Yes/No
└── Print on Creation: Yes/No
```

### Inventory Automation

```
Cycle Count:
├── Auto-Generate Daily: Yes/No
├── Count per Day: 20-30 locations
├── Approval Required: Yes/No
├── Variance Threshold: 5%
├── Auto-Recount: If variance > threshold

Replenishment:
├── Auto-Generate Tasks: Yes/No
├── Min/Max Check: Daily at 6 AM
├── Priority: By velocity
├── Task Assignment: Auto/Manual
└── Confirmation Required: Yes/No
```

***

## Email & Notifications

### SMTP Configuration

**Path**: `Setup > System > Configuration > Email`

```
Server Settings:
├── SMTP Server: smtp.office365.com
├── Port: 587
├── Security: TLS/SSL
├── Authentication: Yes
├── Username: [email address]
└── Password: [encrypted]

From Address:
├── Display Name: P4 Warehouse System
├── Email: noreply@company.com
├── Reply-To: warehouse@company.com
└── Test Email: [Send test]
```

### Notification Templates

**Path**: `Setup > System > Configuration > Notifications`

| Event         | Recipients | Trigger      | Template             |
| ------------- | ---------- | ------------ | -------------------- |
| PO Received   | Purchasing | On close     | Receipt confirmation |
| Order Shipped | Customer   | On ship      | Tracking info        |
| Low Stock     | Purchasing | < Min level  | Reorder alert        |
| Cycle Count   | Supervisor | Variance >5% | Approval needed      |
| Expiry Alert  | Quality    | 30 days      | Action required      |

### Customer Notifications

```
Order Confirmations:
├── Send on Release: Yes/No
├── Include Pick Ticket: No
├── Format: PDF/HTML
└── Language: Per customer

Shipping Notifications:
├── Send on Ship: Yes
├── Include Packing Slip: Yes
├── Include Tracking: Yes
├── Include Invoice: Per customer
└── ASN Format: EDI/Email/Both
```

***

## Printing Configuration

### Print Agent Setup

**Requirements:**

* Windows Server/PC (always on)
* Network access to printers
* P4 Print Agent service installed
* Firewall exceptions configured

**Path**: `Setup > System > Printing > Print Agent`

```
Agent Configuration:
├── Server Name: PRINTSERVER01
├── Port: 9100
├── Status: Running (Windows Service)
├── Auto-Start: Yes
└── Error Notifications: warehouse@company.com
```

### Printer Management

**Path**: `Setup > System > Printing > Printers`

| Printer Name  | Type  | DPI | Location  | Default For     |
| ------------- | ----- | --- | --------- | --------------- |
| ZEBRA-RECV-01 | Label | 203 | Receiving | Product labels  |
| ZEBRA-PICK-01 | Label | 203 | Pick Area | Pick tickets    |
| ZEBRA-PACK-01 | Label | 300 | Packing   | Shipping labels |
| HP-OFFICE-01  | Laser | 600 | Office    | Reports, BOL    |

### Label Configuration

```
Label Types:
├── Product Label (4x2)
│   ├── SKU + Barcode
│   ├── Description
│   ├── Lot/Expiry (if applicable)
│   └── QR code (optional)
├── Location Label (4x2)
│   ├── Location barcode
│   ├── Zone-Aisle-Rack-Bin
│   └── Capacity info
├── Shipping Label (4x6)
│   ├── Carrier format
│   ├── Tracking barcode
│   └── Return address
└── Pallet Label (8.5x11)
    ├── LPN barcode
    ├── Content list
    └── Destination
```

***

## 3PL/Multi-Company Settings

### Client Configuration

**Path**: `Setup > 3PL > Clients`

{% hint style="danger" %}
**CRITICAL**: Must be configured BEFORE creating products or transactions
{% endhint %}

```
Client Setup:
├── Client Code: [Unique 3-5 chars]
├── Client Name: [Full company name]
├── Type: Standard/VIP/Trial
├── Status: Active/Inactive/Hold
└── SSCC Prefix: [7-9 digits for labels]

Billing Settings:
├── Enable Billing: Yes/No
├── Billing Cycle: Monthly/Bi-weekly/Weekly
├── Billing Day: 1st/15th/Monday
├── Auto-Post Invoice: Yes/No
├── Payment Terms: Net 30/45/60
└── Currency: USD
```

### Billing Profile Configuration

**Path**: `Setup > 3PL > Billing Profiles`

```
Service Charges:
├── Receiving
│   ├── Per Receipt: $X.XX
│   ├── Per Line: $X.XX
│   ├── Per Unit: $X.XX
│   └── Per Pallet: $X.XX
├── Storage
│   ├── Per Pallet/Day: $X.XX
│   ├── Per Bin/Month: $X.XX
│   ├── Per Cubic Ft: $X.XX
│   └── Minimum Charge: $X.XX
├── Picking
│   ├── Per Order: $X.XX
│   ├── Per Line: $X.XX
│   ├── Per Unit: $X.XX
│   └── Rush Fee: $X.XX
└── Special Services
    ├── Labeling: $X.XX per label
    ├── Kitting: $X.XX per kit
    ├── Returns: $X.XX per RMA
    └── Cycle Count: $X.XX per count
```

### Client Segregation

```
Inventory Segregation:
├── Separate Zones: Yes (recommended)
├── Shared Bins: No
├── Label Prefix: Client code
└── Report Filtering: Automatic

User Access:
├── Client Portal: Enabled
├── Visible Data: Own only
├── Order Entry: Yes/No
├── Report Access: Limited
└── API Access: Separate keys
```

***

## Integration Configuration

### API Settings

**Path**: `Setup > System > Configuration > API`

```
API Configuration:
├── Base URL: https://api.p4warehouse.com
├── Version: v1
├── Rate Limit: 100 req/minute
├── Timeout: 30 seconds
└── Retry Attempts: 3

Authentication:
├── Method: Bearer Token
├── Token Expiry: 24 hours
├── Refresh Token: Enabled
└── IP Whitelist: [Optional]
```

### Webhook Configuration

```
Webhook Events:
├── Order Events
│   ├── Order Created
│   ├── Order Allocated
│   ├── Order Picked
│   └── Order Shipped
├── Inventory Events
│   ├── Stock Level Change
│   ├── Low Stock Alert
│   └── Expiry Alert
└── System Events
    ├── Integration Error
    ├── Cycle Count Complete
    └── PO Received
```

### EDI Settings

```
EDI Configuration:
├── Provider: [EDI VAN Name]
├── AS2 ID: [Your ID]
├── Certificates: [Upload]
├── Document Types:
│   ├── 850 - Purchase Order
│   ├── 856 - ASN
│   ├── 810 - Invoice
│   └── 997 - Acknowledgment
└── Schedule: Every 15 minutes
```

***

## Advanced Settings

### Performance Optimization

**Path**: `Setup > System > Configuration > Performance`

```
Database Settings:
├── Connection Pool: 100
├── Query Timeout: 120 seconds
├── Archive After: 2 years
├── Purge After: 7 years
└── Index Rebuild: Weekly Sunday 2 AM

Caching:
├── Enable Redis: Yes
├── Cache Duration: 5 minutes
├── Session Cache: 30 minutes
└── Clear Cache: Manual/Scheduled
```

### Audit & Compliance

```
Audit Settings:
├── Track All Changes: Yes
├── Retention Period: 7 years
├── Include Read Access: No
├── User Activity Log: Yes
└── Export Format: CSV/PDF

Compliance:
├── FDA 21 CFR Part 11: Enabled
├── GMP Requirements: Yes
├── Electronic Signatures: Required
├── Change Control: Approved
└── Validation Documents: Maintained
```

### Backup & Recovery

```
Backup Configuration:
├── Frequency: Daily at 2 AM
├── Type: Full + Incremental
├── Retention: 30 days local, 1 year archive
├── Location: Cloud + Local NAS
├── Test Restore: Monthly
└── Notification: IT team email

Disaster Recovery:
├── RPO: 1 hour
├── RTO: 4 hours
├── Failover: Manual/Automatic
├── DR Site: [Location]
└── Test Schedule: Quarterly
```

### Custom Fields

**Path**: `Setup > System > Configuration > Custom Fields`

```
Available Objects:
├── Products
│   ├── Custom1: Country of Origin
│   ├── Custom2: Commodity Code
│   └── Custom3: Special Instructions
├── Orders
│   ├── Custom1: Customer PO
│   ├── Custom2: Department
│   └── Custom3: Cost Center
└── Customers
    ├── Custom1: Account Type
    ├── Custom2: Credit Limit
    └── Custom3: Special Requirements
```

***

## Configuration Best Practices

### Change Management

1. **Document all changes** with date, who, what, why
2. **Test in sandbox** before production
3. **Notify affected users** before changes
4. **Schedule during off-hours** for major changes
5. **Have rollback plan** ready
6. **Monitor after changes** for issues

### Regular Review Schedule

**Daily:**

* Check Print Agent status
* Review error logs
* Monitor integration status

**Weekly:**

* Review automation performance
* Check user access logs
* Validate backup completion

**Monthly:**

* Review configuration settings
* Update user permissions
* Clean up obsolete data
* Review billing profiles (3PL)

**Quarterly:**

* Full configuration audit
* Performance optimization
* Security review
* DR test

***

{% hint style="success" %}
**Remember**: Good configuration is the foundation of efficient warehouse operations. Take time to set up properly initially, and review regularly to ensure settings still meet your business needs.
{% endhint %}
