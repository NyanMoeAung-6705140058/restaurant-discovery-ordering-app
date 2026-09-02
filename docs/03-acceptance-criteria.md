# Restaurant Discovery & Ordering App — Acceptance Criteria

## Document Control

| Item | Detail |
| --- | --- |
| Document | MVP Acceptance Criteria |
| Version | 1.0 |
| Status | Test baseline |
| Related documents | `01-project-charter.md`, `02-requirements-specification.md`, `04-database-design.md` |

## 1. Purpose and Test Rules

These criteria define the observable conditions that the MVP must satisfy. Every criterion has a unique ID and traces to one or more requirements in `02-requirements-specification.md`.

Unless a criterion says otherwise:

- The test environment contains approved restaurant, location, rating, menu, and item data.
- Distance is calculated from WGS 84 coordinates.
- The default radius is 5 km and the valid selectable range is 1–50 km.
- Price levels are integers 1–4.
- Ratings use the 1–5 scale.
- Menu prices use one configured currency.
- The tester begins with a supported browser and a working network connection.
- **Must** criteria block release when they fail. **Should** criteria require a documented product-owner decision when they fail.

## 2. MVP Feature 1 — Discover Restaurants Near the User

### AC-LOC-01 — Discover with Current Location

- **Related requirements:** FR-01, FR-02, FR-04, FR-09; LS-01, LS-03, LS-04
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** Location service is available. The customer has not yet answered the permission request. Active restaurants exist within 5 km.
- **Test steps:**
  1. Open the discovery page.
  2. Select **Use my location**.
  3. Grant location permission.
  4. Wait for results.
- **Expected result:** The app obtains the coordinates, searches within 5 km, and displays each eligible restaurant location once with its name, area, distance, rating information, price level, and operating status when available. No location is shown if its calculated distance exceeds 5 km.
- **Given–When–Then:** **Given** location permission is available and nearby active restaurants exist, **when** the customer grants permission, **then** the system displays the eligible restaurant locations within the radius with calculated distances.

### AC-LOC-02 — Permission Denied with Manual Alternative

- **Related requirements:** FR-01, FR-03, FR-08; LS-02, SP-02
- **Scenario type:** Negative and recovery
- **Priority:** Must
- **Preconditions:** The customer can deny the device/browser location request.
- **Test steps:**
  1. Select **Use my location**.
  2. Deny location permission.
  3. Observe the message and available controls.
  4. Enter a recognized area manually and submit.
- **Expected result:** The app does not repeatedly force the permission request. It explains that current location is unavailable, offers manual entry, resolves the valid manual location, and displays eligible nearby restaurants.
- **Given–When–Then:** **Given** the customer denies location permission, **when** the discovery page handles that response, **then** it offers manual location entry and completes a search after a valid place is entered.

### AC-LOC-03 — Invalid or Ambiguous Manual Location

- **Related requirements:** FR-03, FR-08; LS-06
- **Scenario type:** Negative
- **Priority:** Must
- **Preconditions:** Manual location entry is available.
- **Test steps:**
  1. Enter a value that cannot be resolved and submit.
  2. Confirm that no search is run with invented coordinates.
  3. Enter a location that returns more than one plausible match.
- **Expected result:** For an unrecognized value, the app shows a clear validation message and allows correction. For an ambiguous value, it asks the customer to select or refine a match. It never silently selects an uncertain location.
- **Given–When–Then:** **Given** a manual location is invalid or ambiguous, **when** it is submitted, **then** the system requests correction or selection and does not use uncertain coordinates.

### AC-LOC-04 — Search-Radius Boundary

- **Related requirements:** FR-04; BR-01, LS-05
- **Scenario type:** Boundary
- **Priority:** Must
- **Preconditions:** Test Restaurant A is calculated to be exactly 5.000 km from the search point, and Restaurant B is 5.001 km away.
- **Test steps:**
  1. Search from the configured point with a radius of 5 km.
  2. Inspect the returned restaurant identifiers and calculated distances.
