# Restaurant Discovery & Ordering App — Requirements Specification

## Document Control

| Item | Detail |
| --- | --- |
| Document | Software Requirements Specification (SRS) |
| Version | 1.0 |
| Status | MVP baseline |
| Related documents | `01-project-charter.md`, `03-acceptance-criteria.md`, `04-database-design.md` |

## 1. Purpose

This document defines the functional, non-functional, business, data, interface, location, security, and privacy requirements for the Restaurant Discovery & Ordering App MVP. It is the baseline for design, development, testing, and product acceptance.

The words **must** and **shall** indicate mandatory MVP requirements. **Should** indicates a recommended requirement that may be deferred only with product-owner approval.

## 2. System Overview

The system provides one customer journey:

1. Determine the customer's current or manually entered location.
2. Find restaurants within the selected search radius.
3. Compare results by rating and price level.
4. Open a restaurant and browse its active menu.
5. Review prices, calories, and availability.
6. Add items from one restaurant to a cart.
7. Sign in, review the cart, and submit a pickup order.
8. Receive a unique order confirmation.

The MVP consists of a responsive customer interface, secured backend APIs, a relational database, and integrations with location/geocoding services. Controlled operational processes maintain restaurant and menu data and allow restaurant staff to process orders; a full restaurant-owner portal is outside scope.

## 3. Assumptions and Dependencies

### 3.1 Assumptions

- No usable reference attachment was available when this baseline was prepared. The approved task description and project charter are the source of truth.
- The pilot operates in one city and one configurable ISO 4217 currency.
- Nearby search defaults to 5 km and cannot exceed 50 km.
- Restaurant price level uses values 1 through 4, where 1 is lowest and 4 is highest.
- Browsing and cart building do not require sign-in; order submission does.
- The MVP supports pickup orders and does not process payment.
- Each cart and order contains menu items from one restaurant location only.
- Calorie information may be unknown and must then appear as **Not available**.
- Restaurant and menu data are supplied through approved, controlled processes.

### 3.2 Dependencies

- A supported browser or mobile device with network access.
- Device/browser geolocation capability when current location is used.
- A third-party geocoding or maps service for manual-location resolution and map-related data.
- Accurate and timely data from participating restaurants.
- Identity, API hosting, database hosting, monitoring, and notification infrastructure.

## 4. Target Users

| User group | Need |
| --- | --- |
| Local customers | Quickly find suitable restaurants and order food. |
| Price-conscious customers | Compare restaurants using a simple price-level filter. |
| Rating-conscious customers | See and sort by average customer rating. |
| Nutrition-conscious customers | View disclosed calorie information before selecting food. |
| Restaurant operations staff | Receive and update the status of submitted orders. |
| Authorized data administrators | Maintain accurate restaurant, location, menu, price, calorie, and availability data. |

## 5. User Roles

| Role | Main permissions |
| --- | --- |
| Visitor | Share or enter location, browse restaurants, sort/filter results, view menus, and build a temporary cart. |
| Registered customer | All visitor permissions plus submit an order and view the resulting order confirmation/status. |
| Restaurant staff | View orders assigned to an authorized restaurant location and update permitted order statuses. |
| Data administrator | Create and update approved restaurant, location, menu, category, item, price, calorie, availability, and rating-source data. |
| System administrator | Manage access, configuration, monitoring, recovery, and audit information. |

Roles other than visitor and registered customer are supporting roles. A complete self-service restaurant portal is not part of the MVP.

## 6. User Stories

| ID | User story | Priority |
| --- | --- | --- |
| US-01 | As a visitor, I want to use my current location so that I can see nearby restaurants. | Must |
| US-02 | As a visitor, I want to enter a location manually when location access is unavailable or denied. | Must |
| US-03 | As a visitor, I want to see restaurant distance, rating, and price level so that I can compare options. | Must |
| US-04 | As a visitor, I want to sort restaurants by rating so that highly rated options appear first. | Must |
| US-05 | As a visitor, I want to filter restaurants by price level so that results match my budget. | Must |
| US-06 | As a visitor, I want to open a restaurant and browse its menu by category. | Must |
| US-07 | As a nutrition-conscious visitor, I want to see calories for each item when available. | Must |
| US-08 | As a visitor, I want to add available menu items to a cart and change quantities. | Must |
| US-09 | As a customer, I want to review current prices and my order total before submission. | Must |
| US-10 | As a registered customer, I want to submit my order once and receive a unique confirmation. | Must |
| US-11 | As a customer, I want a clear message when a restaurant, menu item, location service, or order service is unavailable. | Must |
| US-12 | As restaurant staff, I want to receive a complete, accurate order for my location. | Must |

## 7. Functional Requirements

### 7.1 Location and Nearby Discovery

| ID | Requirement | Priority |
| --- | --- | --- |
| FR-01 | Before using device location, the system shall explain why location is needed and request permission through the supported device/browser mechanism. | Must |
| FR-02 | When permission is granted, the system shall obtain the customer's coordinates and use them only for the requested nearby search and permitted operational purposes. | Must |
| FR-03 | The system shall allow a customer to enter a valid address, area, postcode, or recognized place manually and shall convert it to coordinates. | Must |
| FR-04 | The system shall return active restaurant locations within the selected radius and show the calculated distance from the search point. | Must |
| FR-05 | The system shall allow returned restaurants to be sorted by average rating from highest to lowest. | Must |
| FR-06 | The system shall allow one or more restaurant price levels, from 1 through 4, to be selected as filters. | Must |
| FR-07 | The system shall apply location, radius, rating sort, and price filters together and retain the current selections while the customer remains in the discovery journey. | Must |
| FR-08 | The system shall show a clear empty, permission, invalid-location, timeout, or service-error state and an appropriate recovery action. | Must |
| FR-09 | Each result shall show restaurant name, location name or area, distance, average rating, rating count, price level, and open/closed status when operating hours are available. | Must |
| FR-10 | The system shall paginate or progressively load restaurant results without displaying duplicate locations. | Should |

### 7.2 Restaurant and Menu Viewing

| ID | Requirement | Priority |
| --- | --- | --- |
| FR-11 | A customer shall be able to open a selected restaurant location and view its address, contact information, rating, price level, and active menu. | Must |
| FR-12 | The system shall display active menu categories and items in the configured category and item order. | Must |
| FR-13 | Each menu item shall show its name, description when available, current price and currency, calorie value or **Not available**, and availability state. | Must |
| FR-14 | An unavailable or inactive item shall remain clearly identified and shall not be addable to the cart. | Must |

### 7.3 Cart and Ordering

| ID | Requirement | Priority |
| --- | --- | --- |
| FR-15 | A customer shall be able to add an available menu item with a valid quantity to a cart. | Must |
| FR-16 | A customer shall be able to view the cart, increase or decrease quantity within allowed limits, and remove an item. | Must |
| FR-17 | The system shall restrict a cart to one restaurant location and request confirmation before clearing a non-empty cart to add an item from another restaurant. | Must |
| FR-18 | The system shall calculate each line total, subtotal, and final MVP total using current stored prices and integer quantities. | Must |
| FR-19 | The system shall require a customer to authenticate before final order submission while preserving the valid cart through the sign-in journey. | Must |
| FR-20 | Checkout shall display the restaurant location, selected items, quantities, unit prices, disclosed calories, subtotal, total, pickup method, customer contact details, and optional notes for review. | Must |
| FR-21 | Immediately before creating an order, the system shall revalidate restaurant status, item status, availability, quantity, and price. It shall present any change and require customer review. | Must |
| FR-22 | The system shall create the order, order items, price snapshots, and initial status in one atomic transaction. | Must |
| FR-23 | The system shall use an idempotency key so that retries of the same submission do not create duplicate orders. | Must |
| FR-24 | After successful submission, the system shall show a unique order number, restaurant, pickup method, item summary, total, and current order status. | Must |
| FR-25 | The registered customer shall be able to retrieve the newly submitted order and its latest recorded status. | Should |

### 7.4 Ratings and Supporting Operations

| ID | Requirement | Priority |
| --- | --- | --- |
| FR-26 | The system shall display the average rating rounded to one decimal place and the number of valid ratings; a restaurant with no ratings shall show **New / No ratings** rather than a misleading zero-star score. | Must |
| FR-27 | Authorized restaurant staff shall see only orders for assigned restaurant locations and update only permitted status transitions. | Must |
| FR-28 | Authorized administrators shall be able to maintain the data required by the MVP through a controlled interface, import, or administrative API. | Must |

## 8. Non-Functional Requirements

| ID | Requirement | Verification |
| --- | --- | --- |
| NFR-01 | At least 95% of restaurant and menu page requests shall complete within 2 seconds under the agreed normal load, excluding unavailable third-party services. | Performance test |
| NFR-02 | At least 95% of nearby searches shall complete within 3 seconds under the agreed normal load. | Performance test |
| NFR-03 | A valid order submission shall return a result within 5 seconds for at least 95% of attempts under the agreed normal load. | Performance test |
| NFR-04 | The production service target shall be 99.5% monthly availability, excluding approved maintenance. | Monitoring report |
| NFR-05 | The system shall support at least 500 concurrent active customer sessions in the MVP load test without violating NFR-01 through NFR-03. | Load test |
| NFR-06 | All external and internal network communication carrying customer or order data shall use TLS 1.2 or higher. | Security review |
| NFR-07 | Passwords shall be processed by an approved identity service or stored only as salted, adaptive hashes; plaintext passwords are prohibited. | Code/configuration review |
| NFR-08 | Customer pages shall meet WCAG 2.1 Level AA for the tested critical journey, including keyboard access, labels, focus visibility, and color contrast. | Accessibility audit |
| NFR-09 | The responsive application shall support the latest two stable versions of Chrome, Safari, Firefox, and Edge at release. | Compatibility test |
| NFR-10 | Order creation shall be atomic and idempotent and shall never return success unless the committed order can be retrieved. | Integration and recovery test |
| NFR-11 | Logs shall use UTC timestamps, correlation IDs, and non-sensitive operational details; secrets, passwords, full tokens, and unnecessary precise location data shall not be logged. | Log review |
| NFR-12 | Critical service and order failures shall produce an operational alert within 5 minutes. | Monitoring test |
| NFR-13 | The codebase shall use documented APIs, automated tests, version control, and peer review for protected branches. | Delivery audit |
| NFR-14 | Database backups shall run at least daily, and a tested recovery procedure shall meet an MVP recovery-point objective of 24 hours and recovery-time objective of 4 hours. | Recovery exercise |

## 9. Business Rules

| ID | Rule |
| --- | --- |
| BR-01 | The default nearby radius is 5 km; the minimum selectable radius is 1 km and the maximum is 50 km. |
| BR-02 | A restaurant result is eligible only when both the restaurant and its selected location are active. |
| BR-03 | Restaurant price level must be an integer from 1 through 4. |
| BR-04 | A valid individual rating is an integer from 1 through 5. Average rating is the arithmetic mean of valid ratings, rounded to one decimal place for display. |
| BR-05 | A restaurant with no valid ratings is labeled **New / No ratings** and appears after rated restaurants in descending rating order unless another sort is chosen. |
| BR-06 | Calories are stored as a non-negative whole number of kilocalories or NULL when not supplied. NULL is displayed as **Not available**. |
| BR-07 | Menu item price must be zero or greater and use the application's configured currency. |
| BR-08 | A cart may contain items from only one restaurant location. |
| BR-09 | Item quantity must be a whole number from 1 through 99. Setting a quantity to zero through the decrease control removes the item after confirmation or an equivalent clear interaction. |
| BR-10 | Only active and available items from an active menu and restaurant location may be ordered. |
| BR-11 | The checkout total equals the sum of `current unit price × quantity` for all validated items. The MVP adds no tax, service, payment, or delivery fee unless the scope and requirements are formally changed. |
| BR-12 | A registered customer is required to submit an order; visitors may browse and build a temporary cart. |
| BR-13 | Order items store name, price, currency, and calorie snapshots so historical orders do not change when the menu changes. |
| BR-14 | Reusing the same idempotency key for the same customer and submission returns the existing outcome and does not create another order. |
| BR-15 | Allowed order status flow is `PENDING → CONFIRMED → PREPARING → READY → COMPLETED`; an authorized cancellation may move an eligible order to `CANCELLED`. Invalid transitions are rejected. |
| BR-16 | Customer notes are optional, plain text, sanitized, and limited to 500 characters. |

