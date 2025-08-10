# **Unify Your Data: Netflix UDA in simple terms**

UDA is a way to write your business model once and have the system generate the right schemas and pipes for every place you need them, like GraphQL for apps, Avro for data movement, and Iceberg for analytics. It keeps everything tied back to the same concepts in a knowledge graph, so names and meanings stay aligned across teams. ([Netflix Tech Blog][1])

## **What UDA is**

* A **knowledge graph** holds your core business entities and relationships.
* From that model, UDA **projects** concrete things teams actually use: GraphQL schemas, Avro contracts, Iceberg table schemas, and even code and pipelines.
* It is **domain first**. Each domain models what it owns. You reuse where it helps, without forcing a single global schema. 

## **How it works under the hood**

* **Upper (the metamodel)**: the language used to author domain models. Upper plus the graph is the “brain” that knows what things mean and how to project them. 
* **Projections**: generators that turn the model into GraphQL, Avro, and Iceberg artifacts, along with the wiring to move and serve data. 
* **Fit with Netflix’s stack**: those projections plug into the existing GraphQL platform for apps and Iceberg in the lakehouse, and ride Netflix’s Data Mesh for movement. 


# **How someone would use a “UDA engine” day to day**

### 1) Model your domain once

* Define entities like **Title**, **Person**, **Role**, with keys and relationships.
* Validate and publish the model to the knowledge graph.
* Result: a single source of truth that tools can discover and reuse. 

### 2) Generate the things you need

* **App team**: pull the projected **GraphQL** schema, scaffold a Domain Graph Service, and federate it into the company gateway. 
* **Data movement**: use the projected **Avro** schema to stand up a Data Mesh source or stream.
* **Analytics**: create **Iceberg** tables that match the same concepts and are ready for SQL.

### 3) Keep meaning connected

* Because all of these artifacts came from the same model, you can trace a field in GraphQL to a column in Iceberg or a message in Avro and know it means the same thing. That reduces drift and speeds up changes. 

# **Why this helps**

* **Consistency with less manual work**: fewer hand written translations between app, stream, and lake. 
* **Faster delivery**: new features land once in the model, then flow into the systems you use.
* **Discovery and reuse**: people can search for concepts rather than guess table names and service fields. 


# **If you want to build something similar**

Below is a practical blueprint you can adapt to your stack.

### A. Modeling and storage

* Pick a **modeling format** that is strict and versioned. Options: RDF plus SHACL, or a typed schema language that you control. UDA uses Upper to define the “model of models.” 
* Store models in a **graph or metadata store** with APIs for query, diff, and lineage.

### B. Projections

* Write generators that take the domain model and emit:

  * **GraphQL** types and resolvers skeletons for your services. 
  * **Avro** or **Protobuf** schemas for events or movement through your data platform.
  * **Iceberg** table schemas and partition specs for analytics.
* Add small, system specific hints in the model so projections can handle things like partitioning, indexing, and retention.

### C. Mapping and lineage

* Keep **mappings** between model fields and physical containers in the same store. That lets you walk concept to column and back. It also makes changes safer and easier to audit. (Netflix discusses storing these relationships in the graph so they are queryable and versioned.) 

### D. CI and governance

* Run **model validation** and **projection checks** in CI.
* Enforce **semantic versioning** for models and contract change rules for downstream consumers.

### E. Adoption path

1. Start with one domain and two projections, for example GraphQL for apps and Iceberg for analytics.
2. Onboard one data flow through your movement layer using Avro.
3. Prove that analysts and service teams can find and reuse the same concepts end to end.
4. Expand to more domains and projections.

# **Caveats to plan for**
* You still need discipline around **change management** and versioning of models.
* Projections must respect each system’s performance needs, like partition design for Iceberg or resolver boundaries in GraphQL.

## **Bottom line**

UDA is a smart pattern: model once, project everywhere, keep meaning connected. If you have GraphQL for apps, a movement layer like Data Mesh, and Iceberg or another table format in analytics, you can build a lean “UDA engine” that automates a lot of the glue work and cuts schema drift across the company. 

[1]: https://netflixtechblog.com/uda-unified-data-architecture-6a6aee261d8d "Model Once, Represent Everywhere: UDA"
