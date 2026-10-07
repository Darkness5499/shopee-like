# Stakeholders and Actors

- Status: Baseline — retained from the existing project documents; formal sign-off is not recorded.
- Owner: Project owner.

## Product stakeholder

- **Project owner:** defines learning objectives, validates scope, and approves product decisions.

## System actors

- **Buyer:** a User acting in the purchasing role; browses, purchases, pays, tracks orders, reviews, and requests support.
- **Seller / Shop Operator:** a User authorized to operate one or more Shops, manage products/SKUs/inventory, and fulfill orders. A Shop is a business entity, not an actor or an account.
- **Internal Staff:** includes Admin, Moderator, Support, and Operations permissions.
- **Payment Provider:** external simulated system that reports payment outcomes through callbacks/webhooks.
- **Logistics / Delivery Partner:** external simulated system that receives shipment requests and publishes tracking events.

## Actor rule

One User account can act as a Buyer and can also own or operate one or more Shops.

Internal Staff permissions are distinct from Buyer and Seller responsibilities. Whether they share an account type, and the exact shop-staff permission matrix, remain requirements questions; see [OQ-005](../02-requirements/OPEN_DECISIONS.md#oq-005).

Canonical terms are defined in the [glossary](../02-requirements/GLOSSARY.md).
