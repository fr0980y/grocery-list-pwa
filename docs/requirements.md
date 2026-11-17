# Grocery List PWA — Product Requirements

**Document status:** Initial draft for review  
**Project:** Grocery List PWA  
**Scope:** Minimum Viable Product (MVP)

## 1. Product Overview

The Grocery List PWA is a mobile-first grocery shopping application designed to make shopping trips easier, faster, and more organized.

Users maintain a reusable catalog of grocery items, associate those items with individual physical store locations, and maintain an independent shopping list for each location.

The application should progressively learn the user's preferred shopping route through normal use. Rather than requiring users to organize everything manually before their first shopping trip, the application should become more useful as users add items, identify their locations within stores, and check items off while shopping.

The application will initially operate entirely on the user's device, without requiring an account, backend, or subscription.

## 2. Product Goals

The primary goals are:

1. Make creating and maintaining grocery lists quick and convenient.
2. Organize shopping lists around the physical layout of each store.
3. Learn the user's preferred shopping route through normal shopping activity.
4. Minimize the amount of information users must enter before they can use the application.
5. Allow the catalog and store information to become more complete over time.
6. Provide a reliable, installable application that works offline.
7. Maintain a clean, understandable data model that supports future enhancements.

## 3. Core Concepts

The application distinguishes between global grocery information, store-specific information, and temporary shopping activity.

### 3.1 Store

A store represents a retailer or store brand, such as Food Lion or Walmart.

A store may have multiple physical branches.

### 3.2 Store Branch

A store branch represents a particular physical location belonging to a store.

Each branch maintains its own:

- In-store layout.
- Grocery item associations.
- Regular grocery item designations.
- Preferred shopping route.
- Active shopping list.

Two branches belonging to the same retailer may have completely different layouts and shopping routes.

### 3.3 Store Area

A store area represents a department, aisle, counter, or other identifiable shopping area within a physical store branch.

Examples include:

- Produce
- Bakery
- Dairy
- Seafood Counter
- Frozen Foods
- Aisle 5
- Aisle 12

Store areas are managed independently for each physical branch. Users can create, rename, remove, and reorder them to reflect the store's physical layout and their preferred shopping route.

### 3.4 Grocery Item

A grocery item is a reusable, global catalog entry.

Examples include Milk, Apples, Bread, and Peanut Butter.

Global item information may include:

- Name
- Category
- Default quantity
- Default unit

Categories are global rather than store-specific.

An item's default quantity and unit provide a starting point when adding it to a shopping list. Changing the quantity on a shopping list must not change the global default.

### 3.5 Store Item Association

A store item association connects a global grocery item to a particular physical store branch.

It contains information specific to that branch, including:

- Assigned store area, if known.
- Whether the item is regularly purchased at that branch.

An item may be associated with multiple branches, and its store-specific information may differ between them.

An item does not have to be assigned to a store area to be used.

### 3.6 Shopping List

Each physical store branch maintains exactly one active shopping list for the MVP.

A shopping list contains references to grocery items and their store associations, along with temporary shopping information.

Shopping-list information includes:

- Grocery item reference.
- Quantity and unit.
- Purchased/unchecked status.
- Position or ordering information.

Shopping-list quantities are independent of global default quantities.

## 4. Shopping List Creation and Management

### 4.1 Create an Empty List

Users can create an empty shopping list for a selected physical store branch.

Items can then be added individually.

### 4.2 Generate a List from Regular Items

Users can generate a shopping list using the items marked as regular for that branch.

Generating a list replaces the existing list completely.

All generated items start unchecked.

The application must clearly communicate that the existing list will be replaced and request confirmation before proceeding.

Removing an item from an active shopping list does not change its regular-item designation.

Adding an occasional item does not automatically make it a regular item.

### 4.3 Independent Lists

Each physical store branch maintains its own persistent active list.

Switching between branches must not affect the other branches' lists.

The application supports one active list per branch in the MVP. Multiple named lists, archived lists, and shopping sessions are outside the initial scope.

## 5. Adding Grocery Items

### 5.1 Item Entry

Users first select a physical store branch and then add grocery items to its list.

Item entry should support:

- Typing a new item name.
- Basic autocomplete against the global grocery catalog.
- Selecting an existing catalog item.
- Creating a new catalog item when no suitable match exists.

A new grocery item requires only a name.

