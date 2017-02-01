# eZ Views Content Query

As a developer I would like to provide a set of predefined content to my defined views with just configuration.

## Typical use case

As an developer/information architect I
* Create a new content type in the admin interface
* Enter some content in the admin interface for testing
* Use the views config file (e.g. views.yml) to define the available views and which associated Twig templates to use for rendering
* Use the views config to define the content that should be available in the view
* Have one, or more, variables available in the Twig template that contain all the content I defined

## Definition of views
As a developer I need to be able to extend the definition of the views in eZ Platform to include a content section. The content section includes a query language (probably inspired by GraphQL) to enable the definition of deep content to be available directly in the view.

###Example configuration
The following example renders a full view for an article using the templte article.html.twig. The template is instantiated with the following content:
* content: Article
    * fields: id, name, intro and image
    * Sub locations (children) ordered by publihsing date in descending order
        * Comment children has the fields: id, user, title and comment available
        * Gallery children has the fields: id and name
            * Images in the gallery with the fields: name and picture

### YAML config example

```
article:
    template: full\article.html.twig
    match:
        Identifier\ContentType: article
    content:
        node: "@=location.id"
            fields: id,name,intro,image
            children: order(published,desc)
                comment:
                    fields: id,user,title,comment
                gallery:
                    fields: id,name
                    children: order(priority,asc)
                        image:
                            fields: name,picture
```

*TODO*
    * Define pagination (cursors or offset/limit)?

### TWIG Template example
The example above is the full view template using parts of the content defined above.

The example renders the children using the line view template.

*NOTE:* Some concepts needs to be looked into when rendering children as the definition here defines which fields are available. The engine should throw errors when trying to fetch fields that are not available for the given view and NOT try to fetch from the repository.

```
<div class="content-title">
  <h2>{{ ez_content_name(content) }}</h2>
</div>
<div class="article-intro">{{ ez_render_field(content, 'intro') }}</div>

<div>
  {% for child in content.children %}
       {{ render_esi( controller( "ez_content:viewAction", {"content": child, "viewType": "line"} ) ) }}
  {% endfor %}
</div>
```
