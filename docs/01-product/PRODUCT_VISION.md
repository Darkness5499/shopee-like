# Product Vision

- Status: Baseline — retained from the existing project documents; formal sign-off is not recorded.
- Owner: Project owner.

## Product goal

Build a multi-category marketplace demo inspired by Shopee. It will model complex, end-to-end e-commerce business flows and serve as a hands-on project for learning software delivery and solution architecture.

## Intended outcome

The demo must allow a seller to create products and stock, an internal team to moderate listings, and a buyer to discover products, place an order, pay through a simulated provider, receive shipment updates, and complete or dispute the transaction.

## Learning goal

Practice the full lifecycle: requirements analysis, architecture/design, implementation, testing, deployment, observability, and iterative improvement.

## Product boundary

The product is a learning demo, not a production marketplace. It does not need real customers, real money movement, or production-scale traffic.

## Success criteria

- A Seller (a User acting as a Shop operator) can create a product with SKU-level price and inventory.
- An Internal Staff member can approve or reject a submitted product.
- A Buyer can find an active product, purchase it, and track the resulting order.
- Simulated payment and logistics partners can update the order through asynchronous callbacks/events.
- The implementation has documented requirements, architecture decisions, tests, deployment, and basic observability.

## Explicit exclusions for the initial scope

- Real payment-provider integration.
- Real logistics-carrier integration.
- Live commerce, chat, recommendation AI, and production-grade marketing features.
- Production-level scale, legal compliance, and operational support.