Additional information is optional.

### 5.2 Automatic Store Association

If a user selects an existing global grocery item that has not previously been associated with the selected branch, the application automatically creates the association.

If the item is entirely new, the application creates both its global catalog entry and its association with the selected branch.

Adding an item to a shopping list does not automatically designate it as a regular item.

### 5.3 Optional Information

Users may enter additional information when creating an item or edit it later.

Optional information includes:

- Category.
- Default quantity and unit.
- Store area assignment.
- Regular-item designation.

The application must not require users to complete this information before adding an item to a shopping list.

## 6. Grocery Catalog and Item Editing

### 6.1 Global and Store-Specific Editing

Users must be able to edit an item's global information and its branch-specific information.

The item details screen should present:

**Global Information**
- Item name.
- Category.
- Default quantity and unit.

**Store-Specific Information**
- Assigned store area.
- Regular-item designation.

Global and store-specific information should be visually distinct.

The user should be able to edit either section independently and save changes using one Save action.

### 6.2 Deletion

The MVP uses permanent deletion.

Deleting a store-item association removes the item from that branch's catalog association and active shopping list. The global grocery item and its associations with other branches remain intact.

Deleting a global grocery item removes it from the global catalog, all store associations, and all active shopping lists.

The application should confirm destructive actions.

Preserving deleted items on existing shopping lists or retaining historical records is deferred.

## 7. Store and Store Branch Management

Store and branch management should be available in a dedicated management area, separate from everyday shopping.

Users should be able to:

- Add, edit, and remove stores.
- Add, edit, and remove physical branches.
- View branches belonging to each store.
- Manage each branch's store areas.
- Manage grocery items associated with each branch.

### 7.1 Store Area Management

Users should be able to:

- Create departments, aisles, counters, and other store areas.
- Rename existing areas.
- Remove areas.
- Reorder areas to match their preferred shopping route.
- Assign grocery items to areas.

Store areas must support both numbered aisles and named departments or locations.

The application must not assume every store area is a numbered aisle.

### 7.2 Multiple Branches

Branches belonging to the same retailer must maintain independent layouts and shopping preferences.

Copying a store configuration from one branch to another is a planned near-term enhancement, but is not required for the initial MVP.

A future copy operation should be able to copy reusable configuration, such as store areas, item associations, regular-item designations, and preferred ordering, without copying the active shopping list.

## 8. Shopping Experience

The shopping interface is the application's primary screen and should be designed primarily for portrait-mode phone use.

It should prioritize readability, quick scanning, and easy one-handed interaction.

### 8.1 Shopping Screen

The shopping screen should include:

- Selected store and physical branch near the top.
- A readily accessible way to switch branches.
- A sticky store/branch header where practical.
- A shopping progress indicator.
- Shopping-list items grouped by store area.
- A convenient way to add items.

### 8.2 Shopping List Items

Each item should prominently display its name.

Quantity and unit should be displayed less prominently.

The entire item row should provide a sufficiently large touch target.

Interactions:

- Normal tap: Mark an item purchased or return it to the unpurchased state.
- Long press: Open item details for editing.
- An additional discoverable editing option should be available so long press is not the only way to edit an item.

### 8.3 Purchased Items

Within each store area, unpurchased items appear first.

When an item is checked off, it moves to the bottom of the purchased items within its assigned area.

Purchased items remain visible and are visually distinguished, such as through a checkmark and crossed-out text.

If an item is unchecked, it returns to the unpurchased group.

### 8.4 Collapsible Store Areas

Each store area is displayed as a distinct section with a clear heading.

When all items in an area have been purchased, that section automatically collapses.

Users can reopen collapsed sections to review or change items.

### 8.5 Unassigned Items

Items without a known store area must still appear on the shopping list.

They should be grouped into an Unassigned section rather than being omitted or preventing list creation.

## 9. Progressive Store-Location Enrichment

The application should learn where items are located through normal shopping activity.

When a user checks off an item without an assigned store area, the application should optionally prompt:

**Where did you find this item?**

The user should be able to select an existing store area or create a new one.

Saving this information updates the store-item association for that physical branch.

The item remains checked regardless of whether the user supplies the information or dismisses the prompt.

A reminder or visual indication should make it possible to identify items with missing location information later.

The MVP does not need to remember that a prompt was previously dismissed. If an item is unchecked and subsequently checked again, the prompt may appear again.

