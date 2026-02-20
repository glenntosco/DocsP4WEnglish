---
description: Common SQL Queries for P4 Warehouse Database
cover: >-
  https://images.unsplash.com/photo-1752784365239-773786d7848e?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE3NTQ2OTM3MzR8&ixlib=rb-4.1.0&q=85
coverY: 0
---

# Database Query Library

This library contains essential SQL queries for the P4 Warehouse Management System. All queries are read-only (SELECT statements) and optimized for reporting and analysis.

## Table of Contents

1. [Audit Trail Queries](database-query-library.md#audit-trail-queries)
2. [Purchase Order Queries](database-query-library.md#purchase-order-queries)
3. [Pick Ticket Queries](database-query-library.md#pick-ticket-queries)
4. [Inventory Queries](database-query-library.md#inventory-queries)
5. [Customer & Vendor Queries](database-query-library.md#customer-vendor-queries)
6. [Production Order Queries](database-query-library.md#production-order-queries)
7. [Warehouse Analytics](database-query-library.md#warehouse-analytics)
8. [Billing & Invoice Queries](database-query-library.md#billing-invoice-queries)

***

## Audit Trail Queries

### Complete Audit History for Purchase Orders

```sql
-- Get full audit trail for all Purchase Orders with order details
SELECT 
    ar.Timestamp,
    ar.Type as AuditType,
    ar.SubType as AuditSubType,
    ar.Username,
    po.PurchaseOrderNumber,
    po.PurchaseOrderState,
    v.VendorCode,
    v.CompanyName as VendorName,
    w.WarehouseCode,
    ar.Sku,
    ar.Quantity,
    ar.FromBin,
    ar.ToBin,
    ar.FromLpn,
    ar.ToLpn,
    ar.LotNumber,
    ar.ExpiryDate,
    ar.Reason,
    ar.IntegrationMessage
FROM AuditRecords ar
INNER JOIN PurchaseOrders po ON ar.ReferenceId = po.Id
LEFT JOIN Vendors v ON po.VendorId = v.Id
LEFT JOIN Warehouses w ON po.WarehouseId = w.Id
WHERE ar.ReferenceType = 'PurchaseOrder'
ORDER BY ar.Timestamp DESC;
```

### Complete Audit History for Pick Tickets

```sql
-- Get full audit trail for all Pick Tickets with order details
SELECT 
    ar.Timestamp,
    ar.Type as AuditType,
    ar.SubType as AuditSubType,
    ar.Username,
    pt.PickTicketNumber,
    pt.PickTicketState,
    c.CustomerCode,
    c.CompanyName as CustomerName,
    w.WarehouseCode,
    ar.Sku,
    ar.Quantity,
    ar.FromBin,
    ar.ToBin,
    ar.FromLpn,
    ar.ToLpn,
    ar.LotNumber,
    ar.ExpiryDate,
    ar.Reason
FROM AuditRecords ar
INNER JOIN PickTickets pt ON ar.ReferenceId = pt.Id
LEFT JOIN Customers c ON pt.CustomerId = c.Id
LEFT JOIN Warehouses w ON pt.WarehouseId = w.Id
WHERE ar.ReferenceType = 'PickTicket'
ORDER BY ar.Timestamp DESC;
```

### Audit Trail by Date Range

```sql
-- Get audit records for both PO and PT within a date range
SELECT 
    ar.Timestamp,
    ar.ReferenceType,
    ar.ReferenceCode as OrderNumber,
    ar.Type as ActionType,
    ar.Username,
    ar.Sku,
    ar.Quantity,
    ar.FromBin,
    ar.ToBin,
    CASE 
        WHEN ar.ReferenceType = 'PurchaseOrder' THEN po.PurchaseOrderState
        WHEN ar.ReferenceType = 'PickTicket' THEN pt.PickTicketState
    END as CurrentState,
    ar.Reason
FROM AuditRecords ar
LEFT JOIN PurchaseOrders po ON ar.ReferenceId = po.Id AND ar.ReferenceType = 'PurchaseOrder'
LEFT JOIN PickTickets pt ON ar.ReferenceId = pt.Id AND ar.ReferenceType = 'PickTicket'
WHERE ar.ReferenceType IN ('PurchaseOrder', 'PickTicket')
    AND ar.Timestamp >= '2024-01-01' 
    AND ar.Timestamp <= '2024-12-31'
ORDER BY ar.Timestamp DESC;
```

### User Activity Audit Report

```sql
-- Track specific user activities across all operations
SELECT 
    ar.Timestamp,
    ar.Username,
    ar.Type as ActionType,
    ar.SubType,
    ar.ReferenceType,
    ar.ReferenceCode,
    ar.Sku,
    ar.Quantity,
    ar.FromBin,
    ar.ToBin,
    ar.Reason
FROM AuditRecords ar
WHERE ar.Username = 'username_here'
    AND ar.Timestamp >= DATEADD(day, -7, GETDATE())
ORDER BY ar.Timestamp DESC;
```

***

## Purchase Order Queries

### Active Purchase Orders with Line Details

```sql
-- Get all active POs with their line items and product details
SELECT 
    po.PurchaseOrderNumber,
    po.PurchaseOrderState,
    po.RequiredDate,
    v.VendorCode,
    v.CompanyName as VendorName,
    w.WarehouseCode,
    pol.LineNumber,
    p.Sku,
    p.Description as ProductDescription,
    pol.OrderedQuantity,
    pol.ReceivedQuantity,
    (pol.OrderedQuantity - pol.ReceivedQuantity) as RemainingQuantity,
    pol.LotNumber,
    pol.Instructions
FROM PurchaseOrders po
INNER JOIN PurchaseOrderLines pol ON po.Id = pol.PurchaseOrderId
INNER JOIN Products p ON pol.ProductId = p.Id
LEFT JOIN Vendors v ON po.VendorId = v.Id
INNER JOIN Warehouses w ON po.WarehouseId = w.Id
WHERE po.PurchaseOrderState NOT IN ('Closed', 'Cancelled')
ORDER BY po.PurchaseOrderNumber, pol.LineNumber;
```

### Purchase Order Receiving Status

```sql
-- Monitor receiving progress for open POs
SELECT 
    po.PurchaseOrderNumber,
    po.PurchaseOrderState,
    po.AppointmentDate,
    v.CompanyName as VendorName,
    dd.Name as DockDoor,
    u.Username as AssignedTo,
    po.TotalLines,
    po.TotalQuantity,
    po.ReceivingStarted,
    po.ReceivingCompleted,
    SUM(pol.OrderedQuantity) as TotalOrdered,
    SUM(pol.ReceivedQuantity) as TotalReceived,
    CAST(SUM(pol.ReceivedQuantity) * 100.0 / NULLIF(SUM(pol.OrderedQuantity), 0) as DECIMAL(5,2)) as PercentReceived
FROM PurchaseOrders po
LEFT JOIN PurchaseOrderLines pol ON po.Id = pol.PurchaseOrderId
LEFT JOIN Vendors v ON po.VendorId = v.Id
LEFT JOIN DockDoors dd ON po.DockDoorId = dd.Id
LEFT JOIN Users u ON po.AssignedUserId = u.Id
WHERE po.PurchaseOrderState IN ('Released', 'Receiving')
GROUP BY po.Id, po.PurchaseOrderNumber, po.PurchaseOrderState, 
    po.AppointmentDate, v.CompanyName, dd.Name, u.Username,
    po.TotalLines, po.TotalQuantity, po.ReceivingStarted, po.ReceivingCompleted
ORDER BY po.AppointmentDate;
```

### Purchase Order Backorders

```sql
-- Find all POs with backorder relationships
SELECT 
    parent.PurchaseOrderNumber as OriginalPO,
    parent.PurchaseOrderState as OriginalState,
    child.PurchaseOrderNumber as BackorderPO,
    child.PurchaseOrderState as BackorderState,
    child.DateCreated as BackorderCreated,
    v.CompanyName as VendorName
FROM PurchaseOrders child
INNER JOIN PurchaseOrders parent ON child.ParentBackOrderId = parent.Id
LEFT JOIN Vendors v ON parent.VendorId = v.Id
ORDER BY parent.PurchaseOrderNumber, child.DateCreated;
```

***

## Pick Ticket Queries

### Active Pick Tickets with Allocation Status

```sql
-- Monitor pick ticket allocation and fulfillment
SELECT 
    pt.PickTicketNumber,
    pt.PickTicketState,
    pt.RequiredDate,
    c.CustomerCode,
    c.CompanyName as CustomerName,
    w.WarehouseCode,
    pt.TotalLines,
    pt.TotalQuantity,
    pt.PercentageAllocated,
    pt.PercentagePicked,
    u.Username as AssignedPicker,
    pt.ShipToName,
    pt.ShipToCity,
    pt.ShipToStateProvince,
    pt.FreightType
FROM PickTickets pt
LEFT JOIN Customers c ON pt.CustomerId = c.Id
INNER JOIN Warehouses w ON pt.WarehouseId = w.Id
LEFT JOIN Users u ON pt.AssignedUserId = u.Id
WHERE pt.PickTicketState NOT IN ('Closed', 'Cancelled', 'Shipped')
ORDER BY pt.RequiredDate, pt.PickTicketNumber;
```

### Pick Ticket Line Item Details with Inventory Reservations

```sql
-- Get pick ticket lines with their inventory allocations
SELECT 
    pt.PickTicketNumber,
    ptl.LineNumber,
    p.Sku,
    p.Description,
    ptl.OrderedQuantity,
    ptl.AllocatedQuantity,
    ptl.PickedQuantity,
    ptl.ShippedQuantity,
    ir.PickSequence,
    b.BinCode,
    lp.LicensePlateCode,
    ir.Quantity as ReservedQuantity
FROM PickTickets pt
INNER JOIN PickTicketLines ptl ON pt.Id = ptl.PickTicketId
INNER JOIN Products p ON ptl.ProductId = p.Id
LEFT JOIN InventoryReservations ir ON ptl.Id = ir.PickTicketLineId
LEFT JOIN InventoryLocations il ON ir.InventoryLocationId = il.Id
LEFT JOIN Bins b ON il.BinId = b.Id
LEFT JOIN LicensePlates lp ON il.LicensePlateId = lp.Id
WHERE pt.PickTicketState IN ('Released', 'Allocated', 'Picking')
ORDER BY pt.PickTicketNumber, ptl.LineNumber, ir.PickSequence;
```

### Shipping Performance Analysis

```sql
-- Analyze on-time shipping performance
SELECT 
    CAST(pt.RequiredDate as DATE) as RequiredShipDate,
    COUNT(DISTINCT pt.Id) as TotalOrders,
    COUNT(DISTINCT CASE WHEN pt.ShippedDate <= pt.RequiredDate THEN pt.Id END) as OnTimeOrders,
    COUNT(DISTINCT CASE WHEN pt.ShippedDate > pt.RequiredDate THEN pt.Id END) as LateOrders,
    COUNT(DISTINCT CASE WHEN pt.ShippedDate IS NULL AND pt.RequiredDate < GETDATE() THEN pt.Id END) as OverdueOrders,
    CAST(COUNT(DISTINCT CASE WHEN pt.ShippedDate <= pt.RequiredDate THEN pt.Id END) * 100.0 / 
        NULLIF(COUNT(DISTINCT CASE WHEN pt.ShippedDate IS NOT NULL THEN pt.Id END), 0) as DECIMAL(5,2)) as OnTimePercentage
FROM PickTickets pt
WHERE pt.RequiredDate >= DATEADD(month, -3, GETDATE())
    AND pt.PickTicketState NOT IN ('Cancelled')
GROUP BY CAST(pt.RequiredDate as DATE)
ORDER BY RequiredShipDate DESC;
```

### Pick Ticket Cartonization Results

```sql
-- View cartonization results for pick tickets
SELECT 
    pt.PickTicketNumber,
    c.CustomerCode,
    cr.CartonNumber,
    cs.Name as CartonSize,
    cr.Sscc18Code,
    cr.PackedWeight,
    cr.WeightUnitOfMeasure,
    COUNT(crc.Id) as ItemCount,
    SUM(crc.Quantity) as TotalUnits
FROM PickTickets pt
INNER JOIN CartonizationResults cr ON pt.Id = cr.PickTicketId
INNER JOIN CartonizationResultContents crc ON cr.Id = crc.CartonizationResultId
LEFT JOIN CartonSizes cs ON cr.CartonSizeId = cs.Id
LEFT JOIN Customers c ON pt.CustomerId = c.Id
GROUP BY pt.PickTicketNumber, c.CustomerCode, cr.CartonNumber, 
    cs.Name, cr.Sscc18Code, cr.PackedWeight, cr.WeightUnitOfMeasure
ORDER BY pt.PickTicketNumber, cr.CartonNumber;
```

***

## Inventory Queries

### Current Inventory by Location

```sql
-- Get current inventory levels by bin and product
SELECT 
    w.WarehouseCode,
    z.ZoneCode,
    b.BinCode,
    p.Sku,
    p.Description,
    p.Category,
    lp.LicensePlateCode,
    il.Quantity,
    ild.LotNumber,
    ild.SerialNumber,
    ild.ExpiryDate,
    pp.Name as PackSize,
    pp.EachCount
FROM InventoryLocations il
INNER JOIN InventoryLocationDetails ild ON il.Id = ild.InventoryLocationId
INNER JOIN Products p ON il.ProductId = p.Id
LEFT JOIN Bins b ON il.BinId = b.Id
LEFT JOIN Zones z ON b.ZoneId = z.Id
LEFT JOIN Warehouses w ON z.WarehouseId = w.Id
LEFT JOIN LicensePlates lp ON il.LicensePlateId = lp.Id
LEFT JOIN ProductPacksizes pp ON ild.ProductPacksizeId = pp.Id
WHERE il.Quantity > 0
ORDER BY w.WarehouseCode, z.ZoneCode, b.BinCode, p.Sku;
```

### Low Stock Alert Report

```sql
-- Find products below minimum stock levels
SELECT 
    w.WarehouseCode,
    p.Sku,
    p.Description,
    p.Category,
    pwm.MinQuantity,
    pwm.MaxQuantity,
    COALESCE(SUM(il.Quantity), 0) as CurrentStock,
    pwm.MinQuantity - COALESCE(SUM(il.Quantity), 0) as BelowMinBy,
    CASE 
        WHEN COALESCE(SUM(il.Quantity), 0) = 0 THEN 'OUT OF STOCK'
        WHEN COALESCE(SUM(il.Quantity), 0) < pwm.MinQuantity THEN 'LOW STOCK'
        WHEN COALESCE(SUM(il.Quantity), 0) > pwm.MaxQuantity THEN 'OVERSTOCK'
        ELSE 'OK'
    END as StockStatus
FROM Products p
INNER JOIN ProductWarehouseMinMaxes pwm ON p.Id = pwm.ProductId
INNER JOIN Warehouses w ON pwm.WarehouseId = w.Id
LEFT JOIN InventoryLocations il ON p.Id = il.ProductId 
    AND il.BinId IN (SELECT Id FROM Bins WHERE ZoneId IN (SELECT Id FROM Zones WHERE WarehouseId = w.Id))
WHERE p.IsDiscontinued = 0
GROUP BY w.WarehouseCode, p.Sku, p.Description, p.Category, pwm.MinQuantity, pwm.MaxQuantity
HAVING COALESCE(SUM(il.Quantity), 0) < pwm.MinQuantity
ORDER BY w.WarehouseCode, BelowMinBy DESC;
```

### Expiring Inventory Report

```sql
-- Find inventory expiring within specified days
DECLARE @DaysToExpiry INT = 30;

SELECT 
    w.WarehouseCode,
    z.ZoneCode,
    b.BinCode,
    p.Sku,
    p.Description,
    ild.LotNumber,
    ild.ExpiryDate,
    DATEDIFF(day, GETDATE(), ild.ExpiryDate) as DaysUntilExpiry,
    ild.Quantity,
    lp.LicensePlateCode,
    CASE 
        WHEN ild.ExpiryDate < GETDATE() THEN 'EXPIRED'
        WHEN DATEDIFF(day, GETDATE(), ild.ExpiryDate) <= 7 THEN 'CRITICAL'
        WHEN DATEDIFF(day, GETDATE(), ild.ExpiryDate) <= 30 THEN 'WARNING'
        ELSE 'OK'
    END as ExpiryStatus
FROM InventoryLocationDetails ild
INNER JOIN InventoryLocations il ON ild.InventoryLocationId = il.Id
INNER JOIN Products p ON il.ProductId = p.Id
LEFT JOIN Bins b ON il.BinId = b.Id
LEFT JOIN Zones z ON b.ZoneId = z.Id
LEFT JOIN Warehouses w ON z.WarehouseId = w.Id
LEFT JOIN LicensePlates lp ON il.LicensePlateId = lp.Id
WHERE ild.ExpiryDate IS NOT NULL
    AND DATEDIFF(day, GETDATE(), ild.ExpiryDate) <= @DaysToExpiry
    AND ild.Quantity > 0
ORDER BY ild.ExpiryDate, w.WarehouseCode, p.Sku;
```

### Cycle Count Pending Items

```sql
-- Get all pending cycle counts with variance analysis
SELECT 
    pcc.DateCreated as CountRequested,
    w.WarehouseCode,
    z.ZoneCode,
    b.BinCode,
    p.Sku,
    p.Description,
    pcc.PreviousQuantity,
    pcc.NewQuantity,
    pcc.Adjustment,
    ABS(pcc.Adjustment) as AbsoluteVariance,
    CASE 
        WHEN pcc.PreviousQuantity = 0 THEN 100
        ELSE CAST(ABS(pcc.Adjustment) * 100.0 / pcc.PreviousQuantity as DECIMAL(5,2))
    END as VariancePercent,
    u.Username as CountedBy,
    pcc.Reason,
    pcc.CycleCountStyle
FROM PendingCycleCounts pcc
INNER JOIN Products p ON pcc.ProductId = p.Id
LEFT JOIN Bins b ON pcc.BinId = b.Id
LEFT JOIN Zones z ON b.ZoneId = z.Id
LEFT JOIN Warehouses w ON z.WarehouseId = w.Id
LEFT JOIN Users u ON pcc.UserId = u.Id
ORDER BY pcc.DateCreated DESC;
```

***

## Customer & Vendor Queries

### Customer Order History

```sql
-- Get customer order history with statistics
SELECT 
    c.CustomerCode,
    c.CompanyName,
    COUNT(DISTINCT pt.Id) as TotalOrders,
    SUM(pt.TotalQuantity) as TotalUnitsShipped,
    MIN(pt.DateCreated) as FirstOrderDate,
    MAX(pt.ShippedDate) as LastShipDate,
    AVG(DATEDIFF(day, pt.ReleasedToFloorDate, pt.ShippedDate)) as AvgDaysToShip,
    COUNT(DISTINCT CASE WHEN pt.ShippedDate > pt.RequiredDate THEN pt.Id END) as LateShipments
FROM Customers c
LEFT JOIN PickTickets pt ON c.Id = pt.CustomerId
WHERE pt.PickTicketState = 'Closed'
GROUP BY c.CustomerCode, c.CompanyName
ORDER BY TotalOrders DESC;
```

### Vendor Performance Metrics

```sql
-- Analyze vendor delivery performance
SELECT 
    v.VendorCode,
    v.CompanyName,
    COUNT(DISTINCT po.Id) as TotalPOs,
    SUM(po.TotalQuantity) as TotalUnitsOrdered,
    COUNT(DISTINCT CASE WHEN po.ReceivingCompleted <= po.RequiredDate THEN po.Id END) as OnTimePOs,
    COUNT(DISTINCT CASE WHEN po.ReceivingCompleted > po.RequiredDate THEN po.Id END) as LatePOs,
    AVG(DATEDIFF(day, po.ReleasedDate, po.ReceivingCompleted)) as AvgDaysToReceive,
    CAST(COUNT(DISTINCT CASE WHEN po.ReceivingCompleted <= po.RequiredDate THEN po.Id END) * 100.0 / 
        NULLIF(COUNT(DISTINCT po.Id), 0) as DECIMAL(5,2)) as OnTimePercentage
FROM Vendors v
LEFT JOIN PurchaseOrders po ON v.Id = po.VendorId
WHERE po.PurchaseOrderState = 'Closed'
    AND po.DateCreated >= DATEADD(month, -6, GETDATE())
GROUP BY v.VendorCode, v.CompanyName
ORDER BY TotalPOs DESC;
```

### Customer Returns Analysis

```sql
-- Analyze customer returns by reason and product
SELECT 
    c.CustomerCode,
    c.CompanyName,
    cr.CustomerReturnNumber,
    cr.CustomerReturnState,
    cr.TrackingNumber,
    p.Sku,
    p.Description,
    crl.Quantity,
    crl.ReceivedQuantity,
    crl.DamagedQuantity,
    cr.Comments,
    cr.DateCreated,
    cr.ReceivingCompleted
FROM CustomerReturns cr
INNER JOIN CustomerReturnLines crl ON cr.Id = crl.CustomerReturnId
INNER JOIN Products p ON crl.ProductId = p.Id
INNER JOIN Customers c ON cr.CustomerId = c.Id
WHERE cr.DateCreated >= DATEADD(month, -3, GETDATE())
ORDER BY cr.DateCreated DESC;
```

***

## Production Order Queries

### Active Production Orders

```sql
-- Monitor active production orders and their status
SELECT 
    po.ProductionOrderNumber,
    po.ProductionOrderState,
    po.ProductionStep,
    wf.Name as WorkflowName,
    inprod.Sku as InputProduct,
    po.InQuantity as InputQuantity,
    outprod.Sku as OutputProduct,
    po.OutQuantity as OutputQuantity,
    b.BinCode as WorkAreaBin,
    u.Username as AssignedTo,
    po.ProductionStarted,
    po.ProductionCompleted,
    po.AllocationSettings
FROM ProductionOrders po
LEFT JOIN Workflows wf ON po.WorkflowId = wf.Id
LEFT JOIN Products inprod ON po.InProductId = inprod.Id
LEFT JOIN Products outprod ON po.OutProductId = outprod.Id
LEFT JOIN Bins b ON po.WorkAreaBinId = b.Id
LEFT JOIN Users u ON po.AssignedUserId = u.Id
WHERE po.ProductionOrderState NOT IN ('Closed', 'Cancelled')
ORDER BY po.DateCreated DESC;
```

### Bill of Materials Usage Report

```sql
-- Track BOM component consumption
SELECT 
    po.ProductionOrderNumber,
    pol.LineNumber,
    p.Sku as ComponentSku,
    p.Description as ComponentDescription,
    pol.Quantity as RequiredQuantity,
    pol.AllocatedQuantity,
    pol.ConsumedQuantity,
    (pol.Quantity - pol.ConsumedQuantity) as RemainingQuantity
FROM ProductionOrders po
INNER JOIN ProductionOrderOutLines pol ON po.Id = pol.ProductionOrderId
INNER JOIN Products p ON pol.ProductId = p.Id
WHERE po.ProductionOrderState IN ('Released', 'Production')
ORDER BY po.ProductionOrderNumber, pol.LineNumber;
```

***

## Warehouse Analytics

### Daily Throughput Analysis

```sql
-- Analyze daily warehouse throughput
SELECT 
    CAST(ActivityDate as DATE) as Date,
    WarehouseCode,
    SUM(ReceivedUnits) as UnitsReceived,
    SUM(ShippedUnits) as UnitsShipped,
    SUM(PickedUnits) as UnitsPicked,
    SUM(MovedUnits) as UnitsMoved,
    COUNT(DISTINCT ReceivingPOs) as POsReceived,
    COUNT(DISTINCT ShippingPTs) as OrdersShipped
FROM (
    -- Receiving
    SELECT 
        po.ReceivingCompleted as ActivityDate,
        w.WarehouseCode,
        SUM(pol.ReceivedQuantity) as ReceivedUnits,
        0 as ShippedUnits,
        0 as PickedUnits,
        0 as MovedUnits,
        po.Id as ReceivingPOs,
        NULL as ShippingPTs
    FROM PurchaseOrders po
    INNER JOIN PurchaseOrderLines pol ON po.Id = pol.PurchaseOrderId
    INNER JOIN Warehouses w ON po.WarehouseId = w.Id
    WHERE po.ReceivingCompleted IS NOT NULL
        AND po.ReceivingCompleted >= DATEADD(day, -30, GETDATE())
    GROUP BY po.ReceivingCompleted, w.WarehouseCode, po.Id
    
    UNION ALL
    
    -- Shipping
    SELECT 
        pt.ShippedDate as ActivityDate,
        w.WarehouseCode,
        0 as ReceivedUnits,
        SUM(ptl.ShippedQuantity) as ShippedUnits,
        0 as PickedUnits,
        0 as MovedUnits,
        NULL as ReceivingPOs,
        pt.Id as ShippingPTs
    FROM PickTickets pt
    INNER JOIN PickTicketLines ptl ON pt.Id = ptl.PickTicketId
    INNER JOIN Warehouses w ON pt.WarehouseId = w.Id
    WHERE pt.ShippedDate IS NOT NULL
        AND pt.ShippedDate >= DATEADD(day, -30, GETDATE())
    GROUP BY pt.ShippedDate, w.WarehouseCode, pt.Id
) as DailyActivity
GROUP BY CAST(ActivityDate as DATE), WarehouseCode
ORDER BY Date DESC, WarehouseCode;
```

### Zone Utilization Report

```sql
-- Analyze warehouse zone utilization
SELECT 
    w.WarehouseCode,
    z.ZoneCode,
    z.Description as ZoneDescription,
    COUNT(DISTINCT b.Id) as TotalBins,
    COUNT(DISTINCT il.BinId) as OccupiedBins,
    CAST(COUNT(DISTINCT il.BinId) * 100.0 / NULLIF(COUNT(DISTINCT b.Id), 0) as DECIMAL(5,2)) as UtilizationPercent,
    COUNT(DISTINCT il.ProductId) as UniqueProducts,
    SUM(il.Quantity) as TotalUnits,
    z.ProductHandling,
    z.IsQuarantine,
    z.IsProductionArea
FROM Warehouses w
INNER JOIN Zones z ON w.Id = z.WarehouseId
INNER JOIN Bins b ON z.Id = b.ZoneId
LEFT JOIN InventoryLocations il ON b.Id = il.BinId AND il.Quantity > 0
GROUP BY w.WarehouseCode, z.ZoneCode, z.Description, 
    z.ProductHandling, z.IsQuarantine, z.IsProductionArea
ORDER BY w.WarehouseCode, z.ZoneCode;
```

### User Productivity Report

```sql
-- Track user productivity metrics
SELECT 
    u.Username,
    u.FirstName,
    u.LastName,
    u.GroupName,
    COUNT(DISTINCT CASE WHEN ar.Type = 'Pick' THEN ar.Id END) as PickTransactions,
    SUM(CASE WHEN ar.Type = 'Pick' THEN ar.Quantity ELSE 0 END) as UnitsPicked,
    COUNT(DISTINCT CASE WHEN ar.Type = 'Receive' THEN ar.Id END) as ReceiveTransactions,
    SUM(CASE WHEN ar.Type = 'Receive' THEN ar.Quantity ELSE 0 END) as UnitsReceived,
    COUNT(DISTINCT CASE WHEN ar.Type = 'Move' THEN ar.Id END) as MoveTransactions,
    COUNT(DISTINCT CASE WHEN ar.Type = 'Count' THEN ar.Id END) as CountTransactions,
    MIN(ar.Timestamp) as FirstActivity,
    MAX(ar.Timestamp) as LastActivity
FROM Users u
LEFT JOIN AuditRecords ar ON u.Id = ar.UserId
WHERE ar.Timestamp >= DATEADD(day, -7, GETDATE())
    AND u.IsActive = 1
GROUP BY u.Username, u.FirstName, u.LastName, u.GroupName
ORDER BY UnitsPicked DESC;
```

***

## Billing & Invoice Queries

### Client Billing Summary

```sql
-- Generate billing summary for 3PL clients
SELECT 
    c.Name as ClientName,
    ci.InvoiceNumber,
    ci.ClientInvoiceState,
    ci.StartPeriod,
    ci.EndPeriod,
    ci.PostingDate,
    ci.SubTotal,
    ci.Total,
    bp.Name as BillingProfile,
    bp.BillingCycle,
    COUNT(DISTINCT bsr.Id) as ServiceTransactions,
    COUNT(DISTINCT bstr.Id) as StorageTransactions
FROM Clients c
INNER JOIN ClientInvoices ci ON c.Id = ci.ClientId
LEFT JOIN BillingProfiles bp ON ci.BillingProfileId = bp.Id
LEFT JOIN BillingServiceRecords bsr ON ci.Id = bsr.ClientInvoiceId
LEFT JOIN BillingStorageRecords bstr ON ci.Id = bstr.ClientInvoiceId
WHERE ci.StartPeriod >= DATEADD(month, -3, GETDATE())
GROUP BY c.Name, ci.InvoiceNumber, ci.ClientInvoiceState, 
    ci.StartPeriod, ci.EndPeriod, ci.PostingDate,
    ci.SubTotal, ci.Total, bp.Name, bp.BillingCycle
ORDER BY ci.StartPeriod DESC, c.Name;
```

### Storage Billing Details

```sql
-- Calculate storage charges by client and product
SELECT 
    c.Name as ClientName,
    bsr.ProductId,
    p.Sku,
    p.Description,
    SUM(bsr.Quantity) as TotalStorageUnits,
    AVG(bsr.UnitCube) as AvgUnitCube,
    bsr.CubeUoM,
    AVG(bsr.UnitWeight) as AvgUnitWeight,
    bsr.WeightUoM,
    COUNT(DISTINCT bsr.ToWarehouse) as WarehouseCount,
    COUNT(DISTINCT bsr.ToZone) as ZoneCount
FROM BillingStorageRecords bsr
INNER JOIN ClientInvoices ci ON bsr.ClientInvoiceId = ci.Id
INNER JOIN Clients c ON ci.ClientId = c.Id
INNER JOIN Products p ON bsr.ProductId = p.Id
WHERE ci.StartPeriod >= DATEADD(month, -1, GETDATE())
GROUP BY c.Name, bsr.ProductId, p.Sku, p.Description, 
    bsr.CubeUoM, bsr.WeightUoM
ORDER BY c.Name, TotalStorageUnits DESC;
```

***

## Advanced Queries

### Cross-Docking Operations

```sql
-- Identify cross-docking opportunities
SELECT 
    po.PurchaseOrderNumber,
    pt.PickTicketNumber,
    p.Sku,
    p.Description,
    pol.OrderedQuantity as InboundQuantity,
    ptl.OrderedQuantity as OutboundQuantity,
    po.RequiredDate as InboundDate,
    pt.RequiredDate as OutboundDate,
    DATEDIFF(day, po.RequiredDate, pt.RequiredDate) as DaysDifference
FROM PurchaseOrderLines pol
INNER JOIN PurchaseOrders po ON pol.PurchaseOrderId = po.Id
INNER JOIN Products p ON pol.ProductId = p.Id
INNER JOIN PickTicketLines ptl ON p.Id = ptl.ProductId
INNER JOIN PickTickets pt ON ptl.PickTicketId = pt.Id
WHERE po.PurchaseOrderState IN ('Released', 'Receiving')
    AND pt.PickTicketState IN ('Released', 'Allocated')
    AND ABS(DATEDIFF(day, po.RequiredDate, pt.RequiredDate)) <= 3
    AND pol.OrderedQuantity >= ptl.OrderedQuantity
ORDER BY p.Sku, po.RequiredDate;
```

### License Plate Movement History

```sql
-- Track license plate movements through the warehouse
SELECT 
    ar.Timestamp,
    ar.Type as MovementType,
    lp.LicensePlateCode,
    lp.Sscc18Code,
    ar.FromBin,
    ar.ToBin,
    ar.Username,
    ar.ProductId,
    p.Sku,
    ar.Quantity,
    ar.Reason
FROM AuditRecords ar
INNER JOIN LicensePlates lp ON ar.FromLpnId = lp.Id OR ar.ToLpnId = lp.Id
LEFT JOIN Products p ON ar.ProductId = p.Id
WHERE lp.LicensePlateCode = 'LP000001' -- Replace with actual LP code
ORDER BY ar.Timestamp DESC;
```

### Warehouse KPI Dashboard

```sql
-- Generate comprehensive KPI metrics
SELECT 
    'Today' as Period,
    (SELECT COUNT(*) FROM PurchaseOrders WHERE CAST(ReceivingCompleted as DATE) = CAST(GETDATE() as DATE)) as POsReceivedToday,
    (SELECT COUNT(*) FROM PickTickets WHERE CAST(ShippedDate as DATE) = CAST(GETDATE() as DATE)) as OrdersShippedToday,
    (SELECT SUM(ReceivedQuantity) FROM PurchaseOrderLines pol 
     INNER JOIN PurchaseOrders po ON pol.PurchaseOrderId = po.Id 
     WHERE CAST(po.ReceivingCompleted as DATE) = CAST(GETDATE() as DATE)) as UnitsReceivedToday,
    (SELECT SUM(ShippedQuantity) FROM PickTicketLines ptl 
     INNER JOIN PickTickets pt ON ptl.PickTicketId = pt.Id 
     WHERE CAST(pt.ShippedDate as DATE) = CAST(GETDATE() as DATE)) as UnitsShippedToday,
    (SELECT COUNT(DISTINCT ProductId) FROM InventoryLocations WHERE Quantity > 0) as ActiveSKUs,
    (SELECT COUNT(*) FROM PickTickets WHERE PickTicketState = 'Released') as OpenOrders,
    (SELECT COUNT(*) FROM PurchaseOrders WHERE PurchaseOrderState = 'Released') as OpenPOs,
    (SELECT COUNT(*) FROM Users WHERE LastLoginDate >= DATEADD(hour, -1, GETDATE())) as ActiveUsers;
```

***

## Query Usage Notes

### Performance Tips

1. Always include appropriate WHERE clauses to limit date ranges
2. Use indexes on commonly filtered columns (dates, states, reference numbers)
3. Consider creating views for frequently used complex queries
4. Monitor query execution plans for optimization opportunities

### Security Considerations

1. All queries are read-only (SELECT statements only)
2. Consider implementing row-level security for multi-tenant environments
3. Audit query usage through the Query Builder interface
4. Restrict access to sensitive billing and client data

### Customization

1. Replace date ranges with parameters for dynamic reporting
2. Add client/warehouse filters for multi-tenant deployments
3. Extend queries with custom Info1-Info10 fields as needed
4. Integrate with reporting tools like SSRS, Power BI, or Crystal Reports

***

## Export Options

All query results can be exported to:

* Excel (.xlsx)
* CSV (.csv)
* PDF (formatted reports)
* JSON (for API integration)

To export results from the Query Builder:

1. Execute your query
2. Review the results
3. Click the "Export" button
4. Select your preferred format
5. Save or share the exported file

***

_Last Updated: January 2025_ _P4 Warehouse Database Schema Version: 2024_
