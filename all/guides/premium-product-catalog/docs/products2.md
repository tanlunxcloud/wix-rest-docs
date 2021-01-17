SortOrder: 1
## Products

A product is a salable unit, containing a complete definition for creating and managing a subscription, including: features, pricings, coupons, allowed actions, renewal settings. Display settings will also be indirectly associated with a product.
An instance of a product is called a "subscription", and is created as part of a purchase flow.
A product consists of the following:

*   Features
    
    independent entitlements that will be delivered to the user upon purchase. each payment cycle may entitle the user to a different set of features.
    for more see "Features" section.
    
*   Pricing (_currently managed by SBS_)

    payment incurred on user when purchasing this product, and during recurring payments (for recurring subscriptions).
    supports different prices for different currencies and cycles.
    
*   Name (_currently managed by SBS_)

*   Type and family

    used to group and associate products with different subscription management settings: upgrade path, cancellation policies, etc.
    this information enables "Subscription Manager" service to provide different lifecycle management functionality: send e-mails, request cancellation, assign to site, etc.
    subscriptions of the same _product type_ are sold by the same "vendor", and may affect each other when connected to a site (e.g., for some types there may only be 1 subscription allowed per site).
    subscriptions of the same _family_ are expected to be variants of the same product (e.g., share the same name. usually different pricing or minor feature changes).
     
#### Product lifecycle

A product is initially created in draft mode. a product in draft mode can be modified, but cannot be offered to users for purchase.
Once a product is _published_, it can be included in an offer - and therefore be sold to users, but its core properties can no longer be modified. Only backoffice meta-data and family classification may be changed.

To change the features/pricing/names of an existing subscription generated by a product, one must explicitly switch the contract of a subscription to a different product with the desired properties.
this occurs naturally during upgrade/downgrade of a product, but may also be forced manually by a migration. for more see "Subscriptions Manager".

Finally, to disallow new purchases of a product, it can be _archived_. A product must not participate in any active offering when it is archived (pending "Dealer" integration).
Archiving a product does _not_ affect existing subscriptions.