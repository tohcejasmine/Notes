# Objectives
1. Learn what are microservices
2. Learn what are RESTful APIs
3. Learn what is Flask
4. Build a simple Flask application
    * Link application to a database (MongoDB/SQL)
    * Set authenticated login (OAuth)
    * Allow upload of different media (photos, files)
5. Learn to test APIs using Postman
6. Learn how to write API documentation

# Microservices

* A software development architecture
* Focus on building **single-function** modules with **well-defined** interfaces and operations
* Key considerations: **designing, testing, monitoring**
* An application as a suite of small services, each running in its own process and independently deployable (_decentralised governance_)
* Each service
  * May be written in a different programming language
  * May use a different data storage technique
* Benefits
  * **Easier to deploy**: can be deployed independently, will not affect other services
  * **Easier to understand**: more accessible and easier to code, functions are isolated and less dependent
  * **Resuable and scalable**: same module can be scaled and used across multiple systems and users
  * **Faster defect isolation**: quickly isolate affected microservice when test fails or service goes down (failure resistant)
  * **Minimised risk of change**: avoids fixing the technologies or languages to use, able to change on the go
* In line with other concepts such as being Agile, having a DevOps culture (CICD) and continuous testing
* Challenges
  * **Building**: spend time to identify dependencies between services (build-wise, data-wise)
  * **Testing**: more difficult integration testing, depending on how services were architected to support one another
  * **Versioning**: complexity in maintenance and management (ensuring backward compatibility, having multiple live versions for different clients)
  * **Deployment**: invest in automation over human deployment to make it easier, think about how and what order services are rolled out
  * **Logging**: need to centralise logs, especially due to the scale in distributed systems
  * **Monitoring**: critical to have a centralised view of system to pinpoint sources of problems
  * **Debugging**: no single answer to this as of yet but remote debugging across local IDE definitely isn't an option
  * **Connectivity**: consider service discovery, whether centralised or integrated
* Opposes _monolithic_ architecture
  * Single, autonomous unit
  * Uses a single logical database across different applications
  * Changes affect entire system, slow to deploy and scale


# RESTful API Design

* Software intermediary that allows two applications to talk to each other
* Based on 4 methods defined by HTTP protocol: **GET, POST, PUT, DELETE**
  * Correspond to the 4 traditional actions (CRUD) performed on data in a database: CREATE, READ, UPDATE, DELETE
* Benefits of a well-designed API
  * Improved developer experience
  * Faster documentation
  * Higher adoption of API
* Characteristics of a well-designed API
  * **Easy to read and work with**: API resources and associated operations can be quickly memorised by developers who use it constantly
  * **Hard to misuse**: easy to implement, has informative feedback and doesn't enforce strict guidelines on API's end consumer
  * **Complete and concise**: usually happens incrementally over time, building on top of existing APIs

## REST API Concepts

* _Resource_
  * An object important enough to be referenced in itself
  * Has data, relationships to other resources
  * Has methods to allow access and manipulation of associated information
* _Collection_
  * A group of resources
* _URL (Uniform Resource Locator)_
  * Identifies the online location of a resource
  * Consists of a
    * **protocol** (e.g. `https://`)
    * **domain** (e.g. `google.com`)
    * **path** is optional (e.g. `/intl/en-GB/gmail/about/`)
  * Base URL (protocol + domain) are the consistent portion of a URL
* _API (Application Programming Interface)_
* _REST (REpresentational State Transfer)_
  * Philosophy that describes best practices for use of APIs
  * APIs designed with some/all of this principles in mind are called "REST APIs"
* _HTTP (Hypertext Transfer Protocol)_
  * Primary means of communicating data on the web
  * Implements a number of methods, tells which direction data is moving and what should happen to it
* HTTP methods
  * `GET`: pulls data from server
  * `POST`: push new data to server
  * `PUT`: update existing data in server
  * `DELETE`: delete existing data in server
* Difference between REST and RESTful
  * REST is a style of _software architecture_
  * RESTful refers to _web services_ implementing such an architecture


## Principles when designing RESTful APIs
1. **Verbs should not appear in request URL**
  * Don't use `GET`, `DELETE` etc in API endpoints, implies that corresponding HTTP method is used
