# Query Language API Prototype Requirements

# Objective

"As an eZ Engineer, I want...

To create a functioning, experimental implementation of a query language (GraphQL) to retrieve deep data with one definition (query) using the REST API, the PHP API and or the controller view definitions. The purpose of the research of the prove that we can retrieve the content from eZ using this query language as well as to have the foundation to specify the details of the syntax that will be used in REST, PHP and views."

Underlying goals are:
* Reduce the number of calls when making *read only* REST based clients Apps connecting to eZ
* Lower the threshold for developers to retrieve content from eZ
* Unify the way we retrieve deep and specific content structures from eZ via REST, PHP and views
* Enable front-end developers to retrieve all relevant content for Twig templates with only using configuration files (no need for PHP)

# Firm
The prototype *must*:
* Demonstrate queries to the content repository using GraphQL via REST
    * Demonstrate REST GET
    * Demonstrate REST POST
* Demonstrate queries to the content repository using GraphQL via PHP API
* Demonstrate queries to the content repository using GraphQL via view definitions for Twig templates
    * Demonstrate how we could use YAML syntax to replicate a GraphQL query for views
* Demonstrate how we can use path strings and identifiers instead of IDs to define details in the query.

# Preferred
Prototype *should*:
* Demonstrate how caching can be implemented in the following layers:
    * REST GET
    * Persistence caching (which will speed up POST, PHP and views )
* Provide example queries for different scenarios of content fetching like:
    * Fetching current user with sub objects
* Provide an example of how the API documentation structure should look like
* Provide details of the performance impact of the different implementations and how latency and database queries could/should be minimized in development of the features

# Bonus
Prototype may:
* Be written in such a way that it can be directly re-used in implementation
