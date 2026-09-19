# 🧪 Order Validation Bot — Test Cases

This document contains test cases used to verify the AI order validation and inventory management workflow.

---

## 1. Valid Order — Basic T-Shirt

**Message:**

> I want 2 Basic T-Shirts, size M, in black.

**Expected Result:**

* Product: Basic T-Shirt
* Quantity: 2
* Size: M
* Color: Black
* Order should be accepted.
* Total price should be calculated.
* Order should be added to the Orders sheet.
* Product stock should decrease by 2.

---

## 2. Natural Language — Basic T-Shirt

**Message:**

> Can I get 2 basic shirts in black, medium?

**Expected Result:**

The AI should understand that **"basic shirts"** refers to **Basic T-Shirt**.

* Product: Basic T-Shirt
* Quantity: 2
* Size: M
* Color: Black
* Order should be accepted.

---

## 3. Product Name Variation — Basic Tee

**Message:**

> I'd like to buy 3 basic tees, medium, in white.

**Expected Result:**

The AI should normalize **"basic tees"** to **Basic T-Shirt**.

* Quantity: 3
* Size: M
* Color: White
* Order should be accepted if sufficient stock is available.

---

## 4. Oversized Hoodie

**Message:**

> Can I get one hoodie in large and black?

**Expected Result:**

The AI should identify the product as **Oversized Hoodie**.

* Quantity: 1
* Size: L
* Color: Black
* Order should be accepted if the requested options are available.

---

## 5. Running Shoes

**Message:**

> I need 2 pairs of running shoes.

**Expected Result:**

* Product: Running Shoes
* Quantity: 2
* Order should be validated against the product database.

---

## 6. Classic Cap

**Message:**

> I'd like to order one classic cap in black.

**Expected Result:**

* Product: Classic Cap
* Quantity: 1
* Color: Black
* Order should be validated.

---

## 7. Backpack

**Message:**

> I need 2 backpacks.

**Expected Result:**

* Product: Backpack
* Quantity: 2
* Order should be validated.

---

## 8. Product Not Found

**Message:**

> I want 2 Nike Jackets, size M, in black.

**Expected Result:**

The workflow should detect that the product does not exist in the Products sheet.

Expected response:

> Sorry, we couldn't find "Nike Jackets" in our products.

The order should not be added to the Orders sheet.

---

## 9. Quantity Exceeds Stock

**Message:**

> I want 100 Basic T-Shirts, size M, in black.

**Expected Result:**

The workflow should compare the requested quantity with the available stock.

If the available stock is 20, the order should be rejected.

Expected response:

> Sorry, we only have 20 unit(s) of Basic T-Shirt available.

The product stock should remain unchanged.

---

## 10. Exact Maximum Available Stock

**Message:**

> I want 20 Basic T-Shirts, size M, in black.

**Expected Result:**

The order should be accepted because the requested quantity equals the available stock.

After a successful order:

```text
Stock before: 20
Quantity ordered: 20
Stock after: 0
```

---

## 11. Invalid Quantity — Zero

**Message:**

> I want 0 Basic T-Shirts, size M, in black.

**Expected Result:**

The order should be rejected.

Expected response:

> Please provide a valid quantity.

The stock should not change.

---

## 12. Invalid Product

**Message:**

> Do you have smart watches?

**Expected Result:**

The workflow should identify that the requested product is not available in the product database.

The order should be rejected.

---

## 13. Uppercase Input

**Message:**

> I WANT 2 BASIC SHIRTS, SIZE M, BLACK.

**Expected Result:**

The AI should understand the message regardless of capitalization.

* Product: Basic T-Shirt
* Quantity: 2
* Size: M
* Color: Black
* Order should be accepted if stock is available.

---

## 14. Casual Product Description

**Message:**

> I need two black tees in medium.

**Expected Result:**

The AI should understand the casual product description and map it to **Basic T-Shirt**.

The workflow should extract:

```text
Product: Basic T-Shirt
Quantity: 2
Size: M
Color: Black
```

---

## 15. Large Quantity Request

**Message:**

> Can I order 50 backpacks?

**Expected Result:**

The workflow should compare the requested quantity with the current Backpack stock.

If the requested quantity exceeds the available stock, the order should be rejected and the inventory should remain unchanged.

---

## 16. Inventory Update

**Message:**

> I want 3 Basic T-Shirts, size M, in blue.

**Expected Result:**

If the current stock is 20:

```text
Stock before: 20
Quantity ordered: 3
Stock after: 17
```

The order should be recorded in the Orders sheet and the Products sheet should be updated.

---

## 17. Total Price Calculation

**Message:**

> I want 2 Basic T-Shirts, size M, in black.

**Expected Result:**

If the Basic T-Shirt price is $15:

```text
Price: $15
Quantity: 2
Total: $30
```

The total should be calculated automatically by the workflow.

---

## 18. Complete Order Confirmation

**Message:**

> Can I get 2 basic tees in black, medium?

**Expected Result:**

The customer should receive a confirmation message containing:

* Product
* Quantity
* Size
* Color
* Total price

The order should also be recorded and the inventory updated.

---

## ✅ Test Coverage

These test cases cover the main workflow scenarios:

* Natural-language order extraction
* Product name normalization
* Valid orders
* Invalid products
* Invalid quantities
* Stock validation
* Maximum available stock
* Price calculation
* Order recording
* Inventory updates
* Telegram confirmation
* Different customer writing styles
* Uppercase and casual language
