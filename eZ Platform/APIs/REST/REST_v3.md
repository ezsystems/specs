# REST API v3: content management

This specification defines the next iteration of the REST API in eZ Platform known as V3. The API aims to be backwards compatible but will also introduce new capabilities.

As a developer the overall vision of V3 is that the API should create simpler endpoints that can get me started quicker with the API. The endpoints will more default to values like language, draft, versions and misc options in order to make the threshold to use the API lower.

The overall vision is: simple by default and still making the advanced capabilities efficient.

The goal of the API defined here is to manage object fetch, create update and deletion. For read only and content heavy uses the GraphQL endpoints should be used.

*Question:* Should we drop XML support in the REST API? Other than specific field types like RichText there are no real needs to support XML. We could focus on V3 being just JSON for simplification.

## Typical use case

As a front-end developer I would like to:
* Define a content type in eZ Platform
* Manage the content via the REST API:
    * Use the REST endpoints to quickly create a new object
    * Use the REST endpoints to quickly update an already created object
    * Use the REST endpoint

# Creating content POST
The create content endpoint should be simplified to default most values and the simplest possible.

This call should create:
* A new content object (not a draft) and publish
* Assign ownership to the current user
* Create the object in the default language
* Location can be defined either with ID or URL

## Example: creating a new object

```
Resource: /content/object
Method: POST
{
    "content-location": "/blogs",
	"content-type": "blog_post",
	"fields": {
		"title": "My blog title",
		"author": "Bård Farstad",
		"body": "Body text, ideally simple but also with <b>formatting</b>. Perhaps supporting simple form and docbook.",
		"image": "/path/to/image.jpg"
		}
}

Return:
HTTP/1.1 201 Created
Location: /content/object/<newID>
```

# Fetching content GET

Fetching an object by location. Location can be either a location ID (42) or a relative url (/Blog/My_Post)

## Example: fetching an object

```
Resource: /content/object/(location_id)
Method: GET

Return:
HTTP/1.1 200 Success
{
    TODO: add default fields: id, published, modified, version, language etc.
    "content-location": "/blogs",
	"content-type": "blog_post",
    "content-type-name": "Blog Post",
	"fields": {
		"title": "My blog title",
		"author": "Bård Farstad",
		"body": "Body text, ideally simple but also with <b>formatting</b>. Perhaps supporting simple form and docbook.",
		"image": "/path/to/image.jpg"
		}
}
```
# Updating content PUT

The following call updates the complete object.

## Example: updating an object

```
Resource: /content/object/(location_id)
Method: PUT
{
	"fields": {
		"title": "My Updated title",
		"author": "Bård Farstad",
		"body": "Body text, ideally simple but also with <b>formatting</b>. Perhaps supporting simple form and docbook.",
		"image": "/path/to/image.jpg"
		}
}

Return:
HTTP/1.1 200 Success
HTTP/1.1 404 Not found

```
# Patching content PATCH

The following call patches an object and updates only the defined fields.

## Example: partially updating an object

```
Resource: /content/object/(location_id)
Method: PATCH
{
	"fields": {
		"title": "My second update to the title",
		}
}

Return:
HTTP/1.1 200 Success
HTTP/1.1 404 Not found

```
# Deleting content DELETE

The following call removes the current content item.

## Example: partially updating an object

```
Resource: /content/object/(location_id)
Method: DELETE


Return:
HTTP/1.1 200 Success
HTTP/1.1 404 Not found

```

## Field types

TODO: define the interface for the currently available field types to create and update/create.

TODO: define how to implement a new field type that has a simplified interface.
