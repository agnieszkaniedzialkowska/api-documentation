---
title: Showcase it: Products, variants, sizes and displays
altTitle: Product model
excerpt: Learn about Products, Variants, Sizes, Displays, Attributes, Relations, Bundles and Categories.
taxonomy:
    category: docs
---

Centra uses a predefined product model that is optimized to solve the complexities of fashion and lifestyle products. The structure cannot be changed by users, however users can switch some functionalities on and off and define their own attributes.

Two different concepts for modelling and storing items for sale are used in Centra: Displays and Products. This includes Variants and Sizes of Products.

![ProductModel](product-model.png)

### Products, Variants and Sizes

A Product is stored in a hierarchical structure, in which the Product is broken down into Variants and Sizes. The Product represents a certain design. Variants are different versions of the same product, typically different colors. Sizes are the different sizes you can buy the style in. This product structure enables very efficient product information management.

Most information is stored on the Product level, where you find the Folder, Collection and all basic information such as Country of Origin, HS Code and Material Composition.

The Variant level focuses on media (images) and a few attributes, notably color. Campaigns can be set on the Variant level. Besides actual size, the Size level stores GTINs (UPC or EAN codes). Stock inventory is stored on the Size level, as the size represents the physical item that exists in a warehouse.

All products have at least one Variant and all Variants have at least one Size, even if it is a one-size product.

### Displays

A Display is, just like a display in a brick-and-mortar store, one or more related items for sale that is arranged and presented in a way that might trigger a purchase. An item for sale might be included in more than one Display. You might for example have a unisex item included both in a Display in the Women’s and Men’s department. When you query the API for what to show on a category page, you will get a set of Displays back, and when you query the API for what to show on a product page, you will get a single Display.

You can also activate many variants on a single Display, for example all variants of the same Product can share a Display. If you do that, Centra APIs will return the first Variant activated in the Display, and return all other activated Variants as API products related to the first one. The type of relation is configurable.

Displays can also be related to each other, which can be used in the front end to show off different variants of the same product, or suggest products that might go well together. Relations have no special logic in Centra, they are technically only simple attributes pointing one-way to another Display, they mostly affect how Products are returned in the APIs.

### Attributes

Displays carry attributes set on all three different levels in the Product model as well as attributes set on the Display itself.

Centra has three Attribute Categories: Standard, Pre-Defined and Custom.

| Attribute Category | Description | Examples |
|--------------------|-------------|----------|
| Standard | Default attributes, not possible for user to disable | Display Name (Display), GTIN (Item), HS Code (Product) |
| Pre-defined | Pre-configured attributes, possible for user to turn on or off | Age limit (Product), Color (Variant) |
| Custom | Custom attributes, defined by user | N/A (user defined) |

### Relations

Displays can be related to each other in 3 different ways. All Display relationships are one-way, a two-way mutual relationship between 2 displays are created by 2 one-way relationships in opposite directions.

