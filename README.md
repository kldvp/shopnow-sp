# ShopNow app

Tables
- Shop products

<details>
<summary><ins>Click here to see table schema</ins></summary>
Extends from: task

**Fields**

| Label               | Internal name       | Type                  | Mandatory | Choice options                                                  | Default value |
|---------------------|---------------------|-----------------------|-----------|-----------------------------------------------------------------|---------------|
| Title               | title               | String (Full UTF-8)   | Y         |                                                                 |               |
| Product Description | product_description | HTML                  | Y         |                                                                 |               |
| Price               | price               | Floating Point Number | Y         |                                                                 |               |
| Full details        | full_details        | HTML                  |           |                                                                 |               |
| Category            | category            | Choice                | Y         | books, electronics, grocery, clothes, mediine, funiture, others |               |
| Available           | available           | True/False            |           |                                                                 | false         |
| Picture             | picture             | Image                 | Y         |                                                                 |               |



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

| Label         | Internal name | Type                      | Mandatory | Choice options                                       | Default value |
|---------------|---------------|---------------------------|-----------|------------------------------------------------------|---------------|
| Product       | product       | Refernce -> Shop products | Y         |                                                      |               |
| Total amount  | total_amount  | Floating Point Number     | Y         |                                                      |               |
| Quantity      | quantity      | Integer                   | Y         |                                                      |               |
| Ordered by    | ordered_by    | Reference -> User         | Y         |                                                      |               |
| Order Status  | order_status  | Choice                    |           | processing, approval, shipping, delivered, cancelled | processing    |
| Cancel Reason | cancel_reason | String                    |           |                                                      |               |


</details>

## Personas

- Admin
- Customer
- Doctor
- Fulfiller (replaced by schedule script)

## Roles


- Shop_admin
- Shop_user
- Shop_doctor

## Access controls


### Shop products

- Shop admin will have read access to all products
- Shop Admins will have create access to create products
- Shop admins will have write access to all products
- Shop admin will have delete access to all products
- Shop user will have read access to only available products


### Shop cart
- Shop user will have access to read their own cart items
- Shop user should have read access to create new cart items
- Shop admin will have access to read all cart items
- Shop user will have create access for cart items
  - Shop Admin (shop_admin role) group users will also get this access as it inherits Shop user role
- Shop users will have write access to update cart items
  - Shop Admin (shop_admin role) group users will also get this access as it inherits Shop user role
  - There are no conditions defined because if user can read items, they can update cart items too.
- Shop users will have delete access to delete cart items
  - Shop Admin (shop_admin role) group users will also get this access as it inherits Shop user role
  - There are no conditions defined because if user can read items, they can delete cart items too.

### Shop orders

- Shop user will have access to read their own orders
- Shop user should have read access to create orders
- Shop admin will have access to read all cart items
- Shop user will have create access for order  items
  - Shop Admin (shop_admin role) group users will also get this access as it inherits Shop user role
- Shop users will have write access to update order items
  - Shop Admin (shop_admin role) group users will also get this access as it inherits Shop user role
  - There are no conditions defined because if user can read items, they can update order items too.
- Shop Admin will have delete access to delete order items
- Doctors will have read access to see medical orders



## Business Rules


- SetDefaultValues
  - Set a default value for the *_full_details_* field if not provided by the user.
- SetCancelReason
  - Set the reject comment in the *_cancel_reason_* field if the approver rejects the order.

## Script includes


- CartUtils (client callable)
- GetCartItemCount (on demand)


## UI Actions


Overrided default functionalities of Submit & Update buttons

- Add product (default override)
- Update product (default override)

## Scheduled scripts


- Process Orders
  - Execute every 2 mins and process orders placed 5 mins ago
  - Replicate actual order processing by changing status from 
    - Processing -> Shipping
    - Shipping -> Delivered
  - Process all orders placed by users
    - books, groceries & medicine orders status will be changed every 5 mins
    - electronics orders status will be changed every 8 mins
    - clothes, furniture & others orders status will be changed every 10 mins



## Applicaton Menus & Modules


- ShopNow (Menu)
- Add product
- Cart
- Orders
- Products

## Notifications

Email notifications
- OrderProcessing
- OrderShipping
- OrderDelivered
- OrderCancel

## Service Portal, Pages & Widgets

### Portal
- ShopNow (/shop)

### Pages
- Shop - Dashboard
- Shop - cart
- Shop - list
- Shop - orders
- Shop - product info
- Shop - order place
- Shop - home

### Widgets

- Shop - ProductInfo
  - Display product information.
  - Add products to the cart.
- Shop - ProductList
  - List all products available for purchase.
  - Admin users can view all products, including out-of-stock items.
  - Basic users can only view available products.
  - Filter orders based on order status: All, Processing, Shipping, Delivered, Cancelled.
  - Allow search of products using server-side search functionality.
- Shop - Actions
  - Display actions based on the user's role.

**Admin View**
  <img width="1721" alt="Screenshot 2024-11-27 at 11 07 53 AM" src="https://github.com/user-attachments/assets/e53e9786-8d65-4d32-8f74-5ecc7334b786">

**User with doctor role**
<img width="1460" alt="Screenshot 2024-11-27 at 11 11 59 AM" src="https://github.com/user-attachments/assets/0bcfb7d9-736d-4d8d-9e00-01a4194e1a3e">

**User view**
<img width="1459" alt="Screenshot 2024-11-27 at 11 13 00 AM" src="https://github.com/user-attachments/assets/8aa226ed-a74d-4330-8a21-10ceb627ba9a">


  - Admin users have access to all actions.
  - Basic users can access Products and Orders.
  - Doctor users can access Products, Orders, and Approvals.
- Shop - HeaderMenu
  - Navigation menu items
- Shop - IconLink
  - Cloned the icon link widget to display actions based on the user’s role.
- Shop - ProductsCart
  - Display all items added to the cart by the user.
  - Edit or remove items from the cart.
  - Place an order.

  **Real time updates example**
  ![real_time_updates](https://github.com/user-attachments/assets/ebcf1f43-a638-46e7-b173-381cd403fd24)
- Shop - ShopOrders
  - Display all orders placed by the user.
  - Filter orders based on order status: All, Processing, Shipping, Delivered, Cancelled.
  - Allow search of orders using client-side search functionality.


## Reporting


- Last 7 days orders count
- Today orders status

## Flows

- MedicalApproval
  - If ordered product category is medicine, it should get approval from Doctor
- GetAndSetProductImage
  - If product is added by record producer, attachment will be stored in *sys_attachment* table, this flow will copy attachment id to picture column

## Record producer

Register product
- Add a new product to shopping catalog
