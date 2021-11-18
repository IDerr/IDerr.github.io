---
author: IDerr
title: Week 2 - Improving OpenFoodFacts 
date: 2021-11-14
description: How to analyse and create an API easily for OpenFoodFacts ! 
image: "background.png"
categories: 
    - "Food"
    - "OpenFoodFacts"
tags:
    - OpenFoodFacts
---

## Introduction
In the last article, we have imported OpenFoodFacts data from a CSV format to PostgreSQL.

Now, our data is ready to be analysed by anyone.
SQL can be great for analysis but if we want to build an application, we'll need another transport layer like REST or GraphQL.
So in this article, I'll show you how create a quick GraphQL API using [Hasura](https://hasura.io/)

You can find all my work on my Github : https://github.com/IDerr/Openfoodfacts

## What is Hasura

Hasura is a GraphQL engine that can be connected to your database and result in a unified data access layer, here GraphQL.

If you're interested by the project, you can find a lot more informations on their website ! 

## Installation

Installation of hasura is pretty straight forward, I'll use Docker-compose to simplify this process.

Here's the docker-compose I've used to create my hasura engine
```
version: '3.6'
services:
  graphql-engine:
    image: hasura/graphql-engine:v2.0.10
    ports:
      - 8080:8080
    restart: always
    environment:
      ## postgres database to store Hasura metadata
      HASURA_GRAPHQL_METADATA_DATABASE_URL: postgresql://username:password@hostname:5432/mymetadatadatabase
      ## this env var can be used to add the above postgres database to Hasura as a data source. this can be removed/updated based on your needs
      PG_DATABASE_URL: postgresql://username:password@hostname:5432/openfoodfacts
      ## enable the console served by server
      HASURA_GRAPHQL_ENABLE_CONSOLE: "true" # set to "false" to disable console
      ## enable debugging mode. It is recommended to disable this in production
      HASURA_GRAPHQL_DEV_MODE: "true"
      HASURA_GRAPHQL_ENABLED_LOG_TYPES: startup, http-log, webhook-log, websocket-log, query-log
      ## uncomment next line to set an admin secret
      # HASURA_GRAPHQL_ADMIN_SECRET: myadminsecretkey
```

Now you run the tool easily with
```
docker-compose up -d 
```
![1.jpg](2.jpg)

TADAA, you know have a hasura engine ready, just type ``` http://youhostname:8080/console```
You'll be welcomed with a super UI.

You can now go to the "Data" section where you can setup the relationship between your database and Hasura.

![2.jpg](2.jpg)

Just track everything to link all tables !

Then it'll automatically detect all the links between tables, track them also.

![3.jpg](3.jpg)

After that, you can come back in the "API" section and your API is ready !

We can now easily create our GraphQL requests, for example all products that doesn't have a creator

```graphql
query GetProductsWithoutCreator {
  Product(where: {creator: {_is_null: true}}){
    code
    creator
    product_name
  }
}
```
![4.jpg](4.jpg)

Oh, we have found 4 products without any creator, weird...
We can now report that to the OpenFood facts team :)

A lot can be done with that, and if tomorrow we want to build an Android app, our API will be ready to go !

## Next steps
Next steps, develop a lot more graphql queries to find inconsitent data, and improve the model.
I'll focus on that for next week ! 

Also, that we have our API, we can try to make a simple app to try the integration with Hasura.

## Conclusion

I'll continue working on my side, if anyone would like to help me on this project, feel free to create issues and PR on the github repository.
I am also available on Openfoodfacts Slack (IDerr), if there's any questions!
 
See you next week for the next article (on OpenFoodFacts)! 