This process allows the store catalog to become more complete without requiring extensive setup before the first shopping trip.

## 10. Automatic Shopping Order

A major feature of the application is learning the user's preferred shopping route through the order in which items are checked off.

The application should learn two levels of ordering:

1. **Store area order:** The order in which departments, aisles, and other store areas are typically visited.
2. **Item order:** The order in which items are typically purchased within each store area.

Store area order takes precedence over individual item order.

For example, the learned route might be:

Produce → Bakery → Aisle 5 → Dairy → Frozen Foods.

Within Aisle 5, items might be ordered:

Cereal → Oatmeal → Peanut Butter.

### 10.1 Learning Behavior

The sequence in which items are checked off provides information about the user's shopping route.

The application should use this behavior to establish and improve the preferred order for future shopping lists.

An occasional return to a previously visited area should have limited impact on the overall route because the ordering hierarchy prioritizes store areas over individual items.

The current shopping list may change as items are checked off, but the preferred order for future lists must be maintained separately from temporary purchased-item positioning.

The exact algorithm for combining observations from multiple shopping trips remains an implementation decision.

### 10.2 Initial Ordering

Before sufficient shopping-order information exists, users should be able to use a normal shopping list ordered either:

- In the order items were added.
- Alphabetically.

The application should also support the learned shopping order as information becomes available.

The exact default-mode transition as the application learns a route remains to be finalized.

## 11. Navigation and Application Organization

The application should have three primary areas:

1. **Shopping:** Everyday shopping-list creation, management, and use.
2. **Stores:** Store and physical branch management, including store layouts and store-specific item associations.
3. **Grocery Catalog:** Management of global grocery items and categories.

The navigation should be responsive, with a mobile-friendly layout suitable for phone use.

When the application opens, it should display the shopping list for the last-used physical store branch.

Switching branches should be readily accessible.

## 12. Offline Operation and Local Data

The application must be installable as a Progressive Web App.

The MVP must support:

- Installation on compatible mobile devices.
- Launching from the device's home screen.
- Offline application launch after the application has been loaded and cached.
- Viewing shopping lists without an internet connection.
- Adding, editing, and removing items offline.
- Checking and unchecking shopping-list items offline.
- Saving changes locally without requiring a network connection.

All application data is stored locally on the user's device.

The MVP does not require:

- User accounts.
- Authentication.
- A hosted database.
- A backend service.
- Cloud synchronization.

Changes to shopping activity should be persisted automatically.

The application should use local structured storage appropriate for its relational data model.

Users should be informed that clearing browser data or losing the device may result in loss of their information.

Data export/import, backup, and synchronization between devices are future enhancements.

## 13. Technical and Project Constraints

The MVP should respect the existing project environment:

- Next.js with App Router.
- React.
- TypeScript.
- Tailwind CSS.
- Static export compatible with Cloudflare Pages.
- Free hosting and services.
- No required backend or database service.

New dependencies and architectural complexity should be introduced only when justified by a concrete requirement.

The application should be maintainable, accessible, responsive, and structured to support future enhancements without unnecessary abstraction.

## 14. Explicitly Deferred Enhancements

The following features are not required for the initial MVP:

- Item prices and price tracking.
- Estimated shopping totals.
- Multiple named shopping lists per branch.
- Shopping-list archives and historical purchases.
- Merging regular items into an existing list.
- Copying store configurations between branches.
- Advanced autocomplete prioritization and personalization.
- Advanced shopping-order learning algorithms.
- Cross-device synchronization.
- Cloud backups and accounts.
- Preserving deleted catalog items on existing shopping lists.
- Advanced analytics and shopping history.

These may be reconsidered after the MVP is implemented and evaluated through actual use.

## 15. Outstanding Design Decisions

The following details should be resolved before or during the relevant implementation planning:

1. Exact behavior for combining shopping-order observations across multiple trips.
2. Whether learned shopping order automatically becomes the default list ordering or remains a selectable option.
3. Exact category list and whether users can create their own categories.
4. Supported quantity formats and units.
5. Detailed behavior when a store area is deleted while grocery items are assigned to it.
6. Detailed behavior when a physical store branch is deleted.
7. Whether an item can be assigned to more than one store area at the same branch (expected to be no for the MVP unless a clear use case emerges).

These unresolved details should not prevent progress on the broader product design.
