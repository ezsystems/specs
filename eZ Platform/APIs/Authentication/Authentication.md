# Authentication specification

# Objective
"As an eZ Engineer, want...

To have a simple interface to add new authentication strategies, create a local eZ user instantiated remotely and keeping persistence via tokens. The purpose of having a mechanism for pluggable authentication strategies is to enable simple integration points for authentication with external systems and increase adoption by developers creating an integrated system."

Underlying goals are:

* Simplify the creation of third party authentication strategies supporting the likes of:
    * Facebook
    * Google
    * LinkedIn
    * LDAP
* Simplify how to create a new user account in eZ via the API regardless of the authentication strategy being used
* Providing a plugin based persistence with sessions and tokens
    * Should support OAuth2 bearer tokens, but should be pluggable to support others like JWT (JSON Web Token)
* Adding native support for OAuth2 bearer tokens via the REST API by default

## Relevant reading

For inspiration on other frameworks implementing similar you can take a look at:
http://passportjs.org/

# Firm
The prototype *must*:
* Provide a pluggable system for remote, and internal, user authentication strategies
* Provide a generic interface for CRUD a user account in eZ regardless of the authentication strategy being used
* Providing a pluggable system for persistence of Authentication via:
    * Sessions
    * OAuth2 Bearer Token
    * JWT Token
* Add native support for OAuth2 when using the REST API as an altenative to username/password and session persistence

# Preferred

The prototype *should*:
* Implement the most authentication strategies like:
    * Facebook
    * LDAP
    * Google
    * LinkedIN

# Bonus

The prototype *may*:
