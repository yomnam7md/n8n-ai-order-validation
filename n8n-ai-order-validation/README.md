# 🤖 AI Order Validation & Inventory Management

An AI-powered order processing and inventory management workflow built with **n8n, Telegram, Google Sheets, and an LLM**.

This project demonstrates how AI and workflow automation can be combined to understand customer orders written in natural language, validate them against real-time product data, calculate the order total, record successful orders, and automatically update inventory.

---

## 📌 Project Overview

The workflow acts as an automated order assistant through Telegram.

Customers can send orders using natural language without following a strict format.

For example:

> "Can I get 2 basic shirts in black, medium?"

The AI extracts the relevant information and normalizes the product name to match the product database.

The workflow then:

1. Extracts the order information.
2. Identifies the requested product.
3. Checks the product against the Google Sheets inventory.
4. Validates the requested quantity.
5. Calculates the total price.
6. Records valid orders.
7. Sends an order confirmation to the customer.
8. Updates the product stock based on the purchased quantity.

---

## ✨ Features

### 🧠 AI-Powered Order Understanding

The workflow uses an LLM to understand natural language instead of requiring customers to use a fixed format.

For example, the customer can write:

* "I want 2 basic shirts."
* "Can I get two basic tees in black?"
* "I'd like to buy a Basic T-Shirt, size M."
* "Please order 3 oversized hoodies."

The AI extracts the relevant order information and normalizes product names to the names used in the product database.

---

### 📦 Product Validation

Products are validated against a Google Sheets product database.

The workflow checks whether the requested product exists before continuing with the order.

If the product does not exist, the customer receives an appropriate message instead of allowing the order to continue.

Example:

> "I want 2 Nike Jackets."

Response:

> "Sorry, we couldn't find "Nike Jackets" in our products."

---

### 📊 Stock Validation

The workflow checks the requested quantity against the current inventory.

For example, if the database contains:

```text
Basic T-Shirt
Stock: 20
```

and the customer requests:

```text
25 Basic T-Shirts
```

the workflow rejects the order because the requested quantity exceeds the available stock.

---

### 💰 Automatic Price Calculation

The workflow retrieves the product price from Google Sheets and calculates the total automatically.

Example:

```text
Product: Basic T-Shirt
Price: $15
Quantity: 2

Total: $30
```

The customer does not need to calculate the total manually.

---

### 🧾 Order Recording

Once an order passes validation, the order information is appended to an Orders Google Sheet.

The recorded information can include:

* Order ID
* Product ID
* Product Name
* Quantity
* Size
* Color
* Price
* Total Price
* Customer information
* Telegram ID

---

### 📉 Automatic Inventory Update

After a successful order, the workflow updates the product's stock in the Products Google Sheet.

Example:

Before the order:

```text
Basic T-Shirt
Stock: 20
```

Customer orders:

```text
Quantity: 3
```

After the order:

```text
Basic T-Shirt
Stock: 17
```

This keeps the inventory synchronized with successful orders.

---

## 🔄 Workflow Architecture

The main workflow follows this structure:

```text
Telegram Trigger
       ↓
AI Order Information Extraction
       ↓
Get Products from Google Sheets
       ↓
Validate Order
       ↓
IF Order Valid
     ↙        ↘
   TRUE      FALSE
    ↓           ↓
Generate      Send Error
Order Data    Message
    ↓
Append Order
    ↓
Telegram Confirmation
    ↓
Update Product Stock
```

---

## 🧩 Main Workflow Components

### 1. Telegram Trigger

Receives customer messages from Telegram and starts the workflow.

The customer's message is passed to the AI extraction step.

---

### 2. AI Order Information Extraction

The LLM extracts structured information from the customer's natural-language message.

The expected structure is:

```json
{
  "intent": "order",
  "product_name": "Basic T-Shirt",
  "quantity": 2,
  "size": "M",
  "color": "black"
}
```

The AI is also instructed to normalize different product expressions to the exact product names used in the product database.

---

### 3. Google Sheets — Products

The Products sheet acts as the inventory database.

Example structure:

| Product ID | Product Name     | Category    | Price | Stock | Available Sizes | Available Colors |
| ---------- | ---------------- | ----------- | ----: | ----: | --------------- | ---------------- |
| P001       | Basic T-Shirt    | Clothing    |    15 |    20 | S,M,L,XL        | Black,White,Blue |
| P002       | Oversized Hoodie | Clothing    |    30 |    15 | M,L,XL          | Black,White      |
| P003       | Running Shoes    | Footwear    |    50 |    10 | 40,41,42,43     | Black,White      |
| P004       | Classic Cap      | Accessories |    10 |    25 | One Size        | Black,White      |
| P005       | Backpack         | Accessories |    35 |    12 | One Size        | Black,Blue       |