- **Expected result:** Restaurant A is included and Restaurant B is excluded. Rounding used for display does not change the inclusion decision.
- **Given–When–Then:** **Given** restaurants are on and just outside the selected radius, **when** the search is performed, **then** the restaurant on the boundary is included and the restaurant outside it is excluded.

### AC-LOC-05 — Location-Service Timeout

- **Related requirements:** FR-08; EIR-03, LS-07
- **Scenario type:** Error handling
- **Priority:** Must
- **Preconditions:** The geolocation or geocoding provider is configured to time out.
- **Test steps:**
  1. Start a current-location or manual-location search.
  2. Allow the provider request to exceed the configured timeout.
  3. Select **Retry** once.
- **Expected result:** The app stops waiting, shows a safe and understandable error, provides retry and manual-entry options where applicable, and does not display stale results as if they came from the failed request.
- **Given–When–Then:** **Given** the location provider times out, **when** the timeout limit is reached, **then** the app shows a recoverable error without exposing internal details.

### AC-LOC-06 — Valid Search with No Nearby Restaurants

- **Related requirements:** FR-04, FR-08
- **Scenario type:** Empty result
- **Priority:** Must
- **Preconditions:** The search point is valid, but no active restaurant location exists within the selected radius.
- **Test steps:**
  1. Search from the configured point.
  2. Observe the result area.
- **Expected result:** The app shows a **No restaurants found nearby** state, preserves the location, and offers a suitable action such as increasing the radius or changing the location. It does not show a technical error.
- **Given–When–Then:** **Given** the search is valid but no restaurant qualifies, **when** results are returned, **then** the system shows an empty state and useful next action.

## 3. MVP Feature 2 — View Restaurants by Rating

### AC-RAT-01 — Sort Ratings from Highest to Lowest

- **Related requirements:** FR-05, FR-26; BR-04
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** Nearby results include valid average ratings of 4.8, 4.5, 4.5, and 3.9.
- **Test steps:**
  1. Load nearby results.
  2. Select **Rating: High to low**.
  3. Record the displayed ratings in order.
- **Expected result:** The sequence is 4.8, 4.5, 4.5, and 3.9. Each value is displayed to one decimal place with its valid rating count.
- **Given–When–Then:** **Given** restaurants have different valid average ratings, **when** rating sort is selected, **then** no lower-rated restaurant appears before a higher-rated restaurant.

### AC-RAT-02 — Equal-Rating Results

- **Related requirements:** FR-05, FR-26; BR-04
- **Scenario type:** Boundary/tie
- **Priority:** Must
- **Preconditions:** At least two eligible restaurants have the same average rating.
- **Test steps:**
  1. Apply rating sort.
  2. Refresh or repeat the same request.
  3. Compare the result sequence.
- **Expected result:** Both equal-rated restaurants remain in the correct rating group, no lower rating appears between them, and repeated requests return a stable result order.
- **Given–When–Then:** **Given** two restaurants have equal ratings, **when** results are sorted by rating, **then** they are grouped together in a stable order without affecting the primary rating rule.

### AC-RAT-03 — Restaurant with No Ratings

- **Related requirements:** FR-05, FR-26; BR-05
- **Scenario type:** Negative/missing data
- **Priority:** Must
- **Preconditions:** Nearby results contain at least one rated restaurant and one restaurant with no valid ratings.
- **Test steps:**
  1. Apply rating sort from highest to lowest.
  2. Inspect the unrated restaurant.
- **Expected result:** Rated restaurants appear first. The unrated restaurant appears afterward and displays **New / No ratings**, not `0.0` stars or a fabricated value.
- **Given–When–Then:** **Given** a restaurant has no valid rating, **when** rating-sorted results are shown, **then** it is clearly labeled and placed after rated restaurants.

### AC-RAT-04 — Rating Request Failure

- **Related requirements:** FR-05, FR-08, FR-26
- **Scenario type:** Error handling
- **Priority:** Must
- **Preconditions:** The restaurant-results API is configured to return an error while rating sort is active.
- **Test steps:**
  1. Select rating sort.
  2. Trigger the failed response.
  3. Select **Retry** after restoring the service.
