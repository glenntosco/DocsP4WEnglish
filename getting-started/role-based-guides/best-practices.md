# Best Practices

## Overview

This guide provides proven best practices for optimizing warehouse workflows in P4 Warehouse. These practices are based on industry standards and successful implementations across various warehouse operations.

{% hint style="info" %}
**Note**: Adapt these practices to your specific operation. What works for high-volume e-commerce may differ from 3PL or manufacturing workflows. For complete documentation, visit `https://docs.p4.software`
{% endhint %}

## Table of Contents

* [Receiving Best Practices](best-practices.md#receiving-best-practices)
* [Putaway Strategies](best-practices.md#putaway-strategies)
* [Picking Optimization](best-practices.md#picking-optimization)
* [Packing & Shipping](best-practices.md#packing--shipping)
* [Cycle Counting Strategies](best-practices.md#cycle-counting-strategies)
* [Returns Processing](best-practices.md#returns-processing)
* [Cross-Docking Operations](best-practices.md#cross-docking-operations)
* [Inventory Management](best-practices.md#inventory-management)
* [3PL Operations](best-practices.md#3pl-operations)
* [Performance Metrics](best-practices.md#performance-metrics)

***

## Receiving Best Practices

### Pre-Receiving Preparation

#### Appointment Scheduling

```
Best Practice: Schedule receiving appointments to balance workload
```

**Implementation:**

1. Use P4 Warehouse appointment system
2. Allocate 30-minute windows per vendor
3. Stagger LTL and parcel deliveries
4. Reserve dock doors by carrier type

**Benefits:**

* Reduced dock congestion
* Better labor planning
* Improved carrier relations
* Faster unloading

### Receiving Process Flow

#### Optimal Receiving Workflow

```
1. Pre-Receive (Office)
   ├── Print PO labels
   ├── Assign receiver
   └── Reserve dock door

2. Physical Receipt (Dock)
   ├── Verify seal numbers
   ├── Photo document condition
   ├── Count cartons/pallets
   └── Begin scanning

3. System Receipt (PDT)
   ├── Scan PO label
   ├── Scan products
   ├── Enter quantities
   └── Assign locations

4. Quality Check
   ├── Verify counts
   ├── Check damages
   ├── Confirm locations
   └── Close PO
```

### Best Practices by Receiving Type

| Receiving Type        | Best Practice                  | Why It Works                 |
| --------------------- | ------------------------------ | ---------------------------- |
| **Blind Receiving**   | Don't show expected quantities | Forces accurate counting     |
| **Cross-Dock**        | Direct to shipping zone        | Eliminates putaway step      |
| **LPN Receiving**     | Assign LPN at dock             | Maintains pallet integrity   |
| **Mixed SKU Pallets** | Break down immediately         | Prevents picking errors      |
| **Decimal Products**  | Use calibrated scales          | Ensures weight accuracy      |
| **Expiry Products**   | Check dates at receiving       | Prevents expired stock entry |

### Receiving Special Product Types

#### Decimal/Weight-Based Products

**Receiving Process:**

1. Tare weight container/packaging
2. Weigh product on calibrated scale
3. Record weight to 2 decimal places
4. Apply variance tolerance (±2%)
5. Label with exact weight received

**Common Weight-Based Products:**

* Bulk chemicals (gallons/liters)
* Fresh produce (kg/lbs)
* Wire/cable (meters/feet)
* Fabric (yards/meters)
* Hardware (nuts/bolts by weight)

#### Expiry-Dated Products

**Receiving Requirements:**

1. **Mandatory Date Capture**
   * Scan/enter expiry date
   * System validates against minimum shelf life
   * Auto-reject if less than customer allowance (default 70 days)
2.  **Labeling Standards**

    ```
    Label Format:
    SKU: [Product SKU]
    Lot: [Lot Number]
    Exp: MM/YYYY (clearly visible)
    Received: [Date]
    ```
3. **Putaway Rules**
   * Segregate by expiry month
   * Place newer dates behind older
   * Mark short-dated items clearly

### Common Receiving Mistakes to Avoid

❌ **Don't:**

* Receive without labels on products
* Mix multiple POs on same pallet
* Skip damage documentation
* Close PO before putaway complete
* Ignore expiry dates

✅ **Do:**

* Label everything immediately
* Segregate by PO and client
* Photo all damages
* Verify putaway completion
* Check dates on perishables

***

## Putaway Strategies

### Location Assignment Logic

#### Velocity-Based Classification

{% hint style="info" %}
**Note**: P4 Warehouse uses velocity-based slotting rather than traditional ABC classification. Products are organized by pick frequency and physical characteristics rather than rigid ABC categories.
{% endhint %}

**High-Velocity Items (Most frequently picked)**

* Place in golden zone (waist to shoulder height)
* Closest to packing area
* Multiple locations if needed

**Medium-Velocity Items**

* Standard picking locations
* Single location per SKU
* Moderate accessibility

**Low-Velocity Items**

* Upper/lower rack positions
* Bulk storage areas
* Consolidate to save space

### Slotting Optimization

**Velocity-Based Slotting:**

1.  **Analyze Pick Frequency**

    ```sql
    -- Monthly velocity analysis
    SELECT 
        products.Sku, 
        COUNT(*) as Picks, 
        SUM(ptl.PickedQuantity) as Units
    FROM PickTicketLines ptl
        INNER JOIN PickTickets pt ON ptl.PickTicketId = pt.Id
        JOIN Products on Products.id = ptl.ProductId
    WHERE pt.ShippedDate >= DATEADD(month, -1, GETDATE())
        AND pt.PickTicketState = 'Closed'
    GROUP BY Products.Sku
    ORDER BY Picks DESC
    ```
2. **Consider Physical Characteristics**
   * Heavy items → Lower positions
   * Fragile items → Separate zone
   * High-value → Secured area
   * Hazmat → Compliant storage
3. **Family Grouping**
   * Items often ordered together
   * Same customer's products (3PL)
   * Compatible storage requirements

### Putaway Methods Comparison

| Method        | When to Use                 | Advantages                | Disadvantages          |
| ------------- | --------------------------- | ------------------------- | ---------------------- |
| **Directed**  | High volume, multiple zones | Optimal space utilization | Requires setup         |
| **Suggested** | Medium volume               | Flexibility with guidance | May not optimize       |
| **Random**    | Low SKU count               | Simple, fast              | Inefficient space use  |
| **Fixed**     | Stable inventory            | Predictable locations     | Poor space utilization |

***

## Picking Optimization

### Picking Method Selection

#### Decision Matrix

```
Order Size × Volume = Method

Single Item × Low Volume = Single Order Picking
Single Item × High Volume = Batch Picking
Multi-Item × Low Volume = Discrete Picking
Multi-Item × High Volume = Wave Picking
Many Orders × High Volume = Zone Picking
```

### Wave Picking Best Practices

#### Wave Planning Strategy

**Morning Wave (7:00 AM - 10:00 AM)**

* Priority/express orders
* Same-day shipping
* Small parcel carriers
* 2-3 hour wave duration

**Midday Wave (10:00 AM - 2:00 PM)**

* Standard orders
* LTL shipments
* Larger orders
* 4-hour wave duration

**Afternoon Wave (2:00 PM - 5:00 PM)**

* Next-day orders
* Bulk/pallet orders
* Cross-dock transfers
* 3-hour wave duration

#### Wave Configuration

```
Setup > System > Configuration > Fulfillment > Wave Settings

Recommended Settings:
├── Max orders per wave: 20-30 (adjust based on picker capacity)
├── Max picks per wave: 200-300
├── Max locations per wave: 50
├── Sort by: Location sequence
└── Print pick labels: At wave release
```

### Pick Path Optimization

#### Efficient Route Design

```
Optimal Pick Path:
Start → A-01-01 → A-02-01 → A-03-01 (down aisle A)
     → B-03-01 → B-02-01 → B-01-01 (up aisle B)
     → C-01-01 → C-02-01 → C-03-01 (down aisle C)
     → Staging/Packing

Key: Minimize backtracking, use serpentine pattern
```

### Batch Picking Optimization

**Batch Size Guidelines:**

| Order Characteristics | Optimal Batch Size | Container Type         |
| --------------------- | ------------------ | ---------------------- |
| 1-3 items per order   | 8-12 orders        | Multi-compartment cart |
| 4-10 items per order  | 4-8 orders         | Standard cart          |
| 10+ items per order   | 2-4 orders         | Pallet jack            |
| Mixed sizes           | 6-8 orders         | Flexible totes         |
| Decimal quantities    | 2-4 orders         | Scale-equipped cart    |

### Special Picking Considerations

#### Picking Decimal/Weight Products

**Weight-Based Picking Process:**

1. **Equipment Required**
   * Mobile scale or scale stations
   * Tare weight containers
   * Label printer for actual weight
2.  **Picking Steps**

    ```
    1. Scan product location
    2. Place container on scale
    3. Zero/tare scale
    4. Pick required weight (±tolerance)
    5. Print weight label
    6. Apply to container
    7. Confirm pick in system
    ```
3. **Accuracy Tips**
   * Allow 2% variance for moisture loss
   * Round to nearest 0.01 for billing
   * Document any significant variance
   * Reweigh at packing if needed

#### Picking Expiry-Dated Products

**FEFO Pick Enforcement:**

1. System shows oldest expiry first
2. Picker must scan lot/expiry label
3. System validates correct FEFO sequence
4. Override requires supervisor approval

**Near-Expiry Alerts:**

* **Red Flag**: <30 days to expiry
* **Yellow Flag**: 31-60 days to expiry
* **Customer Check**: Verify accepts short dates
* **Document**: Note any short-date shipments

### Zone Picking Implementation

#### Zone Assignment

```
Zone A: Fast-moving consumables
├── Picker 1 (Morning shift)
├── Picker 2 (Afternoon shift)
└── 500+ picks/day capacity

Zone B: Medium-velocity items
├── Picker 3 (Full shift)
└── 300 picks/day capacity

Zone C: Slow-moving/Bulk items
├── Picker 4 (As needed)
└── 100 picks/day capacity
```

#### Zone Consolidation

**Best Practice: Progressive Assembly**

1. Zone A picks → Tote 1
2. Pass to Zone B → Add items
3. Pass to Zone C → Complete order
4. Final QC at packing

***

## Packing & Shipping

### Packing Station Setup

#### Ergonomic Configuration

```
Packing Station Layout:
                 [Monitor/Label Printer]
                        [Scale]
    [Supplies]     [Pack Surface]     [Completed]
    [Boxes]         [Worker]          [Conveyor]
    [Void Fill]   [Tape Dispenser]    [Labels]
```

### Cartonization Best Practices

#### Box Size Selection

**Automated Cartonization Rules:**

```
If total_volume < 500 cu.in. → Small box
If total_volume < 1000 cu.in. → Medium box  
If total_volume < 2000 cu.in. → Large box
If weight > 30 lbs → Split into multiple boxes
If fragile = true → Add 20% volume for padding
```

#### Packing Efficiency Metrics

Target Metrics:

* Box fill rate: >65%
* Void fill usage: <15% by volume
* Packing time: <2 minutes per order
* Accuracy rate: >99.5%

### Shipping Optimization

#### Carrier Selection Logic

| Service Needed | Weight      | Distance | Best Carrier     |
| -------------- | ----------- | -------- | ---------------- |
| Next day       | <5 lbs      | Any      | Express parcel   |
| 2-day          | <150 lbs    | <500 mi  | Regional carrier |
| 3-5 day        | <150 lbs    | Any      | National parcel  |
| Economy        | >150 lbs    | Any      | LTL freight      |
| Bulk           | Full pallet | Any      | TL freight       |

#### Load Planning

**Truck Loading Best Practices:**

1. Heaviest pallets first (floor level)
2. Build stable columns
3. Similar destinations together
4. LIFO for multi-stop routes
5. Secure with load bars

***

## Cycle Counting Strategies

### ABC Cycle Count Frequency

```
A Items: Count monthly (12x per year)
B Items: Count quarterly (4x per year)
C Items: Count annually (1x per year)

Daily counts needed = (A + B + C) / Working days
Example: (1000 + 2000 + 5000) / 250 = 32 counts/day
```

### Cycle Count Scheduling

#### Trigger-Based Counting

**Automatic Triggers:**

* Zero inventory locations
* Negative inventory alerts
* High-value threshold exceeded
* Receiving discrepancies
* Customer complaints

#### Opportunistic Counting

Count when:

* Bin becomes empty
* Before replenishment
* After picking errors
* During slow periods
* When picker reports discrepancy

### Cycle Count Execution

#### Best Practice Process

```
1. Generate Count List
   ├── Print count sheets
   ├── Assign to counter
   └── Set deadline

2. Blind Count
   ├── Don't show expected quantity
   ├── Count everything in location
   └── Record all SKUs found

3. Variance Review
   ├── Recount if >5% variance
   ├── Investigate large discrepancies
   └── Document root cause

4. Approval & Adjustment
   ├── Supervisor approval required
   ├── Update inventory
   └── Track accuracy metrics
```

### Accuracy Improvement

**Target Accuracy Levels:**

* Location accuracy: >99%
* Quantity accuracy: >98%
* SKU accuracy: >99.5%

**Improvement Actions:**

1. Training on count procedures
2. Better bin labeling
3. Reduce bin sharing
4. Implement verification scans
5. Regular accuracy reporting

***

## Returns Processing

### RMA Workflow Optimization

#### Streamlined Returns Process

```
1. RMA Creation (Customer Service)
   ├── Validate return reason
   ├── Generate RMA number
   ├── Email label to customer
   └── Alert warehouse

2. Receipt (Receiving Dock)
   ├── Scan RMA barcode
   ├── Inspect condition
   ├── Photo documentation
   └── Determine disposition

3. Disposition (QC Area)
   ├── Return to stock (65%)
   ├── Repair/refurbish (20%)
   ├── Return to vendor (10%)
   └── Scrap/dispose (5%)

4. Processing (System)
   ├── Update inventory
   ├── Credit customer
   ├── Report to client
   └── Close RMA
```

### Returns Categories & Handling

| Return Type     | Inspection Level | Typical Action        | Location      |
| --------------- | ---------------- | --------------------- | ------------- |
| Unopened        | Visual only      | Restock               | Prime picking |
| Opened/complete | Full inspection  | Test & restock        | Secondary     |
| Damaged         | Document fully   | Vendor claim          | Quarantine    |
| Wrong item      | Verify error     | Restock + investigate | Prime         |
| Defective       | Test if possible | RTV or dispose        | RTV area      |

### Returns Metrics to Track

Key Performance Indicators:

* Return rate by SKU
* Processing time (receipt to disposition)
* Return to stock percentage
* Cost per return
* Return reasons analysis

***

## Cross-Docking Operations

### Cross-Dock Planning

#### Suitable Products for Cross-Docking

✅ **Ideal Candidates:**

* Pre-allocated inventory
* High-velocity items
* Promotional products
* Seasonal items
* Perishables

❌ **Poor Candidates:**

* Items requiring QC
* Mixed SKU pallets
* Small quantities
* Items needing repackaging

### Cross-Dock Execution

#### Workflow Design

```
Inbound Truck          Staging          Outbound Truck
    [1]───────────►[Sort/Scan]───────────►[1]
    [2]───────────►[Consolidate]─────────►[2]
    [3]───────────►[Label/Route]─────────►[3]

Timeline: <4 hours from receipt to ship
```

#### Space Allocation

```
Cross-Dock Layout:
├── Inbound doors: 1-6
├── Staging area: 2000 sq ft
├── Outbound doors: 7-12
└── Flow: Left to right

Staging Time Limits:
├── Parcel: 2 hours max
├── LTL: 4 hours max
└── TL: 24 hours max
```

### Success Factors

1. **Scheduling**: Coordinate inbound/outbound
2. **Communication**: Real-time updates
3. **Labor**: Dedicated cross-dock team
4. **Technology**: RF scanning throughout
5. **Space**: Adequate staging area

***

## Inventory Management

### Min/Max Level Setting

#### Calculation Formula

```
Minimum = (Average daily usage × Lead time) + Safety stock
Maximum = Minimum + Order quantity

Safety Stock = Service level factor × √(Lead time) × Standard deviation of demand

Example:
- Average daily usage: 50 units
- Lead time: 5 days  
- Safety stock: 100 units
- Order quantity: 500 units

Minimum = (50 × 5) + 100 = 350 units
Maximum = 350 + 500 = 850 units
```

#### Dynamic Adjustment Based on Velocity

Use the velocity analysis query to categorize products:

**High-Velocity (>100 picks/month):**

* Review min/max weekly
* Higher safety stock (20-30%)
* Smaller, more frequent orders

**Medium-Velocity (20-100 picks/month):**

* Review min/max monthly
* Standard safety stock (15-20%)
* Balance order frequency with carrying cost

**Low-Velocity (<20 picks/month):**

* Review min/max quarterly
* Lower safety stock (10-15%)
* Larger, less frequent orders

### Inventory Optimization Strategies

#### Slow-Moving Inventory

**Identification Query:**

```sql
-- Identify products with no movement in specified days
DECLARE @DaysWithoutMovement INT = 90;

;WITH WarehouseBins AS (
    SELECT 
        b.Id AS BinId,
        w.WarehouseCode
    FROM dbo.Bins b
    JOIN dbo.Zones z ON z.Id = b.ZoneId
    JOIN dbo.Warehouses w ON w.Id = z.WarehouseId
),
Stock AS (
    SELECT
        p.Id AS ProductId,
        p.Sku,
        p.Description,
        wb.WarehouseCode,
        SUM(il.Quantity) AS OnHandQty
    FROM dbo.InventoryLocations il
    JOIN dbo.Products p   ON p.Id = il.ProductId
    JOIN dbo.Bins b       ON b.Id = il.BinId
    JOIN dbo.Zones z      ON z.Id = b.ZoneId
    JOIN dbo.Warehouses w ON w.Id = z.WarehouseId
    JOIN WarehouseBins wb ON wb.BinId = b.Id
    GROUP BY p.Id, p.Sku, p.Description, wb.WarehouseCode
),
LastMove AS (
    SELECT
        ar.ProductId,
        -- prefer the TO bin's warehouse; fall back to FROM bin's
        ISNULL(wb_to.WarehouseCode, wb_from.WarehouseCode) AS WarehouseCode,
        MAX(ar.[Timestamp]) AS LastMovementTs
    FROM dbo.AuditRecords ar
    LEFT JOIN WarehouseBins wb_from ON wb_from.BinId = ar.FromBinId
    LEFT JOIN WarehouseBins wb_to   ON wb_to.BinId   = ar.ToBinId
    WHERE ar.Type IN ('ProductAdd','ProductRemove')  -- adjust if you use different movement types
    GROUP BY ar.ProductId, ISNULL(wb_to.WarehouseCode, wb_from.WarehouseCode)
)
SELECT
    s.WarehouseCode,
    s.Sku,
    s.Description,
    s.OnHandQty AS CurrentQuantity,
    lm.LastMovementTs AS LastMovementDate,
    CASE WHEN lm.LastMovementTs IS NULL THEN NULL
         ELSE DATEDIFF(DAY, lm.LastMovementTs, GETDATE()) END AS DaysSinceLastMovement
FROM Stock s
LEFT JOIN LastMove lm
    ON lm.ProductId = s.ProductId
   AND lm.WarehouseCode = s.WarehouseCode
WHERE s.OnHandQty > 0
  AND (lm.LastMovementTs IS NULL
       OR lm.LastMovementTs <= DATEADD(DAY, -@DaysWithoutMovement, GETDATE()))
ORDER BY 
    CASE WHEN lm.LastMovementTs IS NULL THEN 1 ELSE 0 END,
    lm.LastMovementTs;
```

**Key Points:**

* Uses AuditRecords to track actual product movements
* Identifies products with no movement in 90+ days
* Shows current quantity and days since last movement
* Groups by warehouse for multi-warehouse operations

**Actions for Slow-Moving Inventory:**

1. Review with purchasing team
2. Check for expired or near-expiry dates
3. Consider promotional pricing
4. Bundle with fast-moving items
5. Evaluate for liquidation
6. Return to vendor if possible
7. Adjust future purchasing patterns

#### Dead Stock Management

**Quarterly Review Process:**

1. Identify items with no movement >180 days
2. Check for expired products first
3. Determine disposition:
   * Return to vendor (if possible)
   * Discount deeply
   * Donate for tax write-off
   * Dispose properly (especially expired items)
4. Update purchasing rules
5. Document lessons learned

### Decimal Product Management

#### Inventory Control for Weight-Based Items

**Challenges with Decimal Products:**

* Natural weight loss (evaporation, moisture)
* Measurement variations between scales
* Partial quantity picks
* Unit conversion errors (kg to lbs)
* Minimum sellable quantities

**Best Practices:**

1.  **Establish Variance Tolerances**

    ```
    Product Type          Acceptable Variance
    Dry Goods            ±1%
    Liquids              ±2%
    Fresh Products       ±3%
    Frozen Products      ±2%
    Chemicals            ±0.5%
    ```
2. **Scale Management**
   * Calibrate scales weekly
   * Use same scale type for receiving/picking
   * Document scale ID in transactions
   * Maintain calibration logs
3.  **System Configuration**

    ```
    Product Setup:
    - Enable Decimal Quantities
    - Set Minimum Order Qty (MOQ)
    - Define Unit of Measure (UOM)
    - Configure Conversion Rules
    - Set Rounding Rules (0.01 or 0.1)
    ```
4. **Inventory Reconciliation**
   * Daily weight checks for high-value items
   * Weekly variance reports
   * Monthly physical weight counts
   * Investigate variances >tolerance

### Lot & Expiry Management

#### FEFO Implementation

**Best Practices:**

* Configure expiry alerts:
  * 90 days before expiry - Planning alert
  * 60 days - Sales expedite alert
  * 30 days - Critical action required
  * 0 days - Automatic quarantine
* Allocate oldest first automatically
* Quarantine expired items immediately
* Generate monthly expiry reports
* Set customer-specific expiry allowances

**Expiry Management Workflow:**

```
1. Daily Expiry Check
   ├── System generates expiry alert report
   ├── Review items expiring in next 30 days
   ├── Move to expedite zone if needed
   └── Update sales team on short-dated inventory

2. Weekly Actions
   ├── Physical check of near-expiry items
   ├── Verify FEFO compliance in bins
   ├── Rotate stock if needed
   └── Generate customer notification list

3. Monthly Review
   ├── Full expiry audit
   ├── Disposition expired items
   ├── Update reorder points for slow-movers
   └── Report to management
```

**Customer Expiry Settings:**

```
Setup > Customers > [Customer Name] > Settings
- Default Allowance: 70 days
- Pharmaceutical: 180 days minimum
- Food Service: 90 days minimum  
- Retail: 120 days minimum
- Industrial: May accept short dates
```

#### Lot Tracking Best Practices

**When to Use Lot Tracking:**

* Regulatory requirements (FDA, pharmaceutical)
* Quality control needs
* Recall management
* Warranty tracking
* Batch production items

**Lot Number Standards:**

```
Format Examples:
- Manufacturing: YYYYMMDD-SHIFT-LINE
- Receiving: VENDOR-YYYYMMDD-PO#
- Production: PROD-YYYYMMDD-BATCH
```

***

## 3PL Operations

### Client Onboarding Workflow

#### Week 1: Setup

* [ ] Create client profile
* [ ] Configure billing profiles
* [ ] Assign warehouse space
* [ ] Setup user accounts
* [ ] Import product catalog

#### Week 2: Testing

* [ ] Test receiving process
* [ ] Process sample orders
* [ ] Validate billing calculations
* [ ] Train client on portal
* [ ] Document procedures

#### Week 3: Go-Live

* [ ] Final data import
* [ ] Inventory transfer
* [ ] Enable integrations
* [ ] Monitor first transactions
* [ ] Daily check-ins

### Multi-Client Best Practices

#### Segregation Strategies

**Physical Segregation:**

```
Client A: Aisles A-C
Client B: Aisles D-F
Client C: Aisles G-H
Shared: Packing/shipping
```

**System Segregation:**

* Unique client codes
* Separate billing
* Client-specific reports
* Restricted user access
* Isolated integrations

### 3PL Billing Accuracy

#### Billing Validation Checklist

Daily:

* [ ] Verify all transactions captured
* [ ] Check special handling charges
* [ ] Confirm storage calculations

Weekly:

* [ ] Review billing exceptions
* [ ] Audit sample transactions
* [ ] Update rates if changed

Monthly:

* [ ] Generate draft invoices
* [ ] Client approval process
* [ ] Post final invoices

***

## Performance Metrics

### Key Performance Indicators (KPIs)

#### Receiving Metrics

| Metric                    | Target        | Calculation                    |
| ------------------------- | ------------- | ------------------------------ |
| Receipt to putaway time   | <4 hours      | Time from dock to location     |
| Receiving accuracy        | >99%          | Correct receipts / Total       |
| Cost per receipt          | Baseline -10% | Labor + overhead / Receipts    |
| Dock to stock time        | <24 hours     | Receipt to available           |
| Weight variance (decimal) | <2%           | Actual vs Expected weight      |
| Expiry capture rate       | 100%          | Products with dates / Required |

#### Picking Metrics

| Metric               | Target         | Calculation                |
| -------------------- | -------------- | -------------------------- |
| Pick rate            | >60 picks/hour | Total picks / Hours        |
| Pick accuracy        | >99.5%         | Correct picks / Total      |
| Order cycle time     | <2 hours       | Order release to pack      |
| Lines per hour       | >30            | Lines picked / Hours       |
| Weight pick accuracy | ±2%            | Within tolerance / Total   |
| FEFO compliance      | >99%           | Correct date picks / Total |

#### Shipping Metrics

| Metric            | Target      | Calculation             |
| ----------------- | ----------- | ----------------------- |
| On-time shipment  | >98%        | Shipped on time / Total |
| Shipping accuracy | >99.5%      | Correct ships / Total   |
| Cost per shipment | Competitive | Total cost / Shipments  |
| Dock time         | <30 min     | Arrival to departure    |

#### Inventory Metrics

| Metric               | Target  | Calculation                  |
| -------------------- | ------- | ---------------------------- |
| Inventory accuracy   | >99%    | Accurate counts / Total      |
| Expired product %    | <0.5%   | Expired value / Total value  |
| Shrinkage (weight)   | <2%     | Weight loss / Total weight   |
| Days to expiry       | >90 avg | Average remaining shelf life |
| Cycle count accuracy | >98%    | Correct counts / Total       |

### Dashboard Configuration

**Executive Dashboard:**

* Orders shipped today
* Inventory accuracy
* Open order aging
* Labor efficiency
* Space utilization

**Operational Dashboard:**

* Current pick queue
* Receiving backlog
* Packing stations active
* Cycle count progress
* Returns pending

**3PL Client Dashboard:**

* Inventory levels
* Order status
* Billing preview
* Performance metrics
* Transaction history

***

## Continuous Improvement

### Monthly Review Process

1. **Gather Data**
   * Export KPIs
   * Collect feedback
   * Review errors
   * Analyze trends
2. **Identify Opportunities**
   * Bottom 20% performance
   * Repeated errors
   * Customer complaints
   * Cost overruns
3. **Implement Changes**
   * Test in small area
   * Measure impact
   * Roll out if successful
   * Document new process
4. **Monitor Results**
   * Track for 30 days
   * Adjust if needed
   * Standardize success
   * Share learnings

### Training & Development

**New Employee Training Path:**

Week 1: System basics, safety, warehouse tour Week 2: Receiving and putaway Week 3: Picking and packing Week 4: Shipping and special processes

**Ongoing Training:**

* Monthly safety topics
* New feature training
* Cross-training programs
* Performance coaching
* Best practice sharing

***

{% hint style="success" %}
**Remember**: Best practices are guidelines, not rules. Continuously measure, adjust, and improve based on your specific operation's needs and constraints. What matters most is consistent execution and continuous improvement.
{% endhint %}