---

### 4. Validate Order

The validation logic checks:

#### Product Availability

Does the requested product exist in the product database?

#### Quantity

Is the requested quantity greater than zero?

#### Stock

Is the requested quantity available in inventory?

If all validation conditions pass, the order is marked as valid.

---

### 5. IF Order Valid

The workflow separates successful and unsuccessful orders.

#### TRUE

The order continues to the order processing steps.

#### FALSE

The workflow sends the validation error back to the customer through Telegram.

---

### 6. Generate Order Data

This step prepares the final structured order information before it is recorded.

The data includes information such as:

```text
Product ID
Product Name
Quantity
Size
Color
Price
Total
```

---

### 7. Append Order

Valid orders are saved to the Orders Google Sheet.

This creates a simple order history that can be used for tracking and reporting.

---

### 8. Telegram Confirmation

After a successful order, the customer receives a confirmation message containing the order details.

Example:

```text
🎉 Order Confirmed!

Product: Basic T-Shirt
Quantity: 2
Size: M
Color: Black

Total: $30

Thank you for your order!
```

---

### 9. Update Product Stock

The workflow identifies the ordered product and decreases its stock by the purchased quantity.

This prevents the inventory sheet from showing outdated stock after successful purchases.

---

## 🧪 Testing

The workflow was tested using different types of customer messages.

### Valid Order

```text
I want 2 basic shirts in black, medium.
```

The AI understands "basic shirts" as:

```text
Basic T-Shirt
```

and the order continues successfully.

---

### Natural Language Variation

```text
Can I get 2 basic tees in black, medium?
```

The workflow can normalize the product description and identify the correct product.

---

### Product Not Found

```text
I want 2 Nike Jackets, size M, black.
```

Expected result:

```text
Sorry, we couldn't find "Nike Jackets" in our products.
```

---

### Insufficient Stock

```text
I want 100 Basic T-Shirts, size M, black.
```

Expected result:

The order is rejected because the requested quantity is greater than the available stock.

---

### Invalid Quantity

```text
I want 0 Basic T-Shirts, size M, black.
```

Expected result:

The order is rejected because the quantity is invalid.

---

## 🛠️ Technologies Used

| Technology           | Purpose                               |
| -------------------- | ------------------------------------- |
| **n8n**              | Workflow automation and orchestration |
| **Telegram Bot API** | Customer communication                |
| **LLM / AI**         | Natural-language order extraction     |
| **Google Sheets**    | Product inventory and order storage   |
| **JavaScript**       | Order validation and calculations     |

---

## 📂 Project Structure

```text
n8n-ai-order-validation/
│
├── README.md
│
├── workflow/
│   └── AI E-commerce Order Automation.json
│
├── test-cases/
│   └── TEST_CASES.md
│
└── screenshots/
    ├── Workflow.png
    ├── Telegram chat1.png
    ├── Telegram chat2.png
    ├── Telegram chat3.png
    ├── Products sheet.png
    └── Orders sheet.png
```

---

## 🔐 Security & Privacy

This repository is intended to demonstrate the workflow architecture and automation logic.

Before sharing the workflow publicly, sensitive credentials and private information should be removed or replaced with placeholders.

Do not commit:

* Telegram bot tokens
* API keys
* Google credentials
* OAuth secrets
* Private customer information
* Personal access tokens

---

## 🎯 Project Goals

This project demonstrates practical applications of AI automation beyond simple chatbot responses.

The main goals were to build a workflow capable of:

* Understanding natural-language requests
* Converting unstructured messages into structured data
* Connecting AI with external data sources
* Validating business rules programmatically
* Automating order processing
* Maintaining inventory automatically
* Providing immediate customer feedback

---

## 🚀 Future Improvements

Possible future improvements include:

* Adding conversational memory for multi-message orders
* Asking customers for missing order information
* Validating size and color against available product options
* Generating unique Order IDs
* Adding customer information collection
* Adding payment integration
* Sending order notifications to an admin
* Adding order cancellation and modification
* Creating sales and inventory dashboards
* Connecting the workflow to a real e-commerce database

---

## 📌 Disclaimer

This project uses a sample product database for demonstration and portfolio purposes. It is not connected to a real store or production inventory system.