2. **Resource collections should be denoted with plural nouns**
  * Make it clear that API is referring to a collection or an entry (e.g. `books` vs `book`)
3. **Use query parameters**
  * More flexible
  * Allows filtering by multiple database fields
  * Allows <u>"optional" data</u> to be provided
  * Allows retrieval of information relating to a specific child resource
    * Should nesting go too far (after 2nd/3rd level), consider directly returning the URL to those resources instead
  
```
GOOD: http://api.example.com/books?author=Ursula+K.+Le Guin&published=1969&output=xml
GOOD: .../articles/:articleId/comments
BAD: http://api.example.com/getbook/10
```

4. **Allow filtering, sorting and pagination**
  * Too much data should not be returned all at once, would be too slow/hang the system
  * Increase performance by returning a few results at a time
  * Consider specifying fields to sort by in query strings (e.g. `+` is ascending, `-` is descending)

```
EXAMPLE: .../employees?lastName=Smith&age=30
EXAMPLE: http://example.com/articles?sort=+author,-datepublished
```

5. **Plan for future additions and functionalities**
  * Even if current version of API services information on only one type of resource, plan as if other resources of non-resource functionality will be added to the API in the future
  * Add an <u>extra segment</u> on path (e.g. `/resource`, `/entries`) to allow users to search across all resources available
  * Add a <u>version number</u> to path (e.g. `/v1`) such that old version of API can be supported even after redesigning the API
    * _Note_: Usually APIs for different versions have different code/logic implementation, but exist in same script
    * _Alternative_: Specify version in custom/access header

```
GOOD: https://api.example.com/v1/resources/books?id=10
GOOD: https://api.example.com/v1/resources/images?id=10
GOOD: https://api.example.com/v1/resources/all

# Custom header     | used by service, route request to correct endpoint
EXAMPLE: X-API-VERSION
# Accept header
EXAMPLE: Accept: application/vnd.hashmapinc.v2+json

```

6. **Base URL should be neat, elegant and simple**
  * Easier for developers to use
  * Less prone to mistakes during coding
7. **Provide good response feedback**
  * Messages should be self-descriptive
  * Positive validation on correct implementation
  * Informative error on incorrect implementation that can help users debug and correct the way they use the product
  * Use errors to provide context to using an API, <u>align errors around standard HTTP codes</u>
  * Successful requests to add new resources should return URI of newly created resource in Location header

```
# Refer to Appendix for more
statusCode: 4XX - Client/application error
statusCode: 5XX - Server/API error
statusCode: 2XX - Client and API worked
```

8. **Design in mind to handle large payloads efficiently**
  * Reduce size pagination, break data into smaller chunks
  * Extending pagination, organise using common hypermedia formats
  * Provide option of schema filtering
  * Allow users to define representation of resource requests (e.g. simple, complete etc)
  * Use caching for more efficient responses
  * Use HTTP Compression to reduce API response size (e.g. DEFLATE, GzIP)
  * Break responses into chunks, send in order
  * Stream using HTTP with standards like Server-Sent Events (SSE) to deliver large volumes of data as streams (individual updates received by user in a continuous long-running HTTP request)
9. **Common considerations for URLs**
  * Use lower casing
  * Separate words with underscores (_) or hypens (-)
  * Keep as short as possible
10. **Other principles to keep in mind**
  * Design for your clients, not for your data
  * Leverage the hierarchical nature of the URL to imply structure
  * Provide APIs that pretty prints by default

```
# Get post with ID 1 by user with ID 123
EXAMPLE: .../users/123/posts/1
```

## Good security practices

* Keep communication between client and server private
  * Use SSL/TLS protocol
* _SSL (Secure Sockets Layer)_
  * Developed in mid-1990s by Netscape
* _TLS (Transport Layer Security)_
  * New name given to new version of SSL released in 1999
  * Encrypts internet traffic of all types
  * E.g. browser is connected via TLS if URL starts with "`https`"  
