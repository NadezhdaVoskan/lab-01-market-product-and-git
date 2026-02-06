## Product Choice

- Wildberries.ru
- <https://www.wildberries.ru/>
- Wildberries (также «Вайлдберриз») — российский маркетплейс. Специализируется на продаже товаров различных категорий, сотрудничает с производителями и дистрибьюторами.
  
## Main components

![Telegram Component Diagram](./diagrams/out/wildberries/architecture-component/Component%20Diagram.svg)

![Telegram Component Diagram code](./diagrams/src/wildberries/component-diagram.puml)

#### Auth & ID Service

This service is responsible for user authentication and authorization, ensuring secure login and access control across the platform.

#### Cart & Checkout Service

This component manages the user’s shopping cart and the checkout process, including adding/removing items and preparing orders for payment.

#### Catalog & Search Service

It stores and provides product information and enables users to search for products using filters, categories, and search queries.

#### Notification Service

This service sends notifications to users, such as order confirmations, delivery updates, and promotional messages, via email, SMS, or push notifications

#### Order Management Service (OMS)

This component handles the lifecycle of orders, including order creation, status updates, and coordination with payment and logistics services.

#### Fintech & Payment Service

It processes payments, refunds, and financial transactions, integrating with external payment providers and ensuring transaction security.

## Data flow

![Telegram Component Diagram](./diagrams/out/wildberries/architecture-sequence/Sequence%20Diagram.svg)

![Telegram Component Diagram code](./diagrams/src/wildberries/component-sequence.puml)

### Flow: 2. Checkout (Reservation & Payment)

### Flow: Checkout (Reservation & Payment)

In this flow, the user completes the checkout and starts the payment process.  
The WB Mobile App sends a request with `cart_id` and `payment_method` to the Storefront Gateway, which forwards it to the Order Management Service (OMS).

OMS creates a new order and asks the Inventory Service to reserve products from the cart. Inventory Service updates the stock in the database and returns a reservation confirmation. The order is then saved in the database with status `PENDING_PAYMENT`.

After that, OMS calls the Payment Service to start the payment. Payment Service sends a request to the bank and receives a 3DS payment URL. This URL is returned back to the mobile app, where the user confirms the payment.

## Deployment

![Telegram Component Diagram](./diagrams/out/wildberries/architecture-deployment/Deployment%20Diagram.svg)

![Telegram Component Diagram code](./diagrams/src/wildberries/component-deployment.puml)

Client applications run on user devices.  
Gateways and backend services are deployed in the cloud.  
Databases and caches are deployed in separate data storage systems.  
External services (banks) are outside the platform and are accessed via APIs.

## Assumptions

- I assume OMS manages the full order lifecycle.
- I assume stock is reserved during checkout.
- I assume payments are processed via external banks.

## Open questions

- How does Wildberries handle high traffic during big sales events?
- How are stock reservations and payments synchronized in failure scenarios?
