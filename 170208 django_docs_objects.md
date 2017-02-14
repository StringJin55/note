## HttpRequest objects¶

class HttpRequest[source]¶

### Attributes¶

All attributes should be considered read-only, unless stated otherwise.

#### HttpRequest.POST¶
A dictionary-like object containing all given HTTP POST parameters, providing that the request contains form data. See the QueryDict documentation below. If you need to access raw or non-form data posted in the request, access this through the HttpRequest.body attribute instead.

It’s possible that a request can come in via POST with an empty POST dictionary – if, say, a form is requested via the POST HTTP method but does not include form data. Therefore, you shouldn’t use if request.POST to check for use of the POST method; instead, use if request.method == "POST" (see above).

Note: POST does not include file-upload information. See FILES.