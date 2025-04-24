# Kitchen Application Documentation

This document provides a high-level overview of the views within the Kitchen application.

---

## Core Views

### `index(request)`

*   **URL:** `/kitchen/` (Assuming standard mapping)
*   **Login Required:** No
*   **Template:** `kitchen/index.html`
*   **Purpose:** Renders the main kitchen dashboard page, showing navigation cards for different sections.

### `show_menu(request)`

*   **URL:** `/kitchen/menu/` (Assuming standard mapping)
*   **Login Required:** No
*   **Template:** `kitchen/menu_list.html`
*   **Purpose:** Displays a paginated list of all available menu items (`Menu` model).

### `accept_order(request)`

*   **URL:** `/kitchen/orders/new/` (Assuming standard mapping)
*   **Login Required:** Yes (`@login_required`)
*   **Template:** `kitchen/accept_order_multiple.html`
*   **Purpose:** Handles both displaying the form (GET) and processing the submission (POST) for creating a new `Order` with multiple `OrderItem`s using an inline formset. Includes validation, low stock checks (`handle_low_stock_check`), and saving the order atomically. Provides user feedback via messages.

### `manage_my_orders(request)`

*   **URL:** `/kitchen/orders/my/` (Assuming standard mapping)
*   **Login Required:** Yes (`@login_required`)
*   **Template:** `kitchen/manage_my_orders.html`
*   **Purpose:** Displays a paginated list of orders created by the currently logged-in user (excluding completed/cancelled). Allows the user to update the status of their orders via POST requests, which triggers inventory updates when an order is marked 'Completed'.

### `show_charts(request)`

*   **URL:** `/kitchen/charts/` (Assuming standard mapping)
*   **Login Required:** Yes (`@login_required`)
*   **Template:** `kitchen/charts.html`
*   **Purpose:** Renders a dashboard page containing links/cards to different chart views.

### `inventory_charts(request)`

*   **URL:** `/kitchen/charts/inventory/` (Assuming standard mapping)
*   **Login Required:** No (Based on current code, but might need `@login_required`)
*   **Template:** `kitchen/recon_inv_daily.html`
*   **Purpose:** Fetches all `Inventory` data, converts it to a Pandas DataFrame, reshapes the data using `melt`, and generates a side-by-side bar chart comparing `quantity_kitchen` and `quantity_actual` for each item using Seaborn/Matplotlib. The chart image is embedded directly into the HTML response using a Base64 Data URI. Handles cases with no data or errors during chart generation.

---

## Helper Functions

### `handle_low_stock_check(formset)`

*   **Purpose:** Internal helper function called by `accept_order`. Takes a validated `OrderItemFormSet`. Checks if any required raw ingredients for the submitted menu items are below their defined reorder level in the `Inventory`. Returns a list of names of menu items potentially affected by low stock.

---