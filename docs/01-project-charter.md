# Restaurant Discovery & Ordering App — Project Charter

## Document Control

| Item | Detail |
| --- | --- |
| Project name | Restaurant Discovery & Ordering App |
| Document | Project Charter |
| Version | 1.0 |
| Status | MVP baseline |
| Prepared date | September 2026 |
| Approved by | Project Sponsor / Product Owner |

## 1. Project Background

Customers often use several sources to find nearby restaurants, compare quality and price, check menu information, and place an order. Information may be incomplete or inconsistent, especially for menu prices and calories. Moving between different services makes the decision and ordering process slow.

The Restaurant Discovery & Ordering App will provide one simple experience for discovering nearby restaurants, comparing them by rating and price range, viewing menus and calorie information, and submitting an order.

## 2. Problem Statement

Potential customers need a convenient way to answer four questions:

1. Which restaurants are near me?
2. Which restaurants have good ratings and suitable prices?
3. What food is available and how many calories does it contain?
4. How can I select food and place an order without changing applications?

Existing information may be spread across maps, review services, social media, and restaurant ordering channels. This increases search time and can cause customers to make decisions using outdated or missing information.

## 3. Project Purpose

The purpose of this project is to develop and validate a minimum viable product (MVP) that combines restaurant discovery, basic comparison, menu transparency, and food ordering in one responsive application.

## 4. Project Objectives

The MVP will:

- Use a customer's current or manually entered location to find nearby restaurants.
- Allow customers to sort restaurant results by average rating.
- Allow customers to filter results by restaurant price level.
- Display restaurant details, menu categories, item prices, availability, and calories.
- Allow customers to add available items to a cart, review the cart, and submit an order.
- Protect personal and location data using consent, secure transmission, and access control.
- Meet the measurable success criteria stated in this charter before MVP release.

## 5. Project Scope

### 5.1 In-Scope Features

- Responsive web or mobile-friendly customer interface.
- Browser or device location-permission request.
- Manual location entry when permission is denied or location is unavailable.
- Nearby restaurant search within a configurable radius.
- Restaurant result cards showing name, distance, average rating, rating count, and price level.
- Rating-based sorting from highest to lowest.
- Price-level filtering using levels 1 through 4.
- Combined use of location, rating sort, and price filters.
- Restaurant detail and menu views.
- Menu categories and menu items.
- Menu item name, description, price, calorie value, and availability status.
- Shopping cart for items from one restaurant at a time.
- Quantity update, item removal, and total calculation.
- Customer sign-in before final order submission.
- Order review, validation, submission, and confirmation.
- Basic order statuses needed to record and process a submitted order.
- Application programming interfaces (APIs), relational database, validation, logging, and basic administration through controlled data-management processes.

### 5.2 Out-of-Scope Features

- Online card, mobile-wallet, or bank payment.
- Live delivery-driver assignment or tracking.
- Route optimization and delivery-fee calculation.
- Table reservations.
- Loyalty points, coupons, subscriptions, and promotions.
- Social-media functions and customer-to-customer messaging.
- Personalized recommendations or artificial-intelligence ranking.
- Customer review creation or moderation through the customer interface.
- Multi-language and multi-currency presentation in the first release.
- A complete restaurant-owner portal.
- Advanced analytics and business-intelligence dashboards.

## 6. Key Stakeholders

| Stakeholder | Interest or responsibility |
| --- | --- |
| Project sponsor | Approves scope, budget, milestones, and MVP release. |
| Product owner | Prioritizes requirements and accepts completed features. |
| Customers | Discover restaurants, compare options, view menus, and order food. |
| Participating restaurants | Provide accurate restaurant, menu, price, calorie, availability, and order data. |
| Restaurant operations staff | Receive and process submitted orders. |
| Project manager | Manages schedule, scope, risks, communication, and delivery. |
| UX/UI designer | Designs an accessible and simple customer journey. |
| Development team | Builds the user interface, APIs, location logic, and database. |
| Quality-assurance team | Verifies requirements, acceptance criteria, security, and regression quality. |
| Data administrator | Maintains authorized restaurant and menu data. |
| Legal/privacy adviser | Reviews consent, personal-data handling, and retention practices. |

## 7. Major Deliverables

1. Approved project charter.
2. Requirements specification with traceability.
3. Testable acceptance criteria.
4. Relational database design, SQL schema, and entity-relationship diagram.
5. UX wireframes and a clickable MVP design.
6. Customer-facing MVP application.
7. Backend APIs and database implementation.
8. Test plan, test results, and defect summary.
9. Deployment and rollback plan.
10. User and operational guidance.

## 8. Assumptions