## 10. Data Requirements

| ID | Requirement |
| --- | --- |
| DR-01 | User data shall include a unique identifier, verified login identifier, display name, phone when needed for the order, status, and audit timestamps. |
| DR-02 | Restaurant data shall include a unique identifier, name, description, price level, active state, rating aggregate, rating count, and timestamps. |
| DR-03 | Restaurant-location data shall include address, latitude, longitude, contact details, timezone, active state, and optional operating-hours representation. |
| DR-04 | Menu data shall identify the restaurant location, name, active state, and validity dates when used. |
| DR-05 | Category and menu-item data shall preserve display order. Menu items shall include price, currency, optional calories, availability, and status. |
| DR-06 | Rating data shall identify its restaurant, score, source or customer where applicable, and creation time. Invalid or removed ratings shall not affect the published aggregate. |
| DR-07 | Cart data shall identify its customer or anonymous session, one restaurant location, items, quantities, and timestamps. |
| DR-08 | Order data shall include a unique public order number, customer, restaurant location, status, contact details, pickup method, totals, currency, notes, idempotency key, and timestamps. |
| DR-09 | Order-item data shall include menu-item reference where available and immutable name, price, currency, calorie, quantity, and line-total snapshots. |
| DR-10 | Data timestamps shall be stored in UTC; restaurant operating times shall be interpreted using the restaurant location's IANA timezone. |
| DR-11 | Required records shall use referential integrity, uniqueness rules, range checks, and transaction control as detailed in `04-database-design.md`. |
| DR-12 | Personal, precise-location, cart, and order data shall be retained only for an approved operational or legal period and then deleted or anonymized according to policy. |

## 11. External Interface Requirements

| ID | Interface | Requirement |
| --- | --- | --- |
| EIR-01 | Customer UI | The interface shall be responsive and provide accessible location, discovery, filter, restaurant, menu, cart, sign-in, checkout, and confirmation screens. |
| EIR-02 | Geolocation API | The application shall use the supported device/browser API only after customer permission and shall handle granted, denied, unavailable, and timeout outcomes. |
| EIR-03 | Geocoding/maps provider | Requests shall use secured credentials, configured timeouts, usage limits, and permitted caching. Provider errors shall be translated into safe customer messages. |
| EIR-04 | Backend REST/JSON API | API requests and responses shall use HTTPS, documented schemas, validation, authentication where required, stable error codes, and correlation IDs. |
| EIR-05 | Identity service | Authentication shall support secure sign-in, session expiry, logout, and account-status checks. |
| EIR-06 | Restaurant operations | Submitted orders shall be made available only to staff authorized for the receiving restaurant location. |

## 12. Location-Service Requirements

| ID | Requirement |
| --- | --- |
| LS-01 | The system shall not request device coordinates before displaying the purpose of location use. |
| LS-02 | A customer who denies permission shall still be able to use manual location entry. |
| LS-03 | Coordinates shall use WGS 84 latitude and longitude; latitude range is −90 to 90 and longitude range is −180 to 180. |
| LS-04 | Nearby distance shall be calculated geodesically and returned in kilometers, rounded for display without changing eligibility calculations. |
| LS-05 | Search eligibility shall include a restaurant exactly on the selected radius boundary. |
| LS-06 | An ambiguous manual location shall require the customer to select or refine a match; the system shall not silently choose an uncertain location. |
| LS-07 | A failed or timed-out provider call shall show a retry action and manual-entry option where applicable. |
| LS-08 | Exact customer coordinates shall not be retained beyond the current search unless retention is separately explained, necessary, and consented to. |

## 13. Security and Privacy Requirements

