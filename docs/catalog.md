# ServiceNow Service Catalog Integration

This document provides information about the ServiceNow Service Catalog integration in the ServiceNow MCP server.

## Overview

The ServiceNow Service Catalog integration allows you to:

- List service catalog categories
- List service catalog items
- Get detailed information about specific catalog items, including their variables
- Filter catalog items by category or search query

## Tools

The following tools are available for interacting with the ServiceNow Service Catalog:

### `list_catalog_categories`

Lists available service catalog categories.

**Parameters:**
- `limit` (int, default: 10): Maximum number of categories to return
- `offset` (int, default: 0): Offset for pagination
- `query` (string, optional): Search query for categories
- `active` (boolean, default: true): Whether to only return active categories

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import ListCatalogCategoriesParams, list_catalog_categories

params = ListCatalogCategoriesParams(
    limit=5,
    query="hardware"
)
result = list_catalog_categories(config, auth_manager, params)
```

### `create_catalog_category`

Creates a new service catalog category.

**Parameters:**
- `title` (string, required): Title of the category
- `description` (string, optional): Description of the category
- `parent` (string, optional): Parent category sys_id
- `icon` (string, optional): Icon for the category
- `active` (boolean, default: true): Whether the category is active
- `order` (integer, optional): Order of the category

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import CreateCatalogCategoryParams, create_catalog_category

params = CreateCatalogCategoryParams(
    title="Cloud Services",
    description="Cloud-based services and resources",
    parent="parent_category_id",
    icon="cloud"
)
result = create_catalog_category(config, auth_manager, params)
```

### `update_catalog_category`

Updates an existing service catalog category.

**Parameters:**
- `category_id` (string, required): Category ID or sys_id
- `title` (string, optional): Title of the category
- `description` (string, optional): Description of the category
- `parent` (string, optional): Parent category sys_id
- `icon` (string, optional): Icon for the category
- `active` (boolean, optional): Whether the category is active
- `order` (integer, optional): Order of the category

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import UpdateCatalogCategoryParams, update_catalog_category

params = UpdateCatalogCategoryParams(
    category_id="category123",
    title="IT Equipment",
    description="Updated description for IT equipment"
)
result = update_catalog_category(config, auth_manager, params)
```

### `move_catalog_items`

Moves catalog items to a different category.

**Parameters:**
- `item_ids` (list of strings, required): List of catalog item IDs to move
- `target_category_id` (string, required): Target category ID to move items to

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import MoveCatalogItemsParams, move_catalog_items

params = MoveCatalogItemsParams(
    item_ids=["item1", "item2", "item3"],
    target_category_id="target_category_id"
)
result = move_catalog_items(config, auth_manager, params)
```

### `list_catalog_items`

Lists available service catalog items.

**Parameters:**
- `limit` (int, default: 10): Maximum number of items to return
- `offset` (int, default: 0): Offset for pagination
- `category` (string, optional): Filter by category
- `query` (string, optional): Search query for items
- `active` (boolean, default: true): Whether to only return active items

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import ListCatalogItemsParams, list_catalog_items

params = ListCatalogItemsParams(
    limit=5,
    category="hardware",
    query="laptop"
)
result = list_catalog_items(config, auth_manager, params)
```

### `get_catalog_item`

Gets detailed information about a specific catalog item.

**Parameters:**
- `item_id` (string, required): Catalog item ID or sys_id

**Example:**
```python
from servicenow_mcp.tools.catalog_tools import GetCatalogItemParams, get_catalog_item

params = GetCatalogItemParams(
    item_id="item123"
)
result = get_catalog_item(config, auth_manager, params)
```

## Resources

The following resources are available for accessing the ServiceNow Service Catalog:

### `catalog://items`

Lists service catalog items.

**Example:**
```
catalog://items
```

### `catalog://categories`

Lists service catalog categories.

**Example:**
```
catalog://categories
```

### `catalog://{item_id}`

Gets a specific catalog item by ID.

**Example:**
```
catalog://item123
```

## Integration with Claude Desktop

To use the ServiceNow Service Catalog with Claude Desktop:

1. Configure the ServiceNow MCP server in Claude Desktop
2. Ask Claude questions about the service catalog

**Example prompts:**
- "Can you list the available service catalog categories in ServiceNow?"
- "Can you show me the available items in the ServiceNow service catalog?"
- "Can you list the catalog items in the Hardware category?"
- "Can you show me the details of the 'New Laptop' catalog item?"
- "Can you find catalog items related to 'software' in ServiceNow?"
- "Can you create a new category called 'Cloud Services' in the service catalog?"
- "Can you update the 'Hardware' category to rename it to 'IT Equipment'?"
- "Can you move the 'Virtual Machine' catalog item to the 'Cloud Services' category?"
- "Can you create a subcategory called 'Monitors' under the 'IT Equipment' category?"
- "Can you reorganize our catalog by moving all software items to the 'Software' category?"

