# GraphQL interface in eZ Platform

As a developer I would like to query the content repository using the GraphQL language.

Supported methods:
* GET: get interface is supported and is managing caching via HTTP caching using disk or edge (Varnish)
* POST: post interface should be supported by needs to rely on optimized persistence caching in eZ Platform to perform

The Graph API should be read only and will not support content manipulation.

## Endpoints

Aa developer I should have two endpoints available for querying the content repository. One GET /content/graph and one POST /content/graph.

The location for the content should be specified by the URL, but alternatively a location ID can be used.

### GET GraphQL

```
GET /content/graph/
/blog?
fields=blog_posts.limit(10){title,body,photo}

Return:

```


### POST GraphQL

```
Resource: /content/graph/
Method: POST
{
    location: "/blog" {
            fields: name, description
            children{

            }
    }
}
Return:

```