- **Expected result:** The app shows a safe error and retry action, retains the selected sort, and displays correctly sorted results after a successful retry. It does not present unsorted data as rating-sorted data.
- **Given–When–Then:** **Given** the sorted-result request fails, **when** the app receives the failure, **then** it shows a recoverable error and retains the customer's selection.

## 4. MVP Feature 3 — View Restaurants by Price

### AC-PRI-01 — Filter by One Price Level

- **Related requirements:** FR-06; BR-03
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** Nearby restaurants exist at price levels 1, 2, 3, and 4.
- **Test steps:**
  1. Select only price level 2.
  2. Apply the filter.
  3. Inspect every returned restaurant.
- **Expected result:** Every displayed restaurant has price level 2. Restaurants at levels 1, 3, and 4 are not displayed, and the active filter is visible.
- **Given–When–Then:** **Given** restaurants have different price levels, **when** only level 2 is selected, **then** only eligible level-2 restaurants are shown.

### AC-PRI-02 — Combine Multiple Price Levels with Rating Sort

- **Related requirements:** FR-05, FR-06, FR-07
- **Scenario type:** Positive combination
- **Priority:** Must
- **Preconditions:** Nearby results include several restaurants at price levels 1 and 2 with different ratings.
- **Test steps:**
  1. Select price levels 1 and 2.
  2. Select rating sort from highest to lowest.
  3. Inspect price levels and rating order.
- **Expected result:** Only level-1 and level-2 restaurants appear, and rated results are ordered from highest to lowest. Both controls remain visibly active when the customer opens a restaurant and returns to results.
- **Given–When–Then:** **Given** multiple price levels and rating sort are selected, **when** results load, **then** the system satisfies both conditions together.

### AC-PRI-03 — Minimum and Maximum Price-Level Boundaries

- **Related requirements:** FR-06, FR-07; BR-03
- **Scenario type:** Boundary
- **Priority:** Must
- **Preconditions:** Eligible restaurants exist at price levels 1 and 4.
- **Test steps:**
  1. Select level 1 only and verify results.
  2. Clear the selection.
  3. Select level 4 only and verify results.
- **Expected result:** Level 1 and level 4 are both accepted as valid boundary selections and return only restaurants at the selected boundary. Values below 1 or above 4 cannot be submitted through the UI and are rejected by the API.
- **Given–When–Then:** **Given** valid minimum and maximum price levels exist, **when** either boundary is selected, **then** the correct restaurants are returned and out-of-range values are rejected.

### AC-PRI-04 — Price Filter with No Matches

- **Related requirements:** FR-06, FR-08
- **Scenario type:** Empty result
- **Priority:** Must
- **Preconditions:** Nearby restaurants exist, but none has the selected price level.
- **Test steps:**
  1. Apply the unmatched price filter.
  2. Observe the result area.
  3. Clear the filter.
- **Expected result:** The app shows **No restaurants match your filters**, displays a clear-filter action, and restores the unfiltered nearby results after the filter is cleared.
- **Given–When–Then:** **Given** no nearby restaurant matches the selected price, **when** the filter is applied, **then** the app shows a filter-specific empty state and a recovery action.

## 5. MVP Feature 4 — Select a Restaurant and Browse Its Menu

### AC-MEN-01 — Open an Active Restaurant Menu

- **Related requirements:** FR-11, FR-12
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** An active restaurant location has one active menu with active categories and items.
- **Test steps:**
  1. Select the restaurant from discovery results.
  2. Wait for the restaurant page to load.
- **Expected result:** The page shows the correct restaurant location details and active menu. Data from another restaurant or location is not mixed into the page.
- **Given–When–Then:** **Given** an active restaurant has an active menu, **when** the customer selects the restaurant, **then** the correct restaurant details and menu are displayed.

### AC-MEN-02 — Menu Category and Item Order

- **Related requirements:** FR-12
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** The menu has categories and items with configured display-order values.
- **Test steps:**
  1. Open the menu.
  2. Compare displayed categories and items with the approved test data.
- **Expected result:** Active categories and active items appear once in configured order. Inactive categories and inactive items are not presented as orderable menu choices.
- **Given–When–Then:** **Given** menu content has active states and display order, **when** the menu opens, **then** active content is displayed once in the configured sequence.

### AC-MEN-03 — Unavailable Menu Item

- **Related requirements:** FR-13, FR-14; BR-10
- **Scenario type:** Negative
- **Priority:** Must
- **Preconditions:** An active menu item is marked unavailable.
- **Test steps:**
  1. Open its menu category.
  2. Inspect the item.
  3. Attempt to activate its add control using pointer and keyboard.
- **Expected result:** The item is clearly marked **Unavailable**. Its add control is disabled or absent for all tested input methods, and no cart item is created.
- **Given–When–Then:** **Given** a menu item is unavailable, **when** it is displayed, **then** the customer can see its status but cannot add it to the cart.

### AC-MEN-04 — Missing or Failed Menu

- **Related requirements:** FR-08, FR-11, FR-12
- **Scenario type:** Empty result and error handling
- **Priority:** Must
- **Preconditions:** Test once with no active menu and once with the menu API returning an error.
- **Test steps:**
  1. Open the restaurant in each test condition.
  2. Observe the menu area.
- **Expected result:** With no active menu, the app shows a non-technical **Menu not available** state. With an API failure, it shows a safe error and retry action. Neither condition displays stale menu items as currently orderable.
- **Given–When–Then:** **Given** a menu is absent or cannot be retrieved, **when** the restaurant page opens, **then** the app shows the correct empty or error state and prevents ordering from stale data.

## 6. MVP Feature 5 — View Menu Items by Calories

### AC-CAL-01 — Display a Supplied Calorie Value

- **Related requirements:** FR-13; BR-06
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** A menu item has `calories_kcal = 450` in the approved data.
- **Test steps:**
  1. Open the item's menu category.
  2. Inspect the item card and accessible label.
- **Expected result:** The item displays **450 kcal** and an assistive technology user can identify the value and unit.
- **Given–When–Then:** **Given** a valid calorie value is supplied, **when** the menu item is shown, **then** the value is displayed as a whole number followed by `kcal`.

### AC-CAL-02 — Zero-Calorie Boundary

- **Related requirements:** FR-13; BR-06
- **Scenario type:** Boundary
- **Priority:** Must
- **Preconditions:** A valid item has `calories_kcal = 0`.
- **Test steps:**
  1. Open the menu containing the item.
  2. Inspect its calorie display.
- **Expected result:** The item displays **0 kcal**. Zero is not changed to **Not available** and is not rejected as missing.
- **Given–When–Then:** **Given** zero is a valid supplied calorie value, **when** the item is displayed, **then** the system shows `0 kcal`.

### AC-CAL-03 — Calories Not Supplied

- **Related requirements:** FR-13; BR-06
- **Scenario type:** Missing data
- **Priority:** Must
- **Preconditions:** A menu item's calorie field is NULL.
- **Test steps:**
  1. Open the menu containing the item.
  2. Inspect its calorie display.
- **Expected result:** The item displays **Calories: Not available**. The system does not display zero, hide the label in a misleading way, or estimate a value.
- **Given–When–Then:** **Given** no calorie value was supplied, **when** the item is displayed, **then** the system clearly states that calories are not available.

### AC-CAL-04 — Invalid Negative Calorie Value

- **Related requirements:** FR-13, FR-28; BR-06
- **Scenario type:** Negative/data validation
- **Priority:** Must
- **Preconditions:** An authorized data request attempts to create or update an item with `calories_kcal = -1`.
- **Test steps:**
  1. Submit the invalid value to the administrative API or import validation process.
  2. Query the item through the customer API.
- **Expected result:** Validation rejects the write with an identified field error, the database remains unchanged, and a negative calorie value is never displayed to customers.
- **Given–When–Then:** **Given** a negative calorie value is invalid, **when** it is submitted, **then** the system rejects it and preserves the last valid item data.

## 7. MVP Feature 6 — Select Menu Items and Place an Order

### AC-ORD-01 — Add an Available Item to the Cart

