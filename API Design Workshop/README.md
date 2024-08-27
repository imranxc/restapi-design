<a href="https://www.youtube.com/playlist?list=PL_XxuZqN0xVAWGDKIzcn6NWikVkljJQZc"> <img src="https://i.ibb.co/KqswBh7/image.png" alt=""> </a>

API is a Contract between 2 parties.

- Consumer
- Producer

### API Architectural Style

- SOAP
- REST
- gRPC
- Web hook
- GraphQL

### Why API Design is Important?

- Interoperability
- Abstraction
- Reusability
- Adaptability
- Developer Experience
- Security & Reliability

### Public Docs

> API first approach priorities APIs at the beginning of the software development process.

- https://woocommerce.github.io/woocommerce-rest-api-docs/
- https://docs.github.com/en/rest/
- https://developer.spotify.com/documentation/web-api

### Different types of API Consumers

- Public
- Private
- Partner (privately shared between 2 parties)

### Life Cycle of an API

![api-life-cycle](https://voyager.postman.com/infographics/producer-consumer-api-lifecycle-postman.png)

## What is API Design?

API design is the process of making intentional decisions about how an API will expose data and functionality to its consumers. A successful API design describes the API endpoints, methods, and resources in a standardized specification format.

#### Design Process

Step 1: Determine what the API is intended to do   
Step 2: Define the API contract with a specification   
Step 3: Validate your assumptions with mocks and tests   
Step 4: Document the API  

### What is your API?

- REST API
- REST like API

> To become a good developer, you've to understand business domain very well.

### What is REST API?

REST stands for *Representational State Transfer* and API stands for *Application Programming Interface*. It's an architectural style for networked applications, defining principles for resource identification, addressing, and data exchange between clients and servers via HTTP.

- A resource is any data object or entity that can be accessed or manipulated through rest API.
- Representational state - it represents the current state of the resources. Representation can be XML, JSON etc

### Constraints of REST API

- Client-Server
- Cacheability (cache control header)
- Uniform Interface
	- Identification of resources
	- Manipulation of resources through representations
	- Self descriptive messages
	- HATEOAS
- Layered System
- Code-on-demand
- Statelessness

### How do we measure API?

<img src="https://dev-to-uploads.s3.amazonaws.com/i/aregpuzds2v57k4shgpa.PNG">

#### Richardson Maturity Model

- https://restfulapi.net/richardson-maturity-model/
- https://blog.restcase.com/4-maturity-levels-of-rest-api-design/
- https://martinfowler.com/articles/richardsonMaturityModel.html

### Partial Response

partial response is a method that lets API users to choose what information they want in the response.

```py
GET - http://127.0.0.1:8000/api/v1/products?fields=name,photo,price
```

#### Benefits

- Reduce bandwidth wage
- Improved Performance
- Optimized client-side performance
- Simplified API Consumption
- Reduced over-fetching

### Query Parameters

Query parameters are a fundamental aspect of HTTP requests used to specify additional information for a resource retrieval or manipulation operation. They are appended to the URL of the request and consist of a key-value pair separated by an equal sign (=) and delimited by an ampersand (&) if multiple parameters are present.

- filtering: `?min_price=600&max_price=1200`
- Sorting: `?sort=price&order=desc`
- Searching: `?search=iphone`
- Pagination: `?limit=10&offset=1`
- Partial Fields: `?fields=name,product,price`

### Well structured response body

#### GET

```py
{
	"status": 200,
	"message": "",
	"information": "",
	"data": {
		"id": "uuid",
		"links": {
			"self": ...,
			"update": ...,
			"delete": ...,
			"add_to_cart": ...
		}
	},
	"trace_id": "456789"
}
```

#### POST

```py
{
	"status": 201,
	"message": "Created Successfully",
	"data": {
			"id": "uuid",
			"links": {
				"update": ...,
				"delete": ...,
				"get": ...,
				"self": ...
			}
		},
	"trace_id": "789"
}
```

#### GET all Products

```py
"status": 200,
"message": "",
"information": "",
"data": [
	{
		"id": "uuid",
		"links": {
			"self": ...,
		}
	},
	{

	}
],
"pagination": {
		"offset": 5,
		"limit": 2,
		"total_pages": 20,
		"total_items": 100,
		"links": [
			"self": ...,
			"first": ...,
			"last": None,
			"prev": 1,
			"next": 3
		]
	}
```

## Error Response

- Use standard HTTP status code
	- 400 - Bad request
	- 401 - Unauthorized
	- 403 - Forbidden
	- 404 - Not found
	- 422 - Unprocessable Content
	- 500 - Internal server error
- Provide descriptive error message
- Follow a consistent error message
- Include error details
- Follow a consistent error format
- Include error details
- Handle uncaught exceptions gracefully
- Offer guidance for recovery
- Response body should includes
	- code
	- message
	- hints
	- trace_id

```py
{
	"message": ...,
	"errors": [],
	"hints": "",
	"trace_id": "124588"
}
```

## HTTP Cache-Control

![cache](https://i.ibb.co/wsV27C4/image.png)

### Directives

- Max-Age: Specifies the maximum time (in seconds) that a response can be cached by the client or intermediary caches.
	- For example, **Cache-Control: max-age=3600** indicates that the response can be cached for up to one hour.
- S-Max-Age: Similar to max-age, but applies only to shared caches (e.g., proxies). It overrides the max-age directive for shared caches.
	- For example, **Cache-Control: s-maxage=3600** specifies that shared caches can cache the response for one hour.
- No-Cache: Indicates that a response can be cached by the client or intermediary caches, but must be re-validated with the server before each use. It does not prevent caching but requires validation of the cached response's freshness. For example, **Cache-Control: no-cache**.
- No-Store: Specifies that a response should not be stored in any cache, including browser caches and intermediary caches. It instructs clients to fetch the response from the server for each request. For example, **Cache-Control: no-store**. Sensitive data should not be stored anywhere (e.g., Banking data, Medical record)

### Public Cache

Specifies that a response can be cached by any cache, including both private (client-side) caches and shared caches (e.g., proxies).Sensitive data should not be cached.  
For example, Cache-Control: public.

![public](https://i.ibb.co/G3DqJK4/image.png)

### Private Cache

Indicates that a response can be cached by the client's browser but not by shared caches. It is typically used for responses intended for a specific user or client.  
For example, Cache-Control: private.

![private](https://i.ibb.co/qRDYqSm/image.png)

### Etag Header

The Etag (Entity Tag) header is an HTTP response header that provides a mechanism for web servers to assign a unique identifier to a specific version of a resource. Etags are used for cache validation and conditional requests, allowing clients to determine whether their cached representation of a resource is still valid without having to download the entire resource again from the server.

The primary purpose of the Etag header is to provide a lightweight and efficient way to validate cached responses and reduce unnecessary data transfer

#### Steps

- Etag Generation: When a client creates a new resource server generates an Etag value for the current version of the resource.
- Inclusion in Response: Server includes the Etag value in response headers using Etag header.
- Storage by Client: The client save (cache, local storage) the response along with the Etag value for future reference.
- Conditional Requests: When the client makes a subsequent request for the resource, it includes the stored Etag value in the `If-None-Match` header of the request. For example: `If-None-Match: "abcdef123456"`
- Validation by Server: Upon receiving the request, server compares the Etag value provided by the client with the current Etag value of the resource. If the Etag values match, it indicates that the cached representation is still valid, and the server responds with a `304 Not Modified` status code, indicating that the client should use its cached copy. If the Etag values do not match, the server responds with the full resource content, along with a new Etag value for the updated version.

![Etag](https://i.ibb.co/WnZbYQM/Capture3.png)

## REST API Versioning

REST API versioning helps to iterate faster when the required, breaking or non-breaking, changes are identified.

### Benefits

- Backward Compatibility
- Incremental Updates
- Flexibility
- Maintainability
- Documentation and Communication

### Examples of Breaking Change

- Changing the URI structure of an existing API endpoint
	- Before: `GET /api/v1/product/{id}`
	- After: `GET /api/v1/products/{id}`
	- This change could break existing client implementations that rely on the
old URI structure, causing requests to fail with `404 Not Found` errors.
- Modifying the behavior of existing API methods or endpoints in a way that affects client applications.

### Versioning of Breaking Change

- URL Based
- Header Based

### Examples of Non-breaking Change

- Adding a new endpoints
- Optimizing Performance
- Adding a new field to the product representation

```py
# Before
{
	"id": "123456",
	"name": "Iphone 15"
}
# After
{
	"id": "123456",
	"name": "Iphone 15",
	"color": "grey"
}
```

### Best Practices of Handling API Changes

- Versioning strategy
	- URL
	- Header
- API Documentation
- Backward Compatibility
- Graceful Deprecation
- Monitoring & Feedback

## REST API Specification

The "REST API Specification" ensures that there is a clear and well-defined contract between these two parties, In the "REST API Specification," we follow two types of contracts:

- Contract-last, also known as the Code-first approach. 
- Contract-first, also known as the Design-first approach.

### Open API Specification

- [OpenAPI Specification v3.1.0](https://spec.openapis.org/oas/latest.html)
- [Swagger Editor](https://editor.swagger.io/)

## REST API Security

API security is the practice of preventing and mitigating attacks that originate at the API level and it is a crucial pillar of any organization's overall security.

### Common API Threats and Vulnerabilities

- Poor security hygiene
- Authentication & Authorization Vulnerabilities
- Lack of read and write granularity
- Failure to implement quotas and throttling
- Improperly set or missing HTTP headers
- Failure to perform input validation, sanitation and encoding in the method level

### How to secure REST API?

- Authentication
	- Basic authentication
	- Session Authentication
	- JWT
	- API Key
	- Oauth 2.0
		- Grant types
			- Authorization code
			- Implicit grand
			- Authorization code grant with proof key for code exchange
			- Resource owner credentials
			- Client credentials
			- Device authorization flow
			- Refresh token grant
- Authorization
- Input validation and sanitation
- Rate limiting
- Security headers
- Logging and continuous monitoring

### Access Control Best Practices

- Implement RBAC
- Throttle requests
- Use HTTPs
- Use UUID over incremental ID

### Input Data Validation

- Use Proper HTTP Methods
- Validate Content-type on request header
- Validate & sanitize request body
- Avoid sending sensitive data in the query parameters
- Use only secure server side encryption.

### Security Headers

- Content-Security-Policy: A powerful allow-list of what can happen on your page which mitigates many attacks Cross-Origin-Opener-Policy: Helps process-isolate your page
- Cross-Origin-Resource-Policy: Blocks others from loading your resources cross-origin
- Origin-Agent-Cluster: Changes process isolation to be origin-based
- Referrer-Policy: Controls the Referer header
- Strict-Transport-Security: Tells browsers to prefer HTTPS
- X-Content-Type-Options: Avoids MIME sniffing
- X-DNS-Prefetch-Control: Controls DNS prefetching
- X-Download-Options: Forces downloads to be saved (Internet Explorer only)
- X-Frame-Options: Legacy header that mitigates clickjacking attacks
- X-Permitted-Cross-Domain-Policies: Controls cross-domain behavior for Adobe products, like Acrobat
- X-Powered-By: Info about the web server. Remove because it could be used in simple attacks
- X-XSS-Protection: Legacy header that tries to mitigate XSS attacks, but makes things worse, so Helmet disables it

## API Management

API management is the organized control of APIs throughout their life cycle, including design, deployment, security, monitoring and monetization.

- Design
- Development
- Deployment
- API monitoring and analytics
- Documentation and developer portals
- Life cycle management
- Monetization & Billing

2 types of API management tools

- Proxy-Based
- Agent-Based
