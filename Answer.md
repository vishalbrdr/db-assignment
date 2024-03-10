# DB-ASSIGNMENT Solution

## 1. Explain the relationship between "Product" and "Product_Category" entities from the above diagram.

One Product - One Category
It is a one-to-one relationship between a product and its primary category.
Each product can have only one category assigned through this foreign key relationship.

## 2. How could you ensure that each product in the "Product" table has a valid category assigned to it?

There are two ways that will ensure that the valid category assigned to it.

1. Using a middleware before saving the saving the product which will check if the provided category_id exist in category table/collection and also check if the category has deleted_at property equal to null, which will ensure that the category id provided is a valid product_category.

2. using mongoose schema validation to define rules for the category id field. eg:
```
    const productSchema = new mongoose.Schema({
            // ... other product properties
        category_id: {
                type: mongoose.Schema.Types.ObjectId,
                ref: 'Category',
                required: [true, 'A product must have a valid category'],
                validate: {
                    validator: async function(value) {
                    const category = await mongoose.model('Category').findById(value);
                    return category ? true : false;
                },
                message: props => `${props.value} is not a valid category ID`,
            },
        },
        // ...
    });

```
## 3. Create schema in any Database script or any ORM (Object Relational Mapping).

Schema using Mongoose in javascirpt lang: [schema.js](./schema.js)