| Relationship | Description |
|--------------|-------------|
| Variant | The related Display represents a different Variant of the Same Product (e.g. a different color of the same style. The linked Variant could also come from a different Product, e.g., if the Variant is made out of a different material. The expected frontend behavior is to provide a link to the related Variant, typically using a color or pattern swatch. |
| Size | The related Display represents a different Product in a different Size, following a different business logic than the fashion business logic built into Centra. E.g., a bag in a different size or a cosmetic product in a different size. The expected frontend behavior is to provide a link to the related Size, typically under a “different sizes” heading. |
| Standard | The related Display represents a related Product. The expected frontend behavior is to provide a link to the related Display, typically under a heading such as “Related products”. |

It is possible to configure custom types of relationships between Displays.

### Bundles

Bundles are available in both the Direct-To-Consumer and Wholesale modules of Centra, and work in the same way.

A bundle is a collection of 2 or more separate items sold together as a package. In the warehouse, the items exist as separate items, but in the store they are sold together, so the customer adds them to the cart as one item and pays a price for the bundle rather than for individual products.

The price of a bundle can be generated by Centra as the sum of the price of the bundle’s components (Dynamic pricing), or set directly by the Centra administrator (Static pricing). Regardless of how the price was set, it's always returned by the webshop APIs in the same way.

Please see [Adding a bundle](https://support.centra.com/centra-sections/general/catalog/adding-a-bundle) article in our support portal to read about specific examples of bundles.

#### Fixed bundles

Fixed bundles consist of a pre-defined number of sections, in each of which the customer can select one of pre-selected product variants. For example, if you're selling a pajamas as a bundle, it will consist of two sections, top and bottom. In the first section, you should add all product variants which are pajamas tops, and in the second - all available bottoms. The customer will be able to select one product in each section, and therefore build their custom pajamas bundle.

#### Flexible bundles

Flexible bundles can consist of different numbers of sections. For example, a bundle can be a custom set of dinner dishes - a selectable soup vase, two bowls and any number of dinner plates - for 1, 2 or 4 persons, each separately selectable. Because the number of sections is not absolute, these are not possible to be defined as a fixed bundle.

[notice-box=info]
Implementing flexible bundles is technically more difficult, and requires custom adjustments to your front end to support them. They should also be enabled in the Checkout API plugin configuration. If you're interested in implementing flexible bundles, please contact Centra Success team.
[/notice-box]

#### Pre-packs

A pre-pack refers to a set of items packaged together and sold as one item. A pre-pack is stored in the warehouse as one pickable item. It is therefore neither an implicit nor an explicit bundle and should be set up in Centra as a normal product and will appear through the Store API as a normal product. The attribute multipack should be used if the pre-pack only contains identical items.

#### Buy X, pay for Y

A discount logic that allows a customer to buy a number of products (with certain restrictions) but pay for fewer (e.g., buy 3, pay for 2) is not a bundle, it is a [Voucher](/overview/promo#voucher).

### Categories and Folders

Centra handles the similar concepts, Category and Folder, differently. A Display belongs to a Category. Categories are hierarchical in up to 3 levels. The expected behavior of a Centra store frontend is to allow the user to navigate the Category tree to discover products. The Category structure can be expected to change dynamically. It is safe to assume there will never be more than 3 category levels, but there are no limitations when it comes to the number of categories.

A Product belongs to one and only one Folder. Folders are used for reporting and should not be exposed on the frontend site. As Categories, Folders are also hierarchical with up to 3 levels. Even though Folders are not expected to be exposed on the website, they are available through the Store API to facilitate development of custom applications.

## How should I set up Product structure in my Centra?

This is a design decision you should carefully consider when onboarding with Centra. It may differ depending on the type of products you are selling and your business model. If you're selling clothes, your product structure will be different than if you're selling jewelery. 

| | Shirts | Phone cases v1 | Phone cases v2 |
|---|---|---|---|
| Product | Specific style or cut. Similar shirt made of different material is probably a different product. | Specific design and material of a case. Same design cases made of plastic and silicon could be different products. | Case made for a specific phone model. |
| Variant | Different colors of the same style. | Same design and material cases made for a different phone model. Amount of material will differ, but thanks to the margins, the prices can be the same for all variants. | Different designs cases for the same case model, including different materials. They can have same or different prices. |
| Size | Different sizes of the same color. Same price for each. | Each variant should only have one size. | Each variant should only have one size. |
| Display | You can display each variant separately, or create a single display for unisex products. | You can set up a single display for a specific case design and activate it for all phone model variants. | You can set up a single display for all designs variants made for the same phone model. |

| | Jewelry | Bags | Cosmetics |
|---|---|---|---|
| Product | Specific ring design or head stone. Same ring with a different head is a different product. Same model of ring made of silver and gold are separate products. | Specific model and design of a bag. | Specific type of cosmetic (cream, shampoo, perfumes or other). |
| Variant | Different sizes of the same ring. Price depends on the amount of material used. | Different sizes of the same bag (big, medium, carry-on). Price depends on the size. | Different amounts of the same cosmetic. 100 ml bottle has higher price than 50 ml one. |
| Size | Each variant should only have one size. | Each variant should only have one size. | Each variant should only have one size. |
| Display | Separate displays for each variant, or a single display for all variants of the same ring. | Same display for all sizes of specific model, or separate displays for each with relations set up between displays. | One display for all versions of the same cosmetic product, with all size variants added. |

#### Product

Product level defines the basic details of the good for sale. Each version of a product has common name, brand and SKU (Stock Keeping Unit), it belongs to a specific collection and is obtained from specific suppliers.

#### Variant

Variants are different variations of the same product. Depending on the product type, it can be split into variants based on such criteria as:
* Colors: Standard distinction when dealing with fashion. For example, red shirt and a blue shirt are different variant of the same product, they have the same price and are likely made of the same material.
* Sizes: In fashion, all sizes of the same product usually have the same price. This is not the case if you're selling suitcases or rings, where the price depends on the amount of material used for production.
* Minor differences: The same color shirt can be offered with different style of collar or cuffs. When they are defined as separate variants, they can be set with different prices, description or promotions.

Variants can also be a combination of criteria. For example, if you were selling bags that came in multiple colors and sizes, you would want to create a variant for each combination.

#### Size

Same variant of the product can be sold in different sizes, for example Red Shirt size XL and Red Shirt size M. This is mostly meant for fashion, as the price is defined on a variant level, and therefore different sizes cannot be priced differently. If you're selling different sized silver rings, you will probably want to set them up as different variants in order to make the bigger ones more expensive.
