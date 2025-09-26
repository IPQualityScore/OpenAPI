# Development Notes
v. 0.4
## OAS Version
OpenAPI Specification (OAS) 3.1.0 was released [more than 2 years ago](https://www.openapis.org/blog/2021/02/18/openapi-specification-3-1-released)
but many tools still lack support, for example 42Crunch's VS Code extension. It
seems silly to spend time writing a new definition from scratch in a spec that's
more than 2 years old. At the same time, it also seems unwise not to utilize
some sort of tool. [@redocly/cli](https://www.npmjs.com/package/@redocly/cli)
has 3.1 support and seems to work well for validation.

Another irritation: Postman doesn't support multi-file definitions in 3.1 but
does for 3.0. Multi-file is definitely preferable for legibility, modularity,
etc.

The majority of the specification is the same between 3.0 and 3.1 [see here](https://www.openapis.org/blog/2021/02/16/migrating-from-openapi-3-0-to-3-1-0), so I think
the approach we should take to be able to use available tools is specify 3.0 (as
much as I dislike creating new things in an "out-dated" standard) and
ideally make notes where the versions differ, perhaps with a particular tag so
these sections can be quickly found later if necessary.

Example of difference: 3.1 allows specifying an array of types, so that the
user-defined variables for example would be:
type:
    - integer
    - string
but this didn't exist in 3.0

### License
- [API Handyman](https://apihandyman.io/what-is-the-info-property-in-openapi/#what-is-a-license-for-an-api)
- [Clarification on the purpose of the API license?](https://github.com/OAI/OpenAPI-Specification/issues/726)

### Tools
- Swagger
- Redocly
- Postman Enterprise (for integrating with self-hosted GitLab)
  *Postman validation and automatic documentation not available for free users*
- [OpenAPI Generator](https://openapi-generator.tech/)

### Responses
Currently, two responses are defined: a success and a failure. I'm not sure yet
whether this necessary, if one response suffices (since the API returns 200
even when success: false), or if additional responses should be defined for the
different possible number of response items (i.e. the number of items is
greater when the request includes "user_agent").

From the [specification](https://spec.openapis.org/oas/v3.1.0#responses-object):
> The documentation is not necessarily expected to cover all possible HTTP response codes because
> they may not be known in advance. However, documentation is expected to cover a successful
> operation response and any known errors.

For me, this raises the question: "should we explicitly define a response for
every type of known error?" e.g. "Invalid API key", "Invalid IP", etc.

### Request Bodies
API key is required, but for a POST request, it may be included either in the
header as IPQS-KEY or in the request body. So, "key" is technically not required
in the body, so long as it is in the header, and vice-versa.

See section on Deep Objects below. The way to specify something like:
"update[transactionID]=100" in request bodies is awkward, and Postman doesn't
seem to like it.

### Descriptions
Some of our descriptions are quite long and occur multiple times, but I don't
think there is currently a way of reusing descriptions, as there is with other
types of components.

### Security (in the OAS sense)
An OAS Security Object is required. API Key is correct but doesn't allow for
passing the key via POST requests (i.e. in: header and in: query exist but not in: "body").

### TODO
- [X]  Proxy & VPN API definition
  - includes transaction scoring?
- [X]  Email validation API definition
- [ ]  POST request bodies
- [ ]  Stats & Averages (proxy)
- [x]  Device fingerprint
- [x]  Phone
- [x]  URL
- [x]  Report (move out of email validation)
- [x]  Account
- [x]  Requests
- [ ]  Postback
  - [x] Proxy
  - [x] Email
  - [x] Device
  - [ ] Mobile

### Tag ideas
- proxy
- vpn
- transaction
- fingerprint
- credit card
- email
- payment
- device
- url
- request list
- postback
- conversion
- phone number
- report
- averages
- account

> Note: To describe API keys passed as query parameters, use securitySchemes and
> security instead. See API Keys.

Doing anything with the entire list of parameters is exhausting, so we need to
maximize reuse+modularity.

#### Other
- [ ]  move Transaction Details to separate file
- [ ]  other reuse improvements
- [ ]  add 400 responses to appease linter

Is it OK to have multiple response types, e.g. XML and JSON? (This seems to be fine)
see: email_validation.yaml#/components/responses/OK/contents

### Deep Object
How to specify/differentiate lookup (e.g. transactionID) and update (e.g.
update[transactionID])? In other words, is there a "composable" parameter?
like: "update[{variable}]"
- [StackOverflow](https://stackoverflow.com/questions/56279024/openapi-parameter-with-brackets-and-variable-name)
- [Serialization](https://swagger.io/docs/specification/serialization/#query)
"deepObject"
See: "components/requestBodies/postbackRequestBody.yaml"

I'm still figuring out how to define responses since they are broadly very
similar but have minor differences based on path and provided parameters...
- https://swagger.io/docs/specification/describing-responses/
- "In OpenAPI 3.0, you can use oneOf to specify alternate schemas for the
  response and document possible dependencies verbally in the response
  description. However, there is no way to link specific schemas to certain
  parameter combinations.**

**Double Check**
- phone number: long integer? minimum/maximum?
- expiration month: number or string?
- order_quantity: integer?
- enumerate AVS/CVV response codes?

No straightforward way to specify conditionally required parameters, as in
"paths/{format}_report_{key}.yaml#/parameters:" that country code and phone are
both required if one of them is used


### Parameters
The amount of parameters accepted is extremely cumbersome to work with in
OpenAPI. See: https://github.com/OAI/OpenAPI-Specification/issues/445 &
https://stackoverflow.com/questions/32091430/how-to-group-multiple-parameters-in-swagger-2-0



### Concerns
Some customers might interpret the OpenAPI definition as a "contract" (which is
technically the OAS perspective), but obviously we want the API itself (or an
internal document) to be the "Single Source of Truth" (SSOT) at least until
we're ready to explicitly make the OpenAPI the SSOT. In other words, we need to
be explicit that the "definition" exists as a resource or tool to help customers
but not the actual ***Definition** unless we make that decision at some point.

One way of approaching the definition is to think of it as not describing
*everything* that the API does/can do, but as the interface we guarantee. In
that sense, we could slim down the definition and point new users towards it,
and theoretically simplify the API behind the scenes.

In any case, we should ideally spend the time necessary to test these thoroughly.

##### footnotes
[Time and Date on the Internet](https://www.rfc-editor.org/rfc/rfc3339)
    type: string
    format: date
    &
    type: string
    format: date-time

Broken links and errata:
[CVV Codes](https://www.ipqualityscore.com/documentation/proxy-detection/transaction-scoring)
    - suggestion: http://www.emsecommerce.net/avs_cvv2_response_codes.htm
typo in https://www.ipqualityscore.com/documentation/proxy-detection/averages "necessarily"
