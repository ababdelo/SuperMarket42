
<h1 align="center">
Supermarket 42 - POS System
</h1>

![SuperMarket42](https://socialify.git.ci/ababdelo/SuperMarket42/image?custom_language=Python&forks=1&issues=1&language=1&name=1&pattern=Circuit+Board&pulls=1&stargazers=1&theme=Dark)

<p align="center">
  <img src="https://img.shields.io/github/last-commit/ababdelo/SuperMarket42?style=flat-square" /> &nbsp;&nbsp;
  <img src="https://img.shields.io/github/commit-activity/m/ababdelo/SuperMarket42?style=flat-square" /> &nbsp;&nbsp;
  <img src="https://img.shields.io/github/followers/ababdelo" /> &nbsp;&nbsp;
  <img src="https://api.visitorbadge.io/api/visitors?path=https%3A%2F%2Fgithub.com%2Fababdelo%2FSuperMarket42&label=Repository%20Visits&countColor=%230c7ebe&style=flat&labelStyle=none"/> &nbsp;&nbsp;
  <img src="https://img.shields.io/github/stars/ababdelo/SuperMarket42" /> &nbsp;&nbsp;
  <img src="https://img.shields.io/github/contributors/ababdelo/SuperMarket42?style=flat-square" />
</p>

A complete desktop graphical application for a supermarket checkout system built with Python and CustomTkinter. It leverages clean architecture separating the data models, the business logic services, and the Graphical User Interface (GUI).

> ⚠️ **Work in Progress**: This project is currently under active development. Features and functionality are subject to change.

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Preview of the application interface and user experience:](#preview-of-the-application-interface-and-user-experience)
  - [Main Interface](#main-interface)
  - [Error Handling \& Edge Cases](#error-handling--edge-cases)
  - [Transaction Completion](#transaction-completion)
- [OOP Concepts Used](#oop-concepts-used)
- [Project File Structure](#project-file-structure)
- [Prerequisites \& How to Run](#prerequisites--how-to-run)
  - [Requirements](#requirements)
  - [Running the App](#running-the-app)
- [Detailed Architecture \& Flow](#detailed-architecture--flow)
  - [1. Application Flow (Start to End)](#1-application-flow-start-to-end)
  - [2. Models (`models/` folder)](#2-models-models-folder)
  - [3. Services (`services/` folder)](#3-services-services-folder)
  - [4. Relationships Between Classes](#4-relationships-between-classes)
- [UML Class Diagram](#uml-class-diagram)

---

## Preview of the application interface and user experience:
The screenshots below demonstrate the main shopping interface, error handling for edge cases, and the final checkout receipt.

### Main Interface
| Standard Cart | Cart with Promo Code Applied |
| :---: | :---: |
| <img src="assets/images/preview/01_cart_no_discount.png" width="100%" alt="Standard Cart"> | <img src="assets/images/preview/02_cart_with_discount.png" width="100%" alt="Discount Applied"> |

### Error Handling & Edge Cases
| Out of Stock | Empty Cart | Insufficient Funds |
| :---: | :---: | :---: |
| <img src="assets/images/preview/04_error_out_of_stock.png" width="100%" alt="Out of stock"> | <img src="assets/images/preview/05_error_empty_cart.png" width="100%" alt="Empty Cart"> | <img src="assets/images/preview/06_error_insufficient_funds.png" width="100%" alt="Insufficient Funds"> |

### Transaction Completion
**Successful Checkout Receipt** <br>
<img src="assets/images/preview/03_checkout_success.png" width="50%" alt="Successful Checkout Receipt">

---

## OOP Concepts Used

This project was built adhering heavily to Object-Oriented Programming (OOP) paradigms to keep the codebase modular, extensible, and clean:

*   **Encapsulation**: Data states like `Product` inventory, `Bill` totals, and `Cart` contents are stored in protected attributes (using the `_` prefix) and accessed exclusively through controlled properties and methods (e.g., `can_buy()` and `decrease_stock()`), preventing invalid external modification.
*   **Inheritance**: Specific product classes (`FoodProduct`, `ElectronicsProduct`, `ClothesProduct`, and `OtherProduct`) branch out from the parent base `Product` class, inheriting its base characteristics like ID, name, price, and stock manipulation methods.
*   **Abstraction**: The main `Product` class is an abstract base class (using the `abc` module). It is never instantiated directly. It defines an abstract method `get_product_family()` that forces all subclasses to provide their own specific logic.
*   **Polymorphism**: The `InventoryManager` and `GUI` iterate through lists of general `Product` instances. When methods like `get_product_family()` or `price` properties are called, the code behaves seamlessly without needing to check the exact type of product it's interacting with.

---

## Project File Structure

```text
supermarket/
│
├── main.py                     # Entry point of the application
├── readme.md                   # Project documentation
│
├── assets/                     # All external persistent files
│   ├── db/                     
│   │   ├── inventory/          
│   │   │   └── products.csv    # Current product stock & data
│   │   ├── orders/             
│   │   │   └── orders.csv      # Logged completed transactions
│   │   ├── promos/             
│   │   │   └── promos.csv      # Valid and used discount codes
│   │   └── users/              
│   │       └── users.csv       # User mock profiles and balances
│   └── images/                 # GUI images (icons, categories, logos)
│
├── gui/                        # View & Controller layer
│   └── app.py                  # CustomTkinter interface class
│
├── models/                     # Data structures
│   ├── bill.py                 # Calculates numeric totals and taxes
│   ├── cart.py                 # Holds active session items
│   └── product.py              # Parent and child product classes
│
└── services/                   # Business logic and File I/O
    ├── file_handler.py         # Static utility for CSV operations
    ├── inventory_manager.py    # Memory holder for available items
    ├── order_manager.py        # Validates and stores receipts
    ├── promo_manager.py        # Parses and validates discount actions
    └── user_manager.py         # Loads profiles and deducts money
```

---

## Prerequisites & How to Run

### Requirements
You need Python 3 installed on your system.
Install the required external packages via `pip`:

```bash
pip install customtkinter pillow
```

### Running the App
Start the system by launching the `main.py` entry point from within the root folder:

```bash
python main.py
```

---

## Detailed Architecture & Flow

### 1. Application Flow (Start to End)
*   **Startup (`main.py`)**: The application entry point. It initializes the `InventoryManager`, loading all products from the `products.csv` database into memory. It then creates the main `SupermarketApp` GUI window, passing the loaded inventory to it, and starts the UI event loop (`mainloop`).
*   **Initialization (`app.py`)**: The GUI configures its layout, loads required image assets, and initializes all the core session components: `Cart` (shopping basket), `Bill` (pricing calculations), `PromoManager` (discounts), `UserManager` (customer profiles), and `OrderManager` (transaction logger). It randomly extracts a user from `users.csv` to heavily simulate an active logged-in customer session (displaying their name and wallet balance in the corner).
*   **Shopping Experience**: 
    *   The main window displays an interactive grid of product cards.
    *   Users can filter listed products by clicking category buttons at the top navigation bar (e.g., "Bakery", "Electronics", "All").
    *   Users click "+ Add to basket" on a product card. This logs the item to the `Cart`, updates the stock integer displayed on the card dynamically, and recalculates the `Bill` subtotals in the sidebar.
*   **Cart & Checkout Management**:
    *   The right side of the screen visualizes the active cart. Users can increment/decrement exact target quantities or clear the cart entirely in one click.
    *   Users can type a discount code into the promo field. If perfectly valid (e.g., matching the `SM42-XXXX-XX` regex) and currently unused in `promos.csv`, an exact discount amount is applied to the active `Bill`.
    *   When the user clicks "Checkout", the system verifies two final states: the cart is not empty, and the dynamic `user_balance` is greater than or equal to the `Bill.total`.
*   **Transaction Completion**:
    *   If successful, the `InventoryManager` permanently deducts the successfully purchased items and writes the new stock limits back to `products.csv`.
    *   The `UserManager` subtracts the `total` cost from the user's electronic balance and updates `users.csv`.
    *   The `OrderManager` strings together the receipt data and appends it to `orders.csv`.
    *   The `PromoManager` updates the consumed promo code as 'used' in `promos.csv`.
    *   The cart and GUI values are safely cleared, popping up an alert reporting successful completion to the user.

### 2. Models (`models/` folder)
Models define the core rigid data structures of the application.

*   **`Product` (Abstract Base Class)**: Represents a single isolated purchase item.
    *   **Attributes**: `_id`, `_name`, `_price`, `_stock`, `_category`, `_image`.
    *   **Methods**:
        *   `can_buy(quantity)`: Evaluates if the requested capacity is strictly available in the database pool.
        *   `decrease_stock(quantity)`: Drops the internal count permanently.
        *   `to_dict()`: Maps object items to a native dictionary format so CSV logic can consume it.
        *   `get_product_family()`: Required stub forcing distinct types.
    *   **Subclasses**: `FoodProduct`, `ElectronicsProduct`, `ClothesProduct`, `OtherProduct`. They inherit from `Product` and return their literal family type via the abstract method string literal (e.g. "Food").
*   **`CartItem`**: Bundles a particular target payload together.
    *   **Attributes**: `product` (reference proxy mapped to a `Product` Object), `quantity` (Current requested purchase size).
    *   **Properties**: `total` (Dynamically yields `price` * `quantity`).
*   **`Cart`**: Encompasses grouped items in processing mode.
    *   **Attributes**: `_items` (A native set mapping mapped distinct product IDs to `CartItems`).
    *   **Methods**: `add()`, `remove()`, `clear()`, `subtotal()`, and `is_empty()`.
*   **`Bill`**: Calculates exact floating numbers over an associated cart list.
    *   **Attributes**: `_cart`, `_tax_rate` (Baseline static 5%), `discount_percentage`, `applied_promo_code`.
    *   **Properties**: Evaluates realtime numbers resolving `subtotal`, `discount_amount`, `taxable_amount`, `tax`, and grand computed `total`.

### 3. Services (`services/` folder)
Services abstract and isolate business application logic interacting with flat files and state states.

*   **`FileHandler`**: A single central static class running direct CSV access procedures.
    *   **Methods**: `read_from_csv(path)`, `write_to_csv(path)`, and `append_row(path)`.
*   **`InventoryManager`**: Holds global tracking of catalog elements.
    *   **Methods**: `load_products()`, `filter_by_category()`, and `save()`.
*   **`UserManager`**: Orchestrates consumer interactions.
    *   **Methods**: `get_random_user()` (Hooks generic entry details), and `update_balance()` (Manages money value mutations).
*   **`PromoManager`**: Regulates string inputs against valid discounts.
    *   **Methods**: `validate_format()` (Regex operations), and `apply_promo()` (Deduces active statuses).
*   **`OrderManager`**: Validates post-transaction history updates to records.
    *   **Methods**: `save_order()`.

### 4. Relationships Between Classes
*   **Controller (`SupermarketApp`)**: Maps visually as a central Controller aggregating `Cart` and `Bill` states and sending data signals outward towards the `InventoryManager`, `UserManager`, `PromoManager`, and `OrderManager` services.
*   **Composition / Aggregation**: `Bill` wraps exactly one active `Cart`. `Cart` owns multiple runtime instances of `CartItem`. `CartItem` wraps exact `Product` locations.
*   **Separation of Concerns**: High-level Domain Controllers like Manager classes NEVER invoke python's intrinsic file descriptors. File handling is completely partitioned and deferred out directly toward the `FileHandler` generic tools.

---

## UML Class Diagram

```mermaid
classDiagram
    %% Base Product and Subclasses
    class Product {
        <<abstract>>
        #_id : str
        #_name : str
        #_price : float
        #_stock : int
        #_category : str
        #_image : str
        +get_product_family()* str
        +can_buy(quantity: int) bool
        +decrease_stock(quantity: int)
        +to_dict() dict
    }
    
    class FoodProduct {
        +get_product_family() str
    }
    class ElectronicsProduct {
        +get_product_family() str
    }
    class ClothesProduct {
        +get_product_family() str
    }
    class OtherProduct {
        +get_product_family() str
    }
    
    Product <|-- FoodProduct
    Product <|-- ElectronicsProduct
    Product <|-- ClothesProduct
    Product <|-- OtherProduct

    %% Cart Models
    class CartItem {
        +product : Product
        +quantity : int
        +total() float
    }

    class Cart {
        -_items : dict
        +add(product: Product, quantity: int)
        +remove(product: Product, quantity: int)
        +clear()
        +subtotal() float
        +is_empty() bool
        +items() list
    }

    class Bill {
        -_cart : Cart
        -_tax_rate : float
        +discount_percentage : float
        +applied_promo_code : str
        +subtotal() float
        +discount_amount() float
        +taxable_amount() float
        +tax() float
        +total() float
        +reset_discount()
    }

    Cart "1" *-- "*" CartItem : contains
    Bill "1" o-- "1" Cart : bills

    %% Services
    class FileHandler {
        <<static>>
        +PRODUCTS_FIELDS : list
        +PROMOS_FIELDS : list
        +USERS_FIELDS : list
        +ORDERS_FIELDS : list
        +read_from_csv(path: str) list
        +write_to_csv(path: str, rows: list, file_type: str)
        +append_row(path: str, row: dict, file_type: str)
    }

    class InventoryManager {
        -_csv_path : str
        -_products : list
        +load_products()
        +get_categories() list
        +filter_by_category(category: str) list
        +find_by_id(product_id: str) Product
        +save()
    }

    class UserManager {
        -_users_file : str
        +get_random_user() dict
        +update_balance(user_id: str, new_balance: float)
    }

    class PromoManager {
        -_promos_file : str
        +PROMO_PATTERN : str
        +validate_format(code: str) bool
        +apply_promo(code: str) float
    }

    class OrderManager {
        -_orders_file : str
        +save_order(user_id: str, cart: Cart, bill: Bill)
    }

    InventoryManager "1" *-- "*" Product : manages
    InventoryManager ..> FileHandler : uses
    UserManager ..> FileHandler : uses
    PromoManager ..> FileHandler : uses
    OrderManager ..> FileHandler : uses

    %% Application / GUI
    class SupermarketApp {
        -inventory : InventoryManager
        -promo_manager : PromoManager
        -user_manager : UserManager
        -order_manager : OrderManager
        -cart : Cart
        -bill : Bill
        -active_user : dict
        +create_product_view()
        +create_cart_view()
        +load_products()
        +filter_products(category: str)
        +add_to_cart()
        +remove_from_cart()
        +clear_cart_action()
        +apply_promo_code()
        +checkout()
    }

    SupermarketApp "1" o-- "1" InventoryManager
    SupermarketApp "1" o-- "1" PromoManager
    SupermarketApp "1" o-- "1" UserManager
    SupermarketApp "1" o-- "1" OrderManager
    SupermarketApp "1" o-- "1" Cart
    SupermarketApp "1" o-- "1" Bill
```

<p align="center">Thanks for stopping by and taking a peek at my work!</p>
