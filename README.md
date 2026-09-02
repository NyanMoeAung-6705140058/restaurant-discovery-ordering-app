# Restaurant Discovery & Ordering App

This repository contains the MVP project documentation for a Restaurant Discovery & Ordering App. The application helps customers discover nearby restaurants, compare restaurants by rating and price, view menu and calorie information, select food, and place a pickup order.

## MVP Features

1. Discover restaurants near the customer's current or manually entered location.
2. View and sort restaurants by customer rating.
3. View and filter restaurants by price level.
4. Select a restaurant and browse its menu by category.
5. View the price, availability, and disclosed calories for each menu item.
6. Add menu items to a cart and submit a pickup order.

## Project Documentation

| Document | Description |
| --- | --- |
| [Project Charter](01-project-charter.md) | Defines the project's background, purpose, objectives, scope, stakeholders, risks, milestones, and success criteria. |
| [Requirements Specification](02-requirements-specification.md) | Defines user roles, user stories, functional and non-functional requirements, business rules, interfaces, security, privacy, and traceability. |
| [Acceptance Criteria](03-acceptance-criteria.md) | Provides testable positive, negative, boundary, and error-handling scenarios for every MVP feature. |
| [Database Design](04-database-design.md) | Defines the relational schema, entities, relationships, constraints, indexes, sample records, PostgreSQL statements, and Mermaid ER diagram. |

## Main MVP Assumptions

- The initial release supports one pilot city and one configurable currency.
- Nearby search uses a default radius of 5 km and a maximum radius of 50 km.
- Customers may browse and build a cart without signing in but must sign in before submitting an order.
- A cart can contain items from only one restaurant location.
- The MVP supports pickup ordering; payment and delivery tracking are outside scope.
- Calories are shown when supplied. Missing values are displayed as **Not available** and are not estimated.
- Restaurant price levels use values 1 through 4, and ratings use a 1-to-5 scale.

## Database Target

The database design targets PostgreSQL 15 or later. PostGIS is recommended for accurate, indexed geographic-radius searches in a production environment.

## Repository Structure

| File | Format |
| --- | --- |
| `README.md` | Repository overview |
| `01-project-charter.md` | Project charter |
| `02-requirements-specification.md` | Software requirements specification |
| `03-acceptance-criteria.md` | MVP acceptance tests |
| `04-database-design.md` | Database design and SQL schema |

## Viewing the Documentation

GitHub renders each Markdown file automatically. Select a document from the table above to read it. To work locally, clone or download the repository and open the files in any Markdown editor.

## Project Status

The documentation represents the approved MVP baseline and is ready for stakeholder review, technical design, development, and testing.
