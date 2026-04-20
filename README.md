# SAP Integration Suite — Order-to-Cash (O2C) Integration

> **TechNova Solutions Pvt Ltd** | Integration Developer Domain 

---

## 📋 Project Overview

| Field | Details |
|---|---|
| **Title** | Order-to-Cash (O2C) Integration using SAP Integration Suite |
| **Company** | TechNova Solutions Pvt Ltd *(Fictitious)* |
| **Domain** | Integration Developer |
| **Platform** | SAP Integration Suite on SAP BTP Trial |
| **Course** | SAP Integration Suite |

---

## 🏢 Company Background

**TechNova Solutions Pvt Ltd** is a software and technology services company headquartered in Bhubaneswar, Odisha. The company sells enterprise software licenses and annual subscriptions to corporate clients across India, using SAP S/4HANA as its core ERP system.

---

## ❗ Business Problem

Before this integration, TechNova's order process was fully manual:

- Customers placed orders on the web portal
- Sales team manually re-entered each order into SAP S/4HANA
- This caused **24–48 hour delays**, data entry errors, and billing disputes
- No automated customer notification existed after invoice generation
- IT had no central visibility to monitor or trace message flows

---

## ✅ Solution

Automated the complete **Order-to-Cash process** using **SAP Integration Suite**, connecting the customer portal directly to SAP S/4HANA with real-time invoice notification via SAP Event Mesh.

---

## 🔄 Integration Flow — 6 Steps

```
Customer Portal  ──[JSON / HTTPS]──▶  SAP Integration Suite iFlow
                                            │
                                    ┌───────▼────────┐
                                    │ 1. HTTPS Sender │  Receives JSON order
                                    │ 2. JSON→XML     │  Converts format
                                    │ 3. Content Mod  │  Adds metadata headers
                                    │ 4. Msg Mapping  │  Maps 11 fields to XML
                                    │ 5. Groovy Script│  Sets Status=CREATED
                                    │ 6. Request Reply│  Sends to SAP S/4HANA
                                    └───────┬────────┘
                                            │
                              ┌─────────────▼──────────────┐
                              │      SAP S/4HANA            │
                              │  (Sales Order Created)      │
                              └─────────────┬──────────────┘
                                            │  Invoice Event
                              ┌─────────────▼──────────────┐
                              │      SAP Event Mesh         │
                              │  (Real-time notification)   │
                              └─────────────┬──────────────┘
                                            │
                              ┌─────────────▼──────────────┐
                              │    Customer Notified        │
                              └────────────────────────────┘
```

---

## ⚙️ Components Used

| # | Component | Purpose |
|---|---|---|
| 1 | **HTTPS Sender Adapter** | Receives JSON order at `/technova/order` endpoint |
| 2 | **JSON to XML Converter** | Converts incoming JSON body to XML format |
| 3 | **Content Modifier** | Adds `OrderStatus`, `ProcessedBy`, `Timestamp` headers |
| 4 | **Message Mapping** | Maps 11 fields from JSON to SAP SalesOrder XML |
| 5 | **Groovy Script** | Sets `Status = CREATED`, adds processing trace headers |
| 6 | **Request Reply** | Forwards mapped XML to SAP S/4HANA (httpbin mock) |
| 7 | **SAP Event Mesh** | Publishes invoice creation event for real-time notification |

---

## 🗂️ Message Mapping — 11 Fields

| Source (JSON) | Target (XML) | Type |
|---|---|---|
| `orderId` | `SalesOrder/OrderID` | Direct |
| `customerId` | `SalesOrder/CustomerID` | Direct |
| `customerName` | `SalesOrder/CustomerName` | Direct |
| `orderDate` | `SalesOrder/OrderDate` | Direct |
| `items[0]/productId` | `SalesOrder/LineItem/ProductID` | Direct |
| `items[0]/productName` | `SalesOrder/LineItem/Description` | Direct |
| `items[0]/quantity` | `SalesOrder/LineItem/Quantity` | Direct |
| `items[0]/unitPrice` | `SalesOrder/LineItem/UnitPrice` | Direct |
| `totalAmount` | `SalesOrder/TotalAmount` | Direct |
| `currency` | `SalesOrder/Currency` | Direct |
| *(constant)* | `SalesOrder/Status` | Constant → `CREATED` |

---

## 📁 Files in This Repository

| File | Description |
|---|---|
| `input.json` | Incoming order payload from the customer portal |
| `output.xml` | Transformed SAP SalesOrder XML after mapping |
| `groovy_script.txt` | Groovy script — sets Status, adds trace headers |
| `mapping_table.txt` | Full field-to-field mapping documentation |
| `content_modifier.txt` | Content Modifier step configuration details |
| `architecture.png` | 5-component integration architecture diagram |
| `README.md` | This file |

---

## 📥 Sample Input (JSON)

```json
{
  "orderId": "ORD-2024-001",
  "customerId": "CUST-5001",
  "customerName": "Rajesh Kumar",
  "orderDate": "2024-01-15",
  "items": [
    {
      "productId": "PROD-LIC-101",
      "productName": "TechNova ERP License",
      "quantity": 5,
      "unitPrice": 12000.00
    }
  ],
  "totalAmount": 60000.00,
  "currency": "INR",
  "deliveryAddress": "Chennai, Tamil Nadu"
}
```

## 📤 Sample Output (XML)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SalesOrder>
  <OrderID>ORD-2024-001</OrderID>
  <CustomerID>CUST-5001</CustomerID>
  <CustomerName>Rajesh Kumar</CustomerName>
  <OrderDate>2024-01-15</OrderDate>
  <LineItem>
    <ProductID>PROD-LIC-101</ProductID>
    <Description>TechNova ERP License</Description>
    <Quantity>5</Quantity>
    <UnitPrice>12000.00</UnitPrice>
  </LineItem>
  <TotalAmount>60000.00</TotalAmount>
  <Currency>INR</Currency>
  <Status>CREATED</Status>
</SalesOrder>
```


## 📚 References

1. [SAP Help Portal — SAP Integration Suite Documentation](https://help.sap.com/docs/integration-suite)
2. [SAP Discovery Center — SAP Integration Suite Service](https://discovery-center.cloud.sap/serviceCatalog/integration-suite)
3. [SAP Discovery Center — Get Started with SAP Integration Suite (Mission)](https://discovery-center.cloud.sap/missionCatalog/?search=integration-suite)
4. [SAP Learning — Administering SAP Integration Suite](https://learning.sap.com/courses/administering-sap-integration-suite)
5. [SAP Community — Integration Group](https://community.sap.com/t5/integration/gh-p/integration)

---


