# Content editing


# Objective

"As an eZ Engineer, want...

To create a functional, experimental implementation of a refactoring of the Field Type API in eZ Platform supporting full CRUD (create, read, update and delete) for content creation and editing. The purpose of the research is to find a common way to define the user interface using different technologies."

Underlying goals are:

* Simplify how to create new custom Field Type
* Reduce errors by limiting the number of places UI definitions are done
* Increase developer velocity when developing and maintaining Field Types

# Firm

The prototype *must* for the following interfaces:
* Twig based content editing
* A remote website (e.g. Angular)
* An App using REST API (e.g.React Native)

provide examples that:

* Demonstrate how we can define input validation
* Demonstrate how we can define input UI commonly
    * Preferrably ending with auto generating on the different platforms a working default
* Demonstrate how we can override UI components where platform specific implementation would be more desirable


# Preffered

The prototype *should*:
* Demonstrate how Symfony forms (repo-forms) can be used
* Identify logic that should be moved to the API
* Demonstrate how we can refactor how to store data using native or custom (e.g. MongoDB or remote AWS RDS + S3)
* Demonstrate how to make content searchable (e.g. how to define facets and sub-structures for search)
* Demonstrate how to display content of the Field Type for a preview or example rendering


# Bonus

The prototype *may*:

* Demonstrate how we could simplify our API for adding a new Field Type today
