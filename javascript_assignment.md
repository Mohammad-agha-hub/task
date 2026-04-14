
##  Objective
Build a simple shopping cart system using JavaScript that runs in the console.

##  User System

Create a User constructor:
- username
- password

Function:
- login(username, password)

---

##  Product System

Create Product constructor:
- id
- name
- price

Functions:
- showProducts()
- searchProduct(keyword)

---

## Cart System

Cart item:
- id
- name
- price
- qty

---

##  Functions

### addToCart(id, qty)
- find product
- if exists → add or update quantity

### updateQuantity(id, qty)
- update item quantity

### removeFromCart(id)
- remove using splice()

### showCart()
Format:
Phone x2 = $1000

### calculateTotal()
- return total price

### checkout()
- must login
- cart not empty
- apply 10% discount if total > 1000
- clear cart

---



