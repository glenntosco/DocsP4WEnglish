# P4 Warehouse — Allocation Process Flowchart

```mermaid
flowchart TD
    A([📦 Sales Order Created]) --> B[Order Status: OPEN]
    B --> C{Enough stock\navailable?}

    C -- Yes --> D[Allocation Engine Runs]
    C -- No --> E{Short Option\nSetting?}

    E -- ShipShort\ndefault --> D
    E -- HoldShort --> F[Order Status: ON HOLD\nStock reserved, not released]
    F --> G{{Wait for\nrestock}}
    G --> D

    D --> H[Apply Allocation Rules]

    H --> H1[Zones to Include\ne.g. A1, B2, C3]
    H --> H2[Allocation Style\nFIFO / LIFO / BinName / Custom]
    H --> H3[Expiry Allowance\nMin days before expiry\ndefault 45 days]
    H --> H4[Allocate Bulk?\nInclude LPNs in\npickable zones]
    H --> H5[Allocate Eaches?\nBreak packsize items\ninto individual units]

    H1 & H2 & H3 & H4 & H5 --> I[Stock Reserved\nfor Pick Ticket]

    I --> J[Order Status: ALLOCATED]

    J --> K[Supervisor Creates Wave]
    K --> L[Orders Grouped into Wave]
    L --> M[Wave Released to Floor]

    M --> N[Pick Tickets Generated\n& Labels Printed]
    N --> O[Order Status: WAVED / RELEASED]

    O --> P[📱 Picker receives task\non handheld PDT]
    P --> Q[Picking in Progress]

    Q --> R{All items\npicked?}
    R -- Yes, full pick --> S[Order Status: PICKED]
    R -- Partial\nShipShort --> S

    S --> T[Staging & Verification]
    T --> U[Shipping]
    U --> V([✅ Order SHIPPED / CLOSED])

    %% Un-wave path
    O -. Un-Waved by supervisor .-> W[Order returns to\nUNALLOCATED state]
    W -. Stock reservation\nreleased .-> B

    %% Styling
    style A fill:#4CAF50,color:#fff
    style V fill:#4CAF50,color:#fff
    style F fill:#FF9800,color:#fff
    style W fill:#f44336,color:#fff
    style J fill:#2196F3,color:#fff
    style O fill:#2196F3,color:#fff
    style S fill:#2196F3,color:#fff
```

## Configuration Reference

| Setting | Location | Default | Options |
|---------|----------|---------|---------|
| Allocation Style | Setup → System Config → Fulfillment → Allocation | FIFO | FIFO, LIFO, BinName, Custom |
| Short Option | Setup → System Config → Fulfillment → Allocation | ShipShort | ShipShort, HoldShort |
| Zones to Include | Setup → System Config → Fulfillment → Allocation | All zones | Comma-separated zone codes |
| Allocate Bulk | Setup → System Config → Fulfillment → Allocation | No | Yes / No |
| Expiry Allowance | Setup → System Config → Fulfillment → Allocation | 45 days | Any number (overridden at customer level) |
| Allocate Eaches | Setup → System Config → Fulfillment → Allocation | Yes | Yes / No |

> **Note:** If an order is Un-Waved after release, it returns to **Unallocated** state — all stock reservations are released and inventory becomes available to other orders.