- **Related requirements:** FR-14, FR-15, FR-18; BR-07, BR-09, BR-10, BR-11
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** The item is active, available, priced at 120.00 in the configured currency, and belongs to an active menu and restaurant location. The cart is empty.
- **Test steps:**
  1. Add the item with quantity 2.
  2. Open the cart.
- **Expected result:** The cart contains the correct item once with quantity 2, unit price 120.00, and line total 240.00. The subtotal and total are 240.00.
- **Given–When–Then:** **Given** an item is valid and available, **when** quantity 2 is added, **then** the cart shows the correct item, quantity, price, and totals.

### AC-ORD-02 — Quantity Boundaries and Removal

- **Related requirements:** FR-16, FR-18; BR-09
- **Scenario type:** Boundary
- **Priority:** Must
- **Preconditions:** One available item is in the cart.
- **Test steps:**
  1. Set quantity to 1 and attempt to reduce below 1.
  2. Use the supported remove interaction and confirm removal.
  3. Add the item again and set quantity to 99.
  4. Attempt to increase above 99 and send quantities 0, 100, and 1.5 directly to the API.
- **Expected result:** Quantities 1 and 99 are accepted. The UI does not retain an item with quantity 0; the remove interaction removes it. Values below 1, above 99, or non-integers are rejected, and totals remain correct.
- **Given–When–Then:** **Given** quantity must be a whole number from 1 to 99, **when** boundary and invalid values are attempted, **then** valid boundaries work and invalid values cannot change the cart.

### AC-ORD-03 — Add an Item from Another Restaurant

- **Related requirements:** FR-17; BR-08
- **Scenario type:** Negative and recovery
- **Priority:** Must
- **Preconditions:** The cart contains an item from Restaurant A, and an available item from Restaurant B is displayed.
- **Test steps:**
  1. Add the Restaurant B item.
  2. Cancel the clear-cart confirmation.
  3. Try again and confirm replacement.
- **Expected result:** On the first attempt, Restaurant A's cart remains unchanged and no Restaurant B item is added. On confirmation, the old cart is cleared and a new Restaurant B cart is created. Items from both restaurants never coexist in one cart.
- **Given–When–Then:** **Given** the cart belongs to Restaurant A, **when** a Restaurant B item is selected, **then** the app requires confirmation before replacing the cart.

### AC-ORD-04 — Review the Order

- **Related requirements:** FR-18, FR-20; BR-11, BR-13, BR-16
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** The cart contains two valid items and the customer is authenticated.
- **Test steps:**
  1. Open checkout.
  2. Enter valid contact details and an optional note of 500 characters or fewer.
  3. Review all displayed information.
- **Expected result:** Checkout shows the correct restaurant location, pickup method, item names, quantities, unit prices, disclosed calories, line totals, subtotal, total, currency, contact details, and sanitized note. The total equals the sum of line totals and contains no unapproved fees.
- **Given–When–Then:** **Given** a valid cart and customer details, **when** checkout is opened, **then** all order information and calculated totals are available for review before submission.

### AC-ORD-05 — Sign In Without Losing the Cart

- **Related requirements:** FR-19; BR-12, SP-03, SP-09
- **Scenario type:** Authentication
- **Priority:** Must
- **Preconditions:** A visitor has a valid non-empty cart.
- **Test steps:**
  1. Select **Checkout**.
  2. Complete a valid sign-in.
  3. Return to checkout.
- **Expected result:** The app requires authentication, completes sign-in securely, and restores the same valid cart once. The customer can continue to checkout without adding the items again.
- **Given–When–Then:** **Given** a visitor has a valid cart, **when** the visitor signs in from checkout, **then** the cart is preserved and associated with the authenticated journey.

### AC-ORD-06 — Price or Availability Changes at Checkout

- **Related requirements:** FR-21; BR-07, BR-10, BR-11
- **Scenario type:** Negative/concurrency
- **Priority:** Must
- **Preconditions:** Item A becomes unavailable and Item B's price changes after both were added to the cart.
- **Test steps:**
  1. Continue to checkout.
  2. Attempt to submit the order.
  3. Review the returned changes.
- **Expected result:** The order is not created using stale data. The unavailable item is identified for removal, the new price is shown, totals are recalculated, and the customer must review the updated cart before a new submission.
- **Given–When–Then:** **Given** cart data changed after selection, **when** final validation runs, **then** the system blocks stale submission and asks the customer to review accurate data.

### AC-ORD-07 — Successful Atomic Order Submission

- **Related requirements:** FR-22, FR-24, FR-27; BR-13, BR-15
- **Scenario type:** Positive
- **Priority:** Must
- **Preconditions:** The customer is authenticated, checkout data is valid, all items remain available at the reviewed prices, and the receiving restaurant location is active.
- **Test steps:**
  1. Select **Place order** once.
  2. Wait for the result.
  3. Retrieve the order using its returned identifier.
- **Expected result:** Exactly one committed order and its order-item snapshots exist. The app shows one unique order number, restaurant, pickup method, items, total, and initial status. Authorized restaurant staff can access the order for the correct location; unrelated staff cannot.
- **Given–When–Then:** **Given** checkout is valid, **when** the customer submits once, **then** exactly one complete order is committed and a matching confirmation is displayed.

### AC-ORD-08 — Duplicate Submission or Network Retry

- **Related requirements:** FR-23, FR-24, FR-25; BR-14, NFR-10
- **Scenario type:** Error recovery/idempotency
- **Priority:** Must
- **Preconditions:** A valid submission has a fixed idempotency key. The first response is delayed or lost after the transaction commits.
- **Test steps:**
  1. Send the submission.
  2. Retry with the same authenticated customer and idempotency key.
  3. Query orders for that key.
- **Expected result:** Only one order exists. The retry returns the existing successful result or enables retrieval of that result; it does not create a second order or charge.
- **Given–When–Then:** **Given** the first order may already be committed, **when** the same submission is retried with the same idempotency key, **then** the system returns one order outcome without duplication.

### AC-ORD-09 — Order Transaction Failure

- **Related requirements:** FR-08, FR-22, FR-23; NFR-10, SP-08
- **Scenario type:** Error handling/rollback
- **Priority:** Must
- **Preconditions:** A controlled database failure occurs after order processing begins but before the transaction commits.
- **Test steps:**
  1. Submit an otherwise valid order.
  2. Trigger the controlled failure.
  3. Inspect the customer response and database.
  4. Restore service and retry safely.
- **Expected result:** No partial order or orphaned order item is committed. The customer receives a safe message that does not falsely confirm success or expose internal details. After outcome reconciliation, a safe retry can create no more than one order.
- **Given–When–Then:** **Given** the order transaction cannot complete, **when** the failure occurs, **then** all database changes roll back and the app presents a safe recovery path.

## 8. Acceptance Summary

| MVP feature | Positive coverage | Negative or missing-data coverage | Boundary coverage | Error/recovery coverage |
| --- | --- | --- | --- | --- |
| Nearby discovery | AC-LOC-01 | AC-LOC-02, AC-LOC-03, AC-LOC-06 | AC-LOC-04 | AC-LOC-05 |
| Rating view/sort | AC-RAT-01 | AC-RAT-03 | AC-RAT-02 | AC-RAT-04 |
| Price view/filter | AC-PRI-01, AC-PRI-02 | AC-PRI-04 | AC-PRI-03 | AC-PRI-04 |
| Restaurant/menu view | AC-MEN-01, AC-MEN-02 | AC-MEN-03, AC-MEN-04 | Not applicable to the stated MVP | AC-MEN-04 |
| Calorie display | AC-CAL-01 | AC-CAL-03, AC-CAL-04 | AC-CAL-02 | AC-CAL-04 |
| Cart and ordering | AC-ORD-01, AC-ORD-04, AC-ORD-07 | AC-ORD-03, AC-ORD-05, AC-ORD-06 | AC-ORD-02 | AC-ORD-08, AC-ORD-09 |

## 9. Release Decision

The product owner may accept the MVP only when every Must-priority criterion above passes in the approved environment, test evidence is recorded, and no unresolved critical or high-severity defect remains.
