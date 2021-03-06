# UHF Hypermedia Format: HTTP

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing rules for specifying additional request information related to HTTP.

The names described herein are:

- `headers`
- `http`
- `method`
- `require`

Additionally, this extension incorporates from the [general](http://uhfs.org/ext/general) extension the names:

- `name` (referred to herein as `g:name`)
- `value` (referred to herein as `g:value`)

## Definitions

Terms in this document are used as in [[UHF]](http://uhfs.org/uhf).

## Contexts

### header

A header context object represents a single header to be sent with the HTTP request described by the context in which `header` appears.

A header context object MUST have a name, `g:name`, with a string value matching the name of some HTTP header.

A header context object MAY have a name, `g:value`, with a string value matching the header value to send when sending the header.

A header context object MAY have a name, `inherit`, with a boolean value.  If this value is `true`, conforming clients SHOULD take as the value of this header the equivalent header in the request producing this document.  If this value is `false`, conforming clients SHOULD NOT use the producing request's equivalent header.

Given a header context containing both `g:value` with some string value and `inherit` with a value of `true`, conforming clients SHOULD take the `g:value` value as the header value if unable to take the equivalent header in the request producing this document.

A header context object MAY have one or more names with other defined prefixes, but this extension describes no additional semantics or requirements for any such names.

### http

The http context represents HTTP-specific information about requests to the URI given by names in the head context from the base UHF specification.

The http context MAY have any or all of the names `header`, `method`, or `required`, as they are defined in this extension.

The http context MAY have one or more names with other defined prefixes, but this extension describes no additional semantics or requirements for any such names.

## Names

### header

If present, the value of `header` MUST be an array of zero or more objects.  Each such object is a header context.

The name `header` MUST NOT appear in any context defined in the base UHF specification.  It MAY appear in the http context.

Extensions which define contexts allowing `header` MAY restrict allowed values for `header` in contexts they define.

### http

The value of `http` is the http context.

The name `http` MAY appear in the head context from the base UHF specification.

Extensions which define other contexts allowing `http` MAY restrict the value of `http` in contexts they define.

### inherit

If present, the value of `inherit` MUST be either `true` or `false`.

The name `inherit` MUST NOT appear in any context defined in the base UHF specification.  It MAY appear in the header context.

### method

If present, the value of `method` MUST be a string naming an HTTP method, such as "GET", "POST", "PUT", "DELETE", or "PATCH".

The name `method` MUST NOT appear in any context defined in the base UHF specification.  It MAY appear in the http context.

Extensions which define other contexts allowing `method` MAY restrict allowed values for `method` in contexts they define.

### required

If present, the value of `required` MUST be an array of zero or more strings.

The name `required` MUST NOT appear in any context defined in the base UHF specification.  It MAY appear in the http context.

Extensions which define other contexts allowing `required` MAY restrict allowed values for `required` in contexts they define.
