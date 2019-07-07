# UHF Hypermedia Format: Resource

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing basic rules for embedding other content in UHF documents.

The names described herein are:

- `res`

Additionally, this extension mentions the names:

- `ref` (referred to herein as `ptr:ref`, from [pointer](http://uhfs.org/ext/pointer))
- `content-type` (referred to herein as `g:content-type`, from [general](http://uhfs.org/ext/general))

## Definitions

Terms in this document are used as in [[UHF]](http://uhfs.org/uhf).

## Contexts

### embedded

The embedded context MAY be a complete and conforming UHF document.  The embedded context MUST NOT add anything which would be inconsistent with a conforming UHF document.  However, it MAY omit the `uhf` name or any part of it.  Any prefix in the embedded context's uhf context which conflicts with a UHF prefix defined in the document containing the embedded context supersedes the containing document's prefix.

An embedded context MUST NOT contain a `ptr:ref` name.

### reference

The reference context MUST contain a `ptr:ref` name.

The reference context MAY contain the `g:content-type` name, or a synonym, from [general](http://uhfs.org/ext/general).

The reference context MUST NOT contain any of the names `uhf`, `head`, or `body`.

### resource

A resource context is either an embedded context or a reference context.  If the resource context contains a `ptr:ref` name, it is a reference context, otherwise, it is an embedded context.

## Names

### res

If present, the value of `res` MUST be an array of zero or more objects.  Each such object is a resource context.
