# UHF Hypermedia Format: JSON Pointer

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing basic rules for JSON pointers in UHF.

The name described herein is:

- `ref`

## Definitions

Terms in this document are used as in [[UHF]](http://uhfs.org/uhf).

## Contexts

This extension defines no contexts.

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
