# Training

## Overview

This comprehensive training guide provides role-specific instructions for all P4 Warehouse users. Each section is designed for hands-on training with clear step-by-step procedures.

{% hint style="info" %}
**Training Tip**: Have trainees practice in a sandbox environment before working in production. For complete documentation, visit `https://docs.p4.software`
{% endhint %}

## Table of Contents

* [New Employee Onboarding](training.md#new-employee-onboarding)
* [Receiver Training Guide](training.md#receiver-training-guide)
* [Picker Training Guide](training.md#picker-training-guide)
* [Packer Training Guide](training.md#packer-training-guide)
* [Dispatcher Training Guide](training.md#dispatcher-training-guide)
* [Cycle Counter Training Guide](training.md#cycle-counter-training-guide)
* [Administrator Training Guide](training.md#administrator-training-guide)
* [3PL Manager Training Guide](training.md#3pl-manager-training-guide)
* [Training Certification](training.md#training-certification)

***

## New Employee Onboarding

### Day 1: Orientation

**Morning (4 hours):**

```
Safety & Compliance:
├── Warehouse safety rules
├── Emergency procedures
├── PPE requirements
├── Equipment safety
└── Incident reporting

System Access:
├── Login credentials
├── Password setup
├── Device assignment (PDT)
├── Time clock setup
└── Badge/access card
```

**Afternoon (4 hours):**

```
Warehouse Tour:
├── Layout and zones
├── Product locations
├── Equipment locations
├── Break areas
├── Emergency exits
└── Restricted areas

Basic System Training:
├── Login to PDT
├── Basic navigation
├── Scanning practice
├── Error handling
└── Getting help
```

### Day 2-5: Role-Specific Training

Follow the appropriate role-based guide below.

***

## Receiver Training Guide

### Module 1: Basic Receiving (Day 1)

#### System Login

```
1. Power on PDT device
2. Connect to WiFi (automatic)
3. Open P4 Warehouse app
4. Enter credentials:
   - Username (lowercase)
   - Password
   - Select warehouse
5. Select "Receiving" menu
```

#### Receiving a Purchase Order

**Step-by-Step Process:**

1.  **Pre-Receipt Setup**

    ```
    Menu > Receiving > PO Receipt
    - Scan PO number label
    - Verify vendor name displays
    - Check expected items list
    ```
2.  **Physical Receipt**

    ```
    At dock door:
    - Verify seal number (if applicable)
    - Count total cartons/pallets
    - Check for visible damage
    - Take photos if damaged
    ```
3.  **System Receipt**

    ```
    Scan each item:
    - Scan product barcode
    - Enter quantity received
    - Enter lot number (if required)
    - Enter expiry date (if required)
    - Scan/assign bin location
    ```
4.  **Completion**

    ```
    - Review quantities
    - Confirm receipt
    - Print labels if needed
    - Close PO (if complete)
    ```

#### Common Receiving Errors

| Error                       | Cause               | Solution                       |
| --------------------------- | ------------------- | ------------------------------ |
| "Product not on PO"         | Wrong item scanned  | Verify correct PO              |
| "Quantity exceeds expected" | Over-shipment       | Contact supervisor             |
| "Invalid location"          | Bin doesn't exist   | Create bin or select different |
| "Lot required"              | Product needs lot # | Enter lot from packaging       |

### Module 2: Advanced Receiving (Day 2)

#### LPN (Pallet) Receiving

```
1. Generate LPN number
2. Print pallet label
3. Scan all products to LPN
4. Maintain LPN through putaway
5. Benefits:
   - Faster putaway
   - Maintains pallet integrity
   - Easier cycle counting
```

#### Blind Receiving

```
Process when no PO exists:
1. Menu > Receiving > Non-PO Receipt
2. Select vendor (or create new)
3. Scan each product
4. Enter quantities
5. Generate receipt number
6. Notify purchasing
```

#### Cross-Dock Receiving

```
For immediate ship items:
1. Identify cross-dock orders
2. Receive to CROSS-DOCK zone
3. Apply shipping labels immediately
4. Move to shipping within 4 hours
5. No putaway required
```

### Module 3: Special Product Handling (Day 3)

#### Decimal/Weight Products

```
Receiving by weight:
1. Place on scale
2. Tare container weight
3. Record actual weight
4. System calculates variance
5. Accept if within tolerance (±2%)
6. Flag if outside tolerance
```

#### Expiry-Dated Products

```
Requirements:
1. Always enter expiry date
2. Format: MM/DD/YYYY
3. Check customer allowance:
   - Default: 70 days
   - Reject if too short
4. Label clearly with expiry
5. Arrange FEFO in bin
```

#### Temperature-Controlled Products

```
Cold chain receipt:
1. Check temperature log
2. Move to cooler immediately
3. Maximum 30 min at dock
4. Document temperature
5. Use COOLER/FREEZER zones only
```

***

## Picker Training Guide

### Module 1: Basic Picking (Day 1)

#### Pick Ticket Picking

**Getting Started:**

```
1. Login to PDT
2. Menu > Picking > Pick Ticket
3. Scan pick ticket barcode
4. Review order summary:
   - Total lines
   - Total units
   - Zone sequence
```

**Picking Process:**

```
For each line:
1. Go to location shown
2. Scan bin barcode
3. Scan product barcode
4. Pick quantity shown
5. Confirm pick
6. Place in tote/pallet
```

**Verification Steps:**

```
Always verify:
- Product matches screen
- Quantity is correct
- Expiry date acceptable
- No damage
- Lot number (if shown)
```

### Module 2: Advanced Picking Methods (Day 2)

#### Wave Picking

```
Multiple orders simultaneously:
1. Get wave assignment
2. Use multi-compartment cart
3. Label each compartment
4. Pick all orders in sequence
5. Sort while picking
6. Deliver to packing
```

#### Batch Picking

```
Same items for multiple orders:
1. Get batch assignment
2. Pick total quantity needed
3. Move to packing area
4. Sort into individual orders
5. Best for small items
```

#### Zone Picking

```
Stay in assigned zone:
1. Pick only your zone items
2. Place in tote
3. Pass to next zone
4. Last zone completes order
5. Send to packing
```

### Module 3: Special Picking Situations (Day 3)

#### Short Picks

```
When insufficient inventory:
1. Pick available quantity
2. Select "Short" option
3. Enter actual quantity
4. System creates backorder
5. Note reason if asked
```

#### Decimal Picking

```
For weight-based items:
1. Use scale station
2. Tare container
3. Pick required weight
4. Allow ±2% variance
5. Print weight label
6. Apply to container
```

#### Expiry/Lot Selection

```
System enforces FEFO:
1. Pick oldest first
2. Scan lot label
3. System verifies correct
4. Override needs supervisor
5. Check customer requirements
```

#### Picking Errors Recovery

```
If wrong item picked:
1. Menu > Void Pick
2. Scan item to return
3. Return to correct bin
4. Pick correct item
5. Document reason
```

***

## Packer Training Guide

### Module 1: Packing Basics (Day 1)

#### Packing Station Setup

```
Start of shift:
1. Check supplies:
   - Boxes (various sizes)
   - Void fill
   - Tape
   - Labels
2. Test scale
3. Test printer
4. Login to system
5. Clean work area
```

#### Standard Packing Process

```
1. Scan tote/pick ticket
2. Review order contents
3. Select appropriate box size
4. Pack items securely:
   - Heavy items bottom
   - Fragile protected
   - Void fill gaps
5. Include packing slip
6. Seal box
7. Weigh package
8. Print shipping label
9. Apply label
10. Move to shipping
```

### Module 2: Cartonization (Day 2)

#### Box Selection

```
System Recommendations:
- Small: <1 cubic ft
- Medium: 1-2 cubic ft
- Large: 2-4 cubic ft
- X-Large: >4 cubic ft

Override if needed:
- Fragile items need more space
- Long items need special box
- Multi-box for heavy orders
```

#### Special Packing Requirements

```
Hazmat:
- Use certified packaging
- Apply hazmat labels
- Include documentation
- Segregate from other packages

Fragile:
- Double box method
- Extra cushioning
- Fragile stickers
- Top load labels
```

### Module 3: Shipping Preparation (Day 3)

#### Carrier Selection

```
Determining best carrier:
- Weight: <150 lbs = Parcel
- Weight: >150 lbs = LTL
- Speed: Next day = Express
- Cost: Ground = Economical
```

#### Documentation

```
Required documents:
- Packing slip (in box)
- Shipping label (outside)
- Invoice (if required)
- Customs forms (international)
- COD forms (if applicable)
```

***

## Dispatcher Training Guide

### Module 1: Order Management (Day 1)

#### Creating Orders

```
Manual Order Entry:
1. Fulfillment > Orders > New
2. Select customer
3. Enter ship-to address
4. Add line items:
   - Search/scan SKU
   - Enter quantity
   - Repeat for all items
5. Set required date
6. Save order
```

#### Order Import

```
Bulk Import Process:
1. Prepare CSV file
2. Fulfillment > Import
3. Select file
4. Map columns
5. Validate data
6. Import orders
7. Review errors
8. Correct and retry
```

### Module 2: Allocation & Wave Management (Day 2)

#### Allocation Process

```
1. Select orders to allocate
2. Check inventory availability
3. Run allocation:
   - FIFO: First in, first out
   - FEFO: First expired, first out
   - Closest: Nearest to packing
4. Review allocation results
5. Handle shortages:
   - Backorder
   - Substitute
   - Cancel line
```

#### Wave Creation

```
Best Practices:
1. Group by:
   - Carrier cutoff times
   - Shipping method
   - Zone/area
   - Priority
2. Wave size: 20-30 orders
3. Consider picker capacity
4. Balance workload
5. Release wave
6. Print pick tickets
```

### Module 3: Reporting & Analysis (Day 3)

#### Daily Reports

```
Morning Reports:
- Previous day shipments
- Current day orders
- Inventory levels
- Backorder report

Afternoon Reports:
- Picking productivity
- Shipping performance
- Carrier performance
- Exception report
```

#### KPI Monitoring

```
Key Metrics:
- Order fill rate
- On-time shipment
- Pick accuracy
- Lines per hour
- Cost per order
```

***

## Cycle Counter Training Guide

### Module 1: Cycle Count Basics (Day 1)

#### Getting Assignments

```
1. Login to PDT
2. Menu > Cycle Count
3. Get daily assignments
4. Review locations list
5. Print count sheets (optional)
```

#### Counting Process

```
For each location:
1. Go to bin location
2. Scan bin barcode
3. Count all items in bin
4. Enter each SKU found:
   - Scan product
   - Enter quantity
   - Enter lot (if applicable)
   - Enter expiry (if applicable)
5. Confirm when complete
6. Move to next location
```

### Module 2: Variance Resolution (Day 2)

#### Handling Variances

```
When count differs from system:
1. Recount to verify
2. Check nearby bins
3. Look for recent transactions
4. If variance confirmed:
   - Document reason
   - Take photos if needed
   - Submit for approval
```

#### Common Count Issues

```
- Product in wrong location
- Unrecorded moves
- Picking errors
- Receiving errors
- Damaged product not reported
- Mixed SKUs in bin
```

### Module 3: Special Counts (Day 3)

#### Decimal Product Counts

```
1. Use calibrated scale
2. Tare container
3. Weigh product
4. Record to 0.01 precision
5. Note any moisture/temperature
```

#### High-Value Counts

```
Enhanced procedures:
1. Two-person verification
2. Count twice independently
3. Document serial numbers
4. Photo documentation
5. Immediate variance investigation
```

***

## Administrator Training Guide

### Module 1: System Administration (Week 1)

#### User Management

```
Creating Users:
1. Setup > System > Users > New
2. Enter user information
3. Assign role
4. Set permissions
5. Configure warehouse access
6. Set default printer
7. Generate password
8. Send credentials
```

#### System Configuration

```
Daily Tasks:
- Monitor Print Agent
- Check integration status
- Review error logs
- Clear stuck transactions

Weekly Tasks:
- User access audit
- Performance review
- Backup verification
- Update procedures
```

### Module 2: Troubleshooting (Week 2)

#### Common Issues Resolution

**Print Agent Issues:**

```
1. Check Windows Services
2. Verify P4 Print Agent running
3. Check printer connections
4. Review print queue
5. Clear stuck jobs
6. Restart service if needed
```

**Integration Failures:**

```
1. Check API logs
2. Verify credentials
3. Test connectivity
4. Review error messages
5. Check data format
6. Retry failed transactions
```

**Performance Issues:**

```
1. Check server resources
2. Review slow queries
3. Check network latency
4. Clear cache
5. Restart services
6. Contact support if needed
```

### Module 3: Reporting & Maintenance (Week 3)

#### Custom Reports

```
Creating SQL Reports:
1. Reports > Widgets > New
2. Write SQL query
3. Test with limited data
4. Add parameters
5. Format output
6. Save and share
7. Schedule if needed
```

#### System Maintenance

```
Monthly Tasks:
- Archive old data
- Clean up logs
- Update documentation
- Review configurations
- Test disaster recovery
- Update training materials
```

***

## 3PL Manager Training Guide

### Module 1: Client Onboarding (Week 1)

#### New Client Setup

```
1. Create client profile
2. Configure billing profile
3. Assign warehouse space
4. Setup user accounts
5. Import products
6. Configure integrations
7. Test all processes
8. Train client users
```

#### Billing Configuration

```
Setting Rates:
- Receiving: per pallet/unit
- Storage: per pallet/day
- Picking: per order/line
- Value-added services
- Minimum charges
- Rush fees
```

### Module 2: Client Management (Week 2)

#### Daily Operations

```
Morning Tasks:
- Review client orders
- Check inventory levels
- Monitor SLAs
- Address issues

Afternoon Tasks:
- Update clients
- Review performance
- Plan next day
- Document issues
```

#### Performance Monitoring

```
Client KPIs:
- Order accuracy: >99.5%
- Ship on time: >98%
- Inventory accuracy: >99%
- Damage rate: <0.5%
- Return rate: <2%
```

### Module 3: Billing & Reporting (Week 3)

#### Monthly Billing Process

```
1. Run preliminary bills
2. Review all charges
3. Verify against activity
4. Make adjustments
5. Generate invoices
6. Client approval
7. Send final invoices
8. Track payments
```

#### Client Reporting

```
Standard Reports:
- Inventory summary
- Order activity
- Shipping performance
- Returns summary
- Billing detail
- KPI dashboard
```

***

## Training Certification

### Certification Requirements

#### Level 1: Basic Operator

```
Requirements:
- Complete role training
- Pass written test (80%)
- Demonstrate competency
- 40 hours supervised work
- Zero safety violations

Valid for: 1 year
```

#### Level 2: Advanced Operator

```
Requirements:
- Level 1 certification
- 6 months experience
- Cross-training complete
- Pass advanced test (85%)
- Train new employee

Valid for: 2 years
```

#### Level 3: Lead/Trainer

```
Requirements:
- Level 2 certification
- 1 year experience
- Complete train-the-trainer
- Create training materials
- Lead 5 training sessions

Valid for: 2 years
```

### Competency Testing

#### Written Test Topics

```
- Safety procedures
- System navigation
- Process knowledge
- Error handling
- Quality standards
- Company policies
```

#### Practical Test Elements

```
- Perform key tasks
- Handle exceptions
- Use equipment safely
- Follow procedures
- Meet productivity standards
- Demonstrate accuracy
```

### Training Records

**Documentation Required:**

```
For each trainee:
├── Training dates
├── Modules completed
├── Test scores
├── Practical evaluations
├── Trainer signatures
├── Certification dates
└── Re-certification schedule
```

***

## Training Resources

### Quick Reference Cards

Create laminated cards for:

* Scanner key functions
* Common error codes
* Process checklists
* Emergency contacts
* Safety reminders

### Video Library

Recommended videos:

* System login process
* Receiving procedures
* Picking methods
* Packing standards
* Safety protocols

### Practice Scenarios

Setup training data:

* Test products (TRAIN-001 to TRAIN-100)
* Test customers
* Test orders
* Test locations (TRAIN zone)

### Evaluation Forms

**Training Evaluation:**

```
Rate 1-5:
- Content clarity
- Trainer effectiveness
- Hands-on practice
- Materials quality
- Time allocation
- Readiness to work
```

***

{% hint style="success" %}
**Remember**: Good training is an investment in quality and efficiency. Take time to train properly, and maintain ongoing education as processes evolve.
{% endhint %}