- No usable reference attachment was available when this baseline was prepared; therefore, the approved task description is treated as the primary source.
- The initial release serves one pilot city and uses one configurable currency.
- Restaurant price level is represented by an integer from 1 (lowest) to 4 (highest).
- Nearby search uses a default radius of 5 km and allows a maximum radius of 50 km.
- Customers may browse and build a cart without signing in, but they must sign in before submitting an order.
- A cart can contain items from only one restaurant at a time.
- The MVP supports pickup ordering. Payment occurs outside the application.
- Participating restaurants or an authorized data administrator provide and maintain menu, price, calorie, availability, and operating data.
- Calories may be unavailable for some items; the application must show this honestly instead of estimating a value.
- Ratings use a 1-to-5 scale. The displayed restaurant rating is the average of valid ratings.
- The initial MVP uses third-party location or geocoding services, subject to their availability and terms.

## 9. Constraints

- The MVP must be delivered within the approved eight-week schedule.
- Only the features identified as in scope may be included without formal change approval.
- Accuracy depends on data supplied by restaurants and external location services.
- Location access depends on customer permission and device/browser capability.
- The MVP has limited time and budget for integrations, performance testing, and platform coverage.
- Applicable privacy, consumer-protection, and food-information requirements must be reviewed before public launch.
- Payment processing and delivery logistics are not available in the MVP.

## 10. Major Risks and Mitigation Strategies

| ID | Risk | Probability | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- | --- |
| R-01 | Restaurant or menu data becomes outdated. | High | High | Record update times, validate data, provide controlled update procedures, and confirm availability again at checkout. | Product owner / Data administrator |
| R-02 | Customers deny location permission. | High | Medium | Provide clear consent text and a manual location-entry alternative. | UX lead |
| R-03 | External maps or geocoding service fails or exceeds limits. | Medium | High | Use timeouts, friendly errors, limited retries, caching where permitted, and service monitoring. | Technical lead |
| R-04 | Calorie information is missing or incorrect. | Medium | High | Require a source for supplied values, allow “Not available,” and do not calculate unsupported estimates. | Restaurant / Data administrator |
| R-05 | Price or availability changes during checkout. | Medium | High | Revalidate every item immediately before creating the order and ask the customer to review any change. | Backend lead |
| R-06 | A retry creates a duplicate order. | Medium | High | Use a unique idempotency key and an atomic database transaction. | Backend lead |
| R-07 | Personal or location data is exposed. | Low | Critical | Apply data minimization, TLS, password hashing, access control, secret management, audit logs, and security testing. | Security lead |
| R-08 | Nearby or filtered results are slow. | Medium | Medium | Add location and filter indexes, paginate results, set performance targets, and load test before release. | Technical lead |
| R-09 | Scope expands beyond the MVP. | High | Medium | Maintain an approved backlog and require product-owner approval for scope changes. | Project manager |
| R-10 | Customers misunderstand the price-level symbol. | Medium | Low | Explain the 1-to-4 price scale consistently in the interface. | UX lead |

## 11. Project Milestones

| Milestone | Target | Exit condition |
| --- | --- | --- |
| Project initiation | Week 1 | Charter, assumptions, roles, and MVP boundary approved. |
| Requirements baseline | Week 2 | Requirements, business rules, acceptance criteria, and traceability approved. |
| UX and technical design | Week 3 | Main user flows, API contracts, architecture, and database design reviewed. |
| Discovery and comparison build | Week 4 | Location, nearby search, rating sort, and price filters demonstrated. |
| Menu and cart build | Week 5 | Restaurant details, menu, calories, availability, and cart demonstrated. |
| Order build and integration | Week 6 | Authentication, checkout validation, order creation, and confirmation integrated. |
| System and user-acceptance testing | Week 7 | Critical scenarios pass and release-blocking defects are resolved. |
| MVP release | Week 8 | Release approval, deployment, monitoring, rollback plan, and handover completed. |

## 12. Success Criteria

The MVP is successful when all of the following are true:

- 100% of Must-priority acceptance criteria pass.
- There are no unresolved critical or high-severity defects at release.
- At least 95% of valid nearby searches return a response within 3 seconds under the agreed normal-load test.
- At least 95% of restaurant and menu pages load within 2 seconds under the agreed normal-load test.
- At least 99% of valid order-submission attempts create exactly one order and return a confirmation in integration testing.
- Rating sorting and price filtering return the correct order and result set in 100% of defined functional tests.
- All displayed prices, availability values, and calories match the latest approved test dataset.
- Customers who deny location access can successfully search using a manual location.
- Security and privacy checks confirm consent handling, encrypted transmission, access control, password hashing, and protection against common web vulnerabilities.
- A pilot usability test shows that at least 80% of participants can find a restaurant, select an item, and reach order confirmation without assistance.

## 13. Approval and Change Control

The project sponsor and product owner approve this charter and its MVP scope. Any requested change that affects scope, time, cost, privacy, architecture, or acceptance criteria must be documented, assessed, prioritized, and approved before implementation.
