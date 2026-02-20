# P4Warehouse REST API

## Overview

P4 Warehouse provides a comprehensive REST API that enables seamless integration with external systems including ERPs, e-commerce platforms, and custom applications. This guide covers authentication, API endpoints, and common integration patterns.

{% hint style="info" %}
**Full Documentation**: For complete P4 Warehouse documentation including user guides, configuration, and additional resources, visit `https://docs.p4.software`
{% endhint %}

## Table of Contents

* [API Authentication](p4warehouse-rest-api.md#api-authentication)
* [API Structure & Patterns](p4warehouse-rest-api.md#api-structure--patterns)
* [Core Resources](p4warehouse-rest-api.md#core-resources)
* [Integration Examples](p4warehouse-rest-api.md#integration-examples)
* [Webhook Configuration](p4warehouse-rest-api.md#webhook-configuration)
* [Error Handling](p4warehouse-rest-api.md#error-handling)
* [Best Practices](p4warehouse-rest-api.md#best-practices)

***

## API Authentication

### Obtaining Your API Key

{% hint style="danger" %}
**SECURITY**: Never expose your API key in client-side code or public repositories. Store credentials securely using environment variables or secure key management systems.
{% endhint %}

1. Navigate to `Setup > System > Users`
2. Click on the **'system'** user from the list
3. Click the dropdown icon to reveal your tenant's API key
4. Copy and securely store the API key

### Authentication Headers

All API requests require the following headers:

```http
Authorization: Bearer {your-api-key}
Content-Type: application/json
Accept: application/json
```

***

## API Structure & Patterns

### Base URL Format

```
https://api.p4warehouse.com/{Resource}Api/{Action}
```

* **Base URL**: Always `https://api.p4warehouse.com` (same for all tenants)
* **{Resource}**: The resource type (Product, Client, Vendor, PickTicket, PurchaseOrder, etc.)
* **{Action}**: The operation (CreateOrUpdate, DeleteBatch, Execute, etc.)

### Standard Endpoints

Each resource typically supports these actions:

| Action         | HTTP Method | Description                               |
| -------------- | ----------- | ----------------------------------------- |
| CreateOrUpdate | POST        | Create new or update existing record      |
| DeleteBatch    | POST        | Delete multiple records                   |
| Get            | GET         | Retrieve single record                    |
| List           | GET         | Retrieve multiple records with pagination |
| Execute        | POST        | Execute specific operations               |

### Example Endpoints

```
https://api.p4warehouse.com/ProductApi/CreateOrUpdate
https://api.p4warehouse.com/PickTicketApi/CreateOrUpdate
https://api.p4warehouse.com/PurchaseOrderApi/DeleteBatch
https://api.p4warehouse.com/FourWallReport/Execute
```

{% hint style="info" %}
**Note**: The tenant identification is handled through the API key in the Authorization header, not through the URL.
{% endhint %}

***

## Core Resources

### 1. Products

#### Create/Update Product

```http
POST https://api.p4warehouse.com/ProductApi/CreateOrUpdate
```

```json
{
  "sku": "WIDGET-001",
  "description": "Blue Widget - Large",
  "barcode": "123456789012",
  "client_id": "CLIENT01",
  "weight": 1.5,
  "length": 10,
  "width": 8,
  "height": 5,
  "unit_of_measure": "EACH",
  "lot_tracking": false,
  "serial_tracking": false,
  "expiry_tracking": false,
  "active": true,
  "pack_sizes": [
    {
      "pack_size": 12,
      "barcode": "123456789013",
      "description": "Case of 12"
    }
  ]
}
```

#### Response

```json
{
  "success": true,
  "product_id": "12345",
  "message": "Product created successfully"
}
```

### 2. Purchase Orders

#### Create Purchase Order

```http
POST https://api.p4warehouse.com/PurchaseOrderApi/CreateOrUpdate
```

```json
{
  "po_number": "PO-2024-001",
  "vendor_id": "VENDOR01",
  "warehouse_code": "WH01",
  "expected_date": "2024-12-31",
  "reference": "Vendor Ref #12345",
  "lines": [
    {
      "line_number": 1,
      "sku": "WIDGET-001",
      "quantity": 100,
      "unit_cost": 5.50
    },
    {
      "line_number": 2,
      "sku": "WIDGET-002",
      "quantity": 50,
      "unit_cost": 7.25
    }
  ]
}
```

### 3. Sales Orders (Pick Tickets)

#### Create Sales Order

```http
POST https://api.p4warehouse.com/PickTicketApi/CreateOrUpdate
```

```json
{
  "order_number": "SO-2024-001",
  "customer_id": "CUST01",
  "warehouse_code": "WH01",
  "order_date": "2024-01-15",
  "required_date": "2024-01-20",
  "ship_to": {
    "name": "John Doe",
    "company": "ABC Company",
    "address1": "123 Main Street",
    "address2": "Suite 100",
    "city": "Anytown",
    "state": "CA",
    "postal_code": "12345",
    "country": "US",
    "phone": "555-123-4567"
  },
  "carrier": "FEDEX",
  "service_level": "GROUND",
  "lines": [
    {
      "line_number": 1,
      "sku": "WIDGET-001",
      "quantity": 5
    },
    {
      "line_number": 2,
      "sku": "WIDGET-002",
      "quantity": 10
    }
  ]
}
```

### 4. Inventory Operations

#### Get Current Inventory (FourWall Report)

```http
GET https://api.p4warehouse.com/FourWallReport/Execute?warehouse_code=WH01
```

#### Response

```json
{
  "success": true,
  "data": [
    {
      "sku": "WIDGET-001",
      "description": "Blue Widget - Large",
      "warehouse_code": "WH01",
      "bin_location": "A-01-01",
      "quantity_on_hand": 150,
      "quantity_allocated": 25,
      "quantity_available": 125,
      "lot_number": "LOT123",
      "expiry_date": "2025-12-31",
      "last_updated": "2024-01-15T10:30:00Z"
    }
  ],
  "total_records": 1
}
```

#### Inventory Adjustment

```http
POST https://api.p4warehouse.com/InventoryApi/Adjust
```

```json
{
  "warehouse_code": "WH01",
  "adjustments": [
    {
      "sku": "WIDGET-001",
      "bin_location": "A-01-01",
      "quantity": 10,
      "reason_code": "CYCLE_COUNT",
      "notes": "Physical count adjustment"
    }
  ]
}
```

### 5. Customers

#### Create/Update Customer

```http
POST https://api.p4warehouse.com/CustomerApi/CreateOrUpdate
```

```json
{
  "customer_number": "CUST01",
  "company_name": "ABC Company",
  "contact_person": "John Doe",
  "email": "john@abccompany.com",
  "phone": "555-123-4567",
  "addresses": [
    {
      "type": "SHIPPING",
      "address1": "123 Main Street",
      "city": "Anytown",
      "state": "CA",
      "postal_code": "12345",
      "country": "US",
      "is_default": true
    }
  ]
}
```

### 6. Vendors

#### Create/Update Vendor

```http
POST https://api.p4warehouse.com/VendorApi/CreateOrUpdate
```

```json
{
  "vendor_number": "VENDOR01",
  "company_name": "XYZ Suppliers",
  "contact_person": "Jane Smith",
  "email": "jane@xyzsuppliers.com",
  "phone": "555-987-6543"
}
```

***

## Integration Examples

### Basic HTTP Request (cURL)

```bash
curl -X POST https://api.p4warehouse.com/ProductApi/CreateOrUpdate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "sku": "TEST-001",
    "description": "Test Product"
  }'
```

### Python Example

```python
import requests
import json

class P4WarehouseClient:
    def __init__(self, api_key):
        self.base_url = "https://api.p4warehouse.com"
        self.headers = {
            "Authorization": f"Bearer {api_key}",
            "Content-Type": "application/json"
        }
    
    def create_product(self, product_data):
        url = f"{self.base_url}/ProductApi/CreateOrUpdate"
        response = requests.post(url, 
                                headers=self.headers, 
                                json=product_data)
        return response.json()
    
    def get_inventory(self, warehouse_code):
        url = f"{self.base_url}/FourWallReport/Execute"
        params = {"warehouse_code": warehouse_code}
        response = requests.get(url, 
                              headers=self.headers, 
                              params=params)
        return response.json()

# Usage
client = P4WarehouseClient("your-api-key")
inventory = client.get_inventory("WH01")
```

### JavaScript/Node.js Example

```javascript
const axios = require('axios');

class P4WarehouseClient {
    constructor(apiKey) {
        this.baseURL = 'https://api.p4warehouse.com';
        this.headers = {
            'Authorization': `Bearer ${apiKey}`,
            'Content-Type': 'application/json'
        };
    }
    
    async createOrder(orderData) {
        try {
            const response = await axios.post(
                `${this.baseURL}/PickTicketApi/CreateOrUpdate`,
                orderData,
                { headers: this.headers }
            );
            return response.data;
        } catch (error) {
            console.error('Error creating order:', error.response.data);
            throw error;
        }
    }
}

// Usage
const client = new P4WarehouseClient('your-api-key');
const order = await client.createOrder({
    order_number: 'SO-2024-001',
    customer_id: 'CUST01',
    // ... other order data
});
```

### C# Example

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json;

public class P4WarehouseClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl = "https://api.p4warehouse.com";
    
    public P4WarehouseClient(string apiKey)
    {
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Add("Authorization", $"Bearer {apiKey}");
        _httpClient.DefaultRequestHeaders.Add("Accept", "application/json");
    }
    
    public async Task<T> PostAsync<T>(string endpoint, object data)
    {
        var json = JsonConvert.SerializeObject(data);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        
        var response = await _httpClient.PostAsync($"{_baseUrl}/{endpoint}", content);
        response.EnsureSuccessStatusCode();
        
        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonConvert.DeserializeObject<T>(responseJson);
    }
}
```

***

## Webhook Configuration

P4 Warehouse can send real-time notifications to your endpoints when specific events occur.

### Available Webhook Events

* **Order Events**: Created, Allocated, Picked, Packed, Shipped
* **Inventory Events**: Low Stock, Stock Received, Cycle Count Complete
* **Purchase Order Events**: Received, Completed
* **3PL Events**: Invoice Generated, Payment Received

### Webhook Payload Example

```json
{
  "event_type": "ORDER_SHIPPED",
  "timestamp": "2024-01-15T14:30:00Z",
  "tenant": "your-tenant-identifier",
  "data": {
    "order_number": "SO-2024-001",
    "tracking_number": "1234567890",
    "carrier": "FEDEX",
    "ship_date": "2024-01-15",
    "lines": [
      {
        "sku": "WIDGET-001",
        "quantity_shipped": 5
      }
    ]
  },
  "signature": "sha256=..."
}
```

### Webhook Security Validation

Always validate webhook signatures to ensure authenticity:

```python
import hmac
import hashlib

def validate_webhook(payload, signature, secret):
    expected = hmac.new(
        secret.encode(),
        payload.encode(),
        hashlib.sha256
    ).hexdigest()
    
    return hmac.compare_digest(
        f"sha256={expected}",
        signature
    )
```

***

## Error Handling

### HTTP Status Codes

| Status Code | Meaning      | Action Required                          |
| ----------- | ------------ | ---------------------------------------- |
| 200         | Success      | None                                     |
| 400         | Bad Request  | Check request format and required fields |
| 401         | Unauthorized | Verify API key                           |
| 403         | Forbidden    | Check permissions for resource           |
| 404         | Not Found    | Verify endpoint URL and resource ID      |
| 429         | Rate Limited | Implement retry with backoff             |
| 500         | Server Error | Contact support if persistent            |

### Error Response Format

```json
{
  "success": false,
  "error": {
    "code": "INVALID_SKU",
    "message": "Product with SKU 'WIDGET-999' not found",
    "details": {
      "field": "sku",
      "value": "WIDGET-999"
    }
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Retry Strategy

Implement exponential backoff for transient errors:

```python
import time
import random

def retry_request(func, max_retries=3):
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            
            # Exponential backoff with jitter
            wait_time = (2 ** attempt) + random.uniform(0, 1)
            time.sleep(wait_time)
```

***

## Best Practices

### 1. Rate Limiting

* Default limit: 100 requests per minute
* Implement request queuing for bulk operations
* Use batch endpoints when available

### 2. Pagination

For large data sets, use pagination parameters:

```http
GET https://api.p4warehouse.com/ProductApi/List?page=1&page_size=100
```

### 3. Filtering

Use query parameters to filter results:

```http
GET https://api.p4warehouse.com/InventoryApi/List?warehouse_code=WH01&sku=WIDGET*
```

### 4. Idempotency

Include idempotency keys for critical operations:

```json
{
  "idempotency_key": "unique-key-12345",
  "order_number": "SO-2024-001",
  // ... rest of order data
}
```

### 5. Data Validation

Always validate data before sending:

* Required fields are present
* Data types are correct
* Values are within acceptable ranges
* SKUs and IDs exist in the system

### 6. Logging

Maintain comprehensive logs:

* All API requests and responses
* Error details and stack traces
* Performance metrics
* Webhook receipts

***

## Testing

### Sandbox Environment

Test your integration using the sandbox:

* Base URL: `https://api.p4warehouse.com` (same as production)
* Use test API keys provided separately for sandbox access
* Data is reset daily

### Test Data

Use these test SKUs in sandbox:

* `TEST-001` through `TEST-100`: Standard products
* `LOT-001`: Lot-tracked product
* `SERIAL-001`: Serial-tracked product
* `EXPIRY-001`: Expiry-tracked product

***

## Support

### Documentation Resources

* **Complete Documentation**: `https://docs.p4.software`
* **Swagger/OpenAPI**: `https://api.p4warehouse.com/swagger/index.html`
* **Support Portal**: Contact your P4 Warehouse partner

### Getting Help

When requesting support, provide:

1. Your tenant identifier
2. API endpoint being called
3. Complete request (minus API key)
4. Response received
5. Timestamp of the request
6. Any error messages

***

## Appendix: Common Integration Scenarios

### E-Commerce Integration

Sync orders from your online store:

1. Receive order webhook from e-commerce platform
2. Create/update customer in P4 Warehouse
3. Create pick ticket with order details
4. Monitor order status via webhooks
5. Update tracking in e-commerce platform when shipped

### ERP Integration

Maintain data synchronization:

1. Product master sync (hourly/daily)
2. Real-time order creation
3. Inventory level updates
4. Purchase order synchronization
5. Financial data export for invoicing

### 3PL Operations

Manage multi-client operations:

1. Client-specific product catalogs
2. Segregated inventory tracking
3. Automated billing generation
4. Client portal access via API
5. Custom reporting per client

***

{% hint style="info" %}
**Note**: This documentation covers the standard P4 Warehouse API. Additional endpoints and features may be available based on your subscription level and enabled modules. For complete documentation, visit `https://docs.p4.software`
{% endhint %}
