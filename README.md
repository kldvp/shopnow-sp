# ShopNow app

Tables
- Shop products
<details>
<summary><ins>Click here to see table schema</ins></summary>
Extends from: task

**Fields**

| Label               | Internal name       | Type                  | Mandatory | Choice options                                                  |
|---------------------|---------------------|-----------------------|-----------|-----------------------------------------------------------------|
| Title               | title               | String (Full UTF-8)   | Y         |                                                                 |
| Product Description | product_description | HTML                  | Y         |                                                                 |
| Price               | price               | Floating Point Number | Y         |                                                                 |
| Full details        | full_details        | HTML                  |           |                                                                 |
| Category            | category            | Choice                | Y         | books, electronics, grocery, clothes, mediine, funiture, others |
| Available           | available           | True/False            |           |                                                                 |
| Picture             | picture             | Image                 | Y         |                                                                 |


</details>
- Shop cart
<details>
<summary><ins>Click here to see table schema</ins></summary>
Extends from: task

**Fields**

| Label        | Internal name | Type                      | Mandatory |
|--------------|---------------|---------------------------|-----------|
| Product      | product       | Refernce -> Shop products | Y         |
| Total amount | total_amount  | Floating Point Number     | Y         |
| Quantity     | quantity      | Integer                   | Y         |
| Added by     | cart_user     | Reference -> User         | Y         |


</details>
- Shop orders
<details>
<summary><ins>Click here to see table schema</ins></summary>
Extends from: task

**Fields**

| Label        | Internal name | Type                      | Mandatory | Choice options                                       |
|--------------|---------------|---------------------------|-----------|------------------------------------------------------|
| Product      | product       | Refernce -> Shop products | Y         |                                                      |
| Total amount | total_amount  | Floating Point Number     | Y         |                                                      |
| Quantity     | quantity      | Integer                   | Y         |                                                      |
| Ordered by   | ordered_by    | Reference -> User         | Y         |                                                      |
| Order Status | order_status  | Choice                    |           | processing, approval, shipping, delivered, cancelled |



</details>

Personas

- Shop user
- Shop admin
- Shop doctor
- Shop fulfiller (optional)

Record producer
- Register Product: Add a new product to shopping catalog


Flows (Flow designer)

- Medical approval

If ordered product category is medicine, it should get approval from *_Shop doctor_* user

- GetAndSetProduct image

If product is added by record producer, attachment will be stored in _sys_attachment_ table, this flow will copy attachment id to _picture_ column


Reports
- Last 7 days orders count

Display a bar chart showing the number of orders placed, completed, canceled, and shipped for the last 7 days.


- Today's order status

Display a donut chart showing the number of orders placed, completed, canceled, and shipped for today.

Portal
- Shop products (/shop)

Portal pages

- shop list (/shop_list) 

  - Display available products for shop users
  - Display all products for shop admin users

- shop cart (/shop_cart_items)
  - Display all cart items added by shop user
- shop orders (/shop_orders)
  - Display all orders placed by shop user

- shop product info (/shop_product_info)
  - Display product information for given sys_id

- shop order place (/shop_order_place)
  - Display order success page

- Shop home (/shop_home)
  - Display all available shop actions for logged in user based on his role
  - Available actions: 
    1. Add product (shop_admin)
    2. Products 
    3. Orders
    4. Dashboard (shop_admin)
    5. Approval requests (shop_doctor)

Widgets

Prefix:ShopNow 
- header menu
- shop actions
- shop icon link
- shop orders
- shop product info
- shop product list
- shop product cart

Menus
- ShopNow

Modules
- Add product
- Cart
- Orders
- Products


Server development
---
Business rules

- setDefaultValues
- ShopCartExplicitAcls
- ShopOrderExplicitAcls

Script includes

- GetCart
- getCartItemsCount
- seedShoppingData


UI actions

- Add product
- Update product


Scheduled script executions

- Process orders

Access control
---

Roles
- x_1550111_shopnow.shop_admin
- x_1550111_shopnow.shop_doctor
- x_1550111_shopnow.shop_doctor

User groups

- Shop Admins
- Shop Users
- Shop Doctors


Access controls

- Shop products
- Shop cart
- Shop orders