* SSL/TLS protocol
  * A continuously updated series of protocols (linked together)
  * Asymmetrical cryptography
    * <u>Public key</u> to encrypt data
    * <u>Private key</u> to decrypt data
    * Two keys related to each other but mathematically complex to reverse-engineer by brute force (takes alot of computing power)
    * **Used only at the very beginning of a communication session between server and client**
  * Symmetrical cryptography
    * A single <u>session key</u> agreed upon by server and client
    * After initial asymmetrical cryptography used in the very beginning, single session key will be used to encrypt packets from then
  * Communication session much secure since session key was established using asymmetrical cryptography
  * <u>Handshake</u> is the process where the session key is agreed upon
* SSL certificate
  * A digital document issued by a third-party authority confirming the server's identity
  * Supplies client with a public cryptographic key necessary to initiate secure connections
  * Authenticates the key is truly associated with organisation supplying key to client
  * Not too difficult to load on server
  * Free/low cost
* Enforce principle of least privilege
  * Add role checks
  * Consider having more granular roles for each user
  * If users have multiple roles, they should have no more and no less the permissions they need
  * If each feature has granular permissions that users have access to, admins should be able to add/remove those features from each user accordingly
  * To make it easier, add preset roles that can be applied to a group of users
* Avoid redirecting non-SSL requests to SSL counterparts, throw a hard error instead
  * A poorly configured client could leak request parameters in the case of a redirection
* <u>Security tokens</u> can be used to enforce access control
  * **token-over-basic-auth method**: in SSL, authentication credentials can be simplified to a randomly generated access token delivered in user name field of _HTTP Basic Auth_
  * **OAuth2**: provide secure token transfer to third party, uses _Bearer tokens_

## Caching data

* Reuse response by returning data from local memory cache instead of querying again
* Include `Cache-Control` information in headers to allow users to effectively use your caching system
* Benefits
  * Allows faster retrieval of data
* Possible problems
  * Cached data may be outdated
* Caching solutions
  * Redis
  * In-memory caching
  * HTTP built-in caching framework: Etag, Last-Modified
  * etc
* **Etag**
  * <u>HTTP header</u> Etag contains hash/checksum of request representation
  * If request contains `If-None-Match` header with matching ETag value, API should return `304 Not Modified` status code
* **Last-Modified**
  * Works similarly to Etag, but uses timestamps
  * <u>Response header</u> `Last-Modified` contains timestamp in RFC 1123 format, validated against `If-Modified-Since`
  * There are 3 different acceptable date formats which server should accept

# Flask

* Micro web framework powered by Python
* Small API, easy to learn and simple to use
* Can support enterprise-level application handling large amounts of traffic
* Up to developer to choose the tools and libraries to use

## 








# API Documentation

* Three kinds of documentation for API
  1. A reference detailing each route and its behaviour
  2. A guide that explains the reference in prose
  3. At least 1 or 2 tutorials that explain every step in detail
* Documentation solutions
  * Swagger
  * etc
`TODO`






# Appendix

## HTTP Status Codes

More commonly-used status codes:
```
# Success
statusCode: 200 OK
statusCode: 201 Created
statusCode: 204 No Content

# Redirection
statusCode: 304 Not modified

# Client Error
statusCode: 400 Bad Request     | Client-side input fails validation
statusCode: 401 Unauthorised    | User is not authorised to access a resource (usually happens when user is not authenicated)
statusCode: 403 Forbidden       | User is authenticated, but not allowed to access a resource
statusCode: 404 Not found       | Resource is not found
statusCode: 409 Conflict

# Server Error
statusCode: 500 Internal Server Error   | Generic server error, should not be thrown explicitly (catch-all error)
statusCode: 503 Service Unavailable     | Unexpected behaviour on server-side (e.g. server overload, part of system failed etc)
```

Overall summary:
```
statusCode: 1XX - Informational
statusCode: 2XX - Success
statusCode: 3XX - Redirection
statusCode: 4XX - Client error
statusCode: 5XX - Server error
```

Detailed information:
* Refer to PDF _HTTP Status Codes_, from [restapitutorial](https://www.restapitutorial.com/httpstatuscodes.html#)

## Design pattern: Exception/Error handling

* **Key takeaways**
  * Exceptions are there are _abnormal conditions_, not those that are reasonably expected
  * Complier implementors usually expect exceptions to be set up often but thrown rarely, hence throw code are usually quite inefficient
  * Use log levels appropriately, decide when to log whole stack trace (ask  yourself if this information is useful in the context) and when to just log a message

* **Defining exception types**
  * Exception names should be clear and meaning
  * State the causes of exception
  * _Fatal_: system crash states
  * _Error_: lack of requirement
  * _Warn_: not an error but error probability
  * _Info_: info for user
  * _Debug_: info for developer

* **When to throw exceptions**
  * When method cannot handle abnormal condition
  * If need to re-throw, throw same exception and consider adding more information in each layer first
  * When error conditions met in method (don't return -1, -2, -3 etc values for invalidness)

* **When not to throw exceptions**
  * When method will return null, control with if-else statements instead (null control)
  * For flow control/managing business logic

* **What exceptions to catch**
  * Catch specific exceptions instead of most generic Exception class (more readable)
  * When method returns NullPointerException (null control)

* **Handling exceptions**
  * Ignoring exceptions will create chaos for maintainability later
  * Minimally log some information or perform an action
  * Avoid logging same exception more than once
  * Always clean up resources
    * E.g. opened files etc
    * In `finally` block
    * Recover object to safe state (in cases like e.g. disk full, failed network connection etc) in languages without garbage collection or when there is exception-handling in a class constructor
  *  

Python code examples:

```
## EXAMPLE 1
    # Returns "Exception <type 'exceptions.ZeroDivisionError'> occurred."
try:
    ...
except:
    print(f"Exception {sys.exc_info()[0]} occurred.")
```
```
## EXAMPLE 2
    # First line is messaged passed in as param, subsequent lines are full stack trace (including exception type)
    # By default, uses log level of ERROR
try:
    ...
except Exception:
    logger.exception("Fatal error in main loop")

## EXAMPLE 3
    # Logging will include full stack trace, logging level can be varied
try:
    ...
except Exception:
    logger.error("Fatal error in main loop", exc_info=True)
```
```
## EXAMPLE 4
    # Re-throw an exception but preserve full stack trace
    # Also known as "Exception Chaining"
    # Happens automatically when exception raised inside except or finally section
    # Provides traceback for both exceptions raised
    # Raised exception has attribute __cause__ whose value is the instigating exception

# Includes statement "The above exception was the direct cause of the following exception:"
try:
    ...
except IOError as exc:
    raise RuntimeError("Failed to open database") from exc

# Includes statement "During handling of the above exception, another exception occurred:"
try:
    ...
except IOError as exc:
    raise RuntimeError("Failed to open database")

## EXAMPLE 5
    # To disable exception chaining
try:
    ...
except OSError:
    raise RuntimeError from None
```
```
## EXAMPLE 6
    # Log an event before propagating exception to be handled at a higher level
    # Not exactly handling exception
    # When there is a higher-level handler for error
    # Useful in troubleshooting
try:
    ...
except SomeError:
    logger.warn("...")
    raise
```



# References
* Microservices
  * https://smartbear.com/solutions/microservices/
  * https://www.redhat.com/en/topics/microservices/what-are-microservices
* RESTFUL apis
  * https://programminghistorian.org/en/lessons/creating-apis-with-python-and-flask#api-design-principles
  * https://swagger.io/resources/articles/best-practices-in-api-design/
  * https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/
  * https://www.csoonline.com/article/3246212/what-is-ssl-tls-and-how-this-encryption-protocol-works.html
  * https://medium.com/hashmapinc/rest-good-practices-for-api-design-881439796dc9
  * https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#pretty-print-gzip
* Flask
  * https://github.com/realpython/discover-flask
* Appendix
  * HTTP Status Codes: https://www.restapitutorial.com/httpstatuscodes.html
  * Design pattern: Exception/Error handling: https://www.tutorialspoint.com/python_design_patterns/python_design_patterns_exception_handling.htm, http://wiki.c2.com/?ExceptionPatterns, http://codebuild.blogspot.com/2012/01/15-best-practices-about-exception.html, https://www.loggly.com/blog/exceptional-logging-of-exceptions-in-python/






* https://www.udemy.com/course/advanced-rest-apis-flask-python/ has OAuth, Postman tests
* https://www.udemy.com/course/rest-api-flask-and-python/ has deployment to heroku, api security
* https://www.udemy.com/course/restful-api-flask-course/
* https://www.udemy.com/course/python-rest-apis-with-flask-docker-mongodb-and-aws-devops/