| ID | Requirement |
| --- | --- |
| SP-01 | The system shall collect only data necessary for discovery, authentication, ordering, support, security, and approved legal purposes. |
| SP-02 | Location purpose and permission choice shall be clear, and denial shall not prevent manual discovery. |
| SP-03 | Authentication and role-based authorization shall protect customer, staff, administrator, and restaurant-location data. |
| SP-04 | Server-side validation shall protect all input, including coordinates, filters, quantities, identifiers, contact details, and notes. |
| SP-05 | The system shall protect against common web risks, including injection, broken access control, cross-site scripting, cross-site request forgery where relevant, credential attacks, and insecure secret handling. |
| SP-06 | Sensitive data shall be encrypted in transit and protected at rest using approved platform controls. |
| SP-07 | Administrative and order-status changes shall create audit records containing actor, action, target, result, and timestamp without recording prohibited secrets. |
| SP-08 | Customer-facing errors shall not expose stack traces, SQL, credentials, internal paths, or sensitive identifiers. |
| SP-09 | Sessions shall use secure cookies or protected tokens, expiration, logout invalidation where supported, and rate limiting for authentication and order endpoints. |
| SP-10 | Privacy notice, retention periods, access/correction procedures, and deletion or anonymization rules shall be approved before public launch. |

## 14. Requirements Traceability Matrix

| Requirement | User story or source | Related rules | Acceptance criteria |
| --- | --- | --- | --- |
| FR-01, FR-02 | US-01 | LS-01, LS-03, SP-02 | AC-LOC-01, AC-LOC-02 |
| FR-03 | US-02 | LS-02, LS-06 | AC-LOC-02, AC-LOC-03 |
| FR-04 | US-01, US-03 | BR-01, BR-02, LS-04, LS-05 | AC-LOC-01, AC-LOC-04, AC-LOC-06 |
| FR-05 | US-04 | BR-04, BR-05 | AC-RAT-01, AC-RAT-02, AC-RAT-03, AC-RAT-04 |
| FR-06 | US-05 | BR-03 | AC-PRI-01, AC-PRI-02, AC-PRI-03, AC-PRI-04 |
| FR-07 | US-03, US-04, US-05 | BR-01, BR-03 | AC-PRI-02, AC-PRI-03 |
| FR-08 | US-11 | LS-06, LS-07, SP-08 | AC-LOC-02, AC-LOC-03, AC-LOC-05, AC-RAT-04, AC-PRI-04, AC-MEN-04, AC-ORD-09 |
| FR-09, FR-10 | US-03 | BR-02, BR-03, BR-04 | AC-LOC-01, AC-LOC-04 |
| FR-11, FR-12 | US-06 | BR-02, BR-10 | AC-MEN-01, AC-MEN-02, AC-MEN-04 |
| FR-13 | US-07 | BR-06, BR-07 | AC-CAL-01, AC-CAL-02, AC-CAL-03, AC-CAL-04 |
| FR-14 | US-08, US-11 | BR-10 | AC-MEN-03, AC-ORD-01 |
| FR-15, FR-16 | US-08 | BR-08, BR-09, BR-10 | AC-ORD-01, AC-ORD-02 |
| FR-17 | US-08 | BR-08 | AC-ORD-03 |
| FR-18 | US-09 | BR-07, BR-09, BR-11 | AC-ORD-01, AC-ORD-02, AC-ORD-04 |
| FR-19 | US-10 | BR-12, SP-03, SP-09 | AC-ORD-05 |
| FR-20 | US-09 | BR-11, BR-13, BR-16 | AC-ORD-04 |
| FR-21 | US-09, US-11 | BR-07, BR-10, BR-11 | AC-ORD-06 |
| FR-22, FR-23 | US-10, US-12 | BR-13, BR-14 | AC-ORD-07, AC-ORD-08, AC-ORD-09 |
| FR-24, FR-25 | US-10 | BR-13, BR-15 | AC-ORD-07, AC-ORD-08 |
| FR-26 | US-03, US-04 | BR-04, BR-05 | AC-RAT-01, AC-RAT-02, AC-RAT-03 |
| FR-27 | US-12 | BR-15, SP-03, SP-07 | AC-ORD-07 |
| FR-28 | Project charter | BR-02 through BR-07, SP-03, SP-07 | Verified by administrative API and data-validation tests |
| NFR-01 through NFR-05 | Project success criteria | — | Performance, availability, and load test suites |
| NFR-06 through NFR-14 | Security and operational quality | SP-01 through SP-10 | Security, accessibility, compatibility, monitoring, and recovery test suites |

## 15. MVP Release Requirement

The MVP may be released only when all Must-priority acceptance criteria pass, no critical or high-severity defects remain unresolved, and the product owner accepts any documented limitations affecting Should-priority requirements.