## Example Scripts

### Integration Test

The `examples/catalog_integration_test.py` script demonstrates how to use the catalog tools directly:

```bash
python examples/catalog_integration_test.py
```

### Claude Desktop Demo

The `examples/claude_catalog_demo.py` script demonstrates how to use the catalog functionality with Claude Desktop:

```bash
python examples/claude_catalog_demo.py
```

## Data Models

### CatalogItemModel

Represents a ServiceNow catalog item.

**Fields:**
- `sys_id` (string): Unique identifier for the catalog item
- `name` (string): Name of the catalog item
- `short_description` (string, optional): Short description of the catalog item
- `description` (string, optional): Detailed description of the catalog item
- `category` (string, optional): Category of the catalog item
- `price` (string, optional): Price of the catalog item
- `picture` (string, optional): Picture URL of the catalog item
- `active` (boolean, optional): Whether the catalog item is active
- `order` (integer, optional): Order of the catalog item in its category
- `delivery_time` (string, optional): How long it is expected to take to deliver this item (if defined)
- `mandatory_attachment` (boolean, optional): Whether the catalog item requires an attachment prior to submission

### CatalogCategoryModel

Represents a ServiceNow catalog category.

**Fields:**
- `sys_id` (string): Unique identifier for the category
- `title` (string): Title of the category
- `description` (string, optional): Description of the category
- `parent` (string, optional): Parent category ID
- `icon` (string, optional): Icon of the category
- `active` (boolean, optional): Whether the category is active
- `order` (integer, optional): Order of the category

### CatalogItemVariableModel

Represents a ServiceNow catalog item variable or container.

**Fields:**
- `id` (string): Unique identifier for the variable.
- `name` (string): Internal name of the variable.
- `label` (string): Display label for the variable.
- `type` (integer): Numeric variable type code.
- `friendly_type` (string): Readable variable type (e.g., `reference`, `check_box`, `container_start`).
- `display_type` (string, optional): Display type label shown in UI (e.g., `Reference`, `Select Box`).
- `mandatory` (boolean): Whether the variable is mandatory.
- `read_only` (boolean): Whether the variable is read-only.
- `active` (boolean): Whether the variable is active.
- `value` (string | boolean | null): Stored or default value of the variable.
- `displayvalue` (string): Human-readable form of the value.
- `help_text` (string, optional): Help or tooltip text shown to the user.
- `dynamic_value_field` (string, optional): Field used for dynamic default value sourcing.
- `dynamic_value_dot_walk_path` (string, optional): Dot-walk path used in dynamic value references.
- `ref_qualifier` (string, optional): Reference qualifier for filtering reference field results.
- `reference` (string, optional): Reference table name (for `type = reference`).
- `table` (string, optional): Table name used by list collector or reference-type variables.
- `choices` (ChoiceModel[], optional): Available choice options for `select_box` or `choice` type variables.
- `attributes` (string, optional): Comma-separated string of configuration attributes.
- `max_length` (integer): Maximum input length.
- `order` (integer): Display order for the variable within the item.
- `render_label` (boolean): Whether to render the label in UI.
- `conversational_label` (string, optional): Label used in Virtual Agent conversations.
- `pricing_implications` (boolean, optional): Whether the variable affects pricing.
- `layout` (string, optional): Layout configuration (e.g., `normal`).
- `children` (CatalogItemVariableModel[], optional): Nested variables, if this variable is a container.
- `containerType` (string, optional): Container layout type (e.g., `one_to_one`, `checkbox_container`).
- `containerSplitId` (string | null, optional): Split container reference.
- `containerEndId` (string | null, optional): Container end reference.
- `columnCount` (integer, optional): Number of columns used in container layout.
- `hasSplit` (boolean, optional): Whether the container has a split layout.
- `friendly_type` (string): Human-readable description of the variable type (e.g., `select_box`, `reference`).

---

### ChoiceModel

Represents a selectable option within a catalog variable.

**Fields:**
- `label` (string): Display label for the choice.
- `value` (string): Value stored when the choice is selected.
- `index` (integer, optional): Choice order.
- `price` (number, optional): Price associated with the choice.
- `recurring_price` (number, optional): Recurring price for the choice.
- `price_currency` (string, optional): Currency for the one-time price.
- `recurring_price_currency` (string, optional): Currency for recurring price.

---

### ColumnModel (for List Collector / Table field definitions)

Represents a single column definition used within complex list collector variables.

**Fields:**
- `label` (string): Label of the column field.
- `type` (string): Data type of the column (e.g., `string`, `boolean`, `reference`).
- `value` (string): Field name in the referenced table.
- `reference` (string, optional): Reference table name, if applicable.
- `choices` (ChoiceModel[], optional): Available options if the column has enumerated values.
