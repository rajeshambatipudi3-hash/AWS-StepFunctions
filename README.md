# 🛒 Order Processing Workflow with AWS Step Functions

This project implements a serverless **order processing workflow** using **AWS Step Functions** and **AWS Lambda**. The workflow involves receiving an order, checking inventory, processing payment, and sending a confirmation email.

---

## 🗂️ Architecture Overview

The workflow is implemented using the following AWS services:

- **Step Functions** – for orchestration
- **Lambda Functions** – for each logical task
- **CloudWatch Logs** – for logging execution details

---

## 🚀 Workflow Steps

1. **ReceiveOrder**  
   Triggers the `ReceiveOrderFunction` Lambda which initiates the order process.

2. **CheckInventory**  
   Calls the `CheckInventoryFunction` to verify item availability.

3. **InventoryConfirmation (Choice)**  
   - If `inventory_status == "available"` → Proceed to payment.
   - Else → End with failure (`InventoryFailed`).

4. **ChargePayment**  
   Calls the `ChargePaymentFunction` to charge the customer.

5. **PaymentConfirmation (Choice)**  
   - If `payment_status == "charged"` → Proceed to send confirmation.
   - Else → End with failure (`PaymentFailed`).

6. **SendConfirmation**  
   Calls the `SendConfirmationEmailFunction` to notify the user.

---

## 🧩 Lambda Functions

Ensure the following Lambda functions are deployed and their ARNs are used in the Step Function:

| Function Name                 | Description                      |
|------------------------------|----------------------------------|
| `ReceiveOrderFunction`       | Accepts the order input          |
| `CheckInventoryFunction`     | Checks if items are available    |
| `ChargePaymentFunction`      | Processes the payment            |
| `SendConfirmationEmailFunction` | Sends confirmation to user  |

---

## 🛠️ Error Handling

- **CheckInventory** has a `Catch` block that redirects to `InventoryFailed` on `States.TaskFailed`.
- **ChargePayment** has a `Catch` block that redirects to `PaymentFailed` on failure.

---

## 📤 Sample Input

```json
{
  "order_id": "12345",
  "product_id": "sku-001",
  "quantity": 1,
  "customer_email": "user@example.com"
}
