# UHF Hypermedia Format: Embedding

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing basic rules for embedding other content in UHF documents   .

The names described herein are:

- `res`

Additionally, this extension mentions the names:

- `ref` (referred to herein as `ptr:ref`, from [pointer](http://uhfs.org/ext/pointer))
- `content-type` (referred to herein as `g:content-type`, from [general](http://uhfs.org/ext/general))

## Definitions

Terms in this document are used as in [[UHF]](http://uhfs.org/uhf).

## Contexts

### resource

The resource context MAY be a complete and conforming UHF document.  The resource context MUST NOT add anything which would be inconsistent with a conforming UHF document.  However, it MAY omit the `uhf` key or any part of it.  Any prefix in the resource context's uhf context which conflicts with a UHF prefix defined in the document containing the resource context supersedes the outer document's prefix.

A conforming parser MUST use any prefixes

## Names

### ref

If present, the value of `ref` MUST be an array of zero or more strings.

Clients MUST NOT infer semantic meaning from the order of items in this array.

Each string value MUST be one of:

- an [IRI](https://tools.ietf.org/html/rfc3987)
- a [SafeCURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/#P_safe_curie) with a prefix known to the parser

The IRI (after expansion if a SafeCURIE) MUST represent a JSON document with a URI fragment conforming to [JSON Pointer](https://tools.ietf.org/html/rfc6901) in the [URI Fragment Identifier Representation](https://tools.ietf.org/html/rfc6901#section-6).  Parsers SHOULD treat an IRI without a fragment as containing a fragment of "#".

The name `ref` MAY appear in the head context from the base UHF specification.  In the head context, parsers MAY treat the base `rel` as relating the head context's link to the value or values pointed at by `ref`, rather than necessarily the entire UHF document.

Extensions which define other contexts allowing `ref` MAY restrict allowed array items in contexts they define.

