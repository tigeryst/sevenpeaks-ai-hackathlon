# Search within a website #

## References links:

 * [Study Case #1](https://github.com/Azure-Samples/azure-search-comparison-tool/blob/main/README.md)
   
    * [Demo](https://aka.ms/VectorSearchDemo/)
    * [Sample Data](https://github.com/Azure-Samples/azure-search-comparison-tool/tree/main/data)

 * [Study Case #2](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/apps/ecommerce-search)

    * [Demo](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs/)
    * [NYC Sample Data](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs/blob/master/NYCJobsWeb/Schema_and_Data/README.md)

---
## Lab #1

### Introduction

Your website `my-website.company-of-mine.co.th` has reviewed by an internal group of data and business analysts. According to their report, several issues have emerged:

 * The incremental sales growth of the last 6 months has resulted in a catalog of products so extensive that users drop their sessions after a few clicks without reaching any product pages.
 * The level of friction within the current UI is too high, implying that a user needs to navigate through 5-6 pages before reaching the desired result.

In order not to impede sales growth and avoid business disruption, the developer team came up with the idea to create a search experience more powerful than the current one, improving the search method and a new recommendation engine for the users.

### Goals

So, a new search system should be created structuring a new micro-service. The frontend application will use the new search functionalities using a new set of REST API; the new search system have to follow these requirements:

 * Define the product models to search, with all the fields and features that should be provided by the new system
 * Define the feature of search, planning the fields indexing and search modes
 * Provision the database with your catalog
 * Use Azure Search service organise all the search operations
 * Expose an OpenAPI interface (Swagger) as documentation for usage by Front-End team.

### Product domain structure

Here an example of product structure that should be extended for the scope of the exercise.

```json
{
  "product_id": "...",
  "name": "Example Product",
  "description": "This is a sample product description.",
  "price": 29.99,
  "currency": "USD",
  "category": "Electronics",
  "brand": "Example Brand",
  "stock_quantity": 100,
  "images": [
    "image_url_1.jpg",
    "image_url_2.jpg"
  ],
  "ratings": {
    "average": 4.5,
    "count": 120
  },
  "features": [
    "Feature 1",
    "Feature 2",
    "Feature 3"
  ]
}
```

### API description sample

Here an example of the API endpoint using OpenAPI syntax that should be provided, as before this piece of code should be extende for the scope of the exercise.

```yaml
openapi: 3.0.0
info:
  title: E-commerce B2C API
  version: 1.0.0
paths:
  /products:
    get:
      summary: Search the catalog based on product parameters
      parameters:
        - name: name
          in: query
          description: Name of the product to search for
          schema:
            type: string
        - name: category
          in: query
          description: Category of the product to search for
          schema:
            type: string
        - name: brand
          in: query
          description: Brand of the product to search for
          schema:
            type: string
        - name: min_price
          in: query
          description: Minimum price of the product
          schema:
            type: number
        - name: max_price
          in: query
          description: Maximum price of the product
          schema:
            type: number
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  products:
                    type: array
                    items:
                      $ref: '#/components/schemas/Product'

        '400':
          description: Bad request
          content:
            application/json:
              schema:
                type: object
                properties:
                  error:
                    type: string
                example:
                  error: "Invalid parameters"

        '404':
          description: No products found
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
                example:
                  message: "No products found"

      components:
        schemas:
          Product:
            type: object
            properties:
              product_id:
                type: string
                ...
```

Useful links:

 1. Sample #1 of product catalog as [JSON list](./sample_products_catalog-01.json)
 1. Sample #2 of product catalog as [JSON list](./sample_products_catalog-02.json)

---

## Lab #2

The recommendation engine can be based on three properties of the product:

 * **Rating**: This represents the 'User's likes' and can be viewed as a 'Most Popular' criterion.
 * **Stock**: If a product has a stock level near a specific threshold, it is considered a candidate for recommendation.
 * **Discount**: As part of a marketing strategy, various products may or may not have discounts throughout the year.

So, the team should be able to:

 * create one or more `scoringProfile` for the recommendation criteria, defining the proper criteria of "boosting"
 * modify the existing business logic of the search based on this recommendation engine

As testing scope only, please provide a new API endpoint that will implement this logic in order to create a comparison between the endpoints.

 * [Scoring profile - Official Documentation](https://learn.microsoft.com/en-us/azure/search/index-add-scoring-profiles)
 * [Scoring profile - Using weighted field](https://learn.microsoft.com/en-us/azure/search/index-add-scoring-profiles#using-weighted-fields)
 * [Scoring profile - Using functions](https://learn.microsoft.com/en-us/azure/search/index-add-scoring-profiles#using-functions)