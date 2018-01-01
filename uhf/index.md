# UHF Hypermedia Format: Default

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.

## Specified keys

Specified keys are JSON object keys which are defined by this document or extensions to UHF.

## CURIEs definition object

UHF uses [CURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/)s to namespace all specified keys, allowing extensions by adding new CURIE definitions to a JSON object such that the CURIE prefix is the key, and the mapped IRI is the value.  The mapped (partial) IRI is called the "expansion".  This JSON object is called the "CURIEs definition object".

###### Example

```json
{ "default": "http://uhfs.org/uhf", "bar": "http://example.org/foo/bar" }
```

A default key MUST be present in the CURIEs definition object, and MUST have a string value starting with `http://uhfs.org/uhf`, and optionally continuing with a forward slash ('/') and an integer representing a change revision number.  This specification is currently unversioned, and the intent is to satisfy future changes via extensions rather than changes to this core document, but if such changes are necessary and if they have effects which UHF parser implementations must take into account, the default prefix MAY reference any revision of this specification which exists at the time of UHF document creation.

If two or more expansions have `http://uhfs.org/uhf`

Given the default CURIE prefix as `default`, the following CURIEs are equivalent as UHF keys:

- `"uhf"`
- `":uhf"`
- `"default:uhf"`
- `"[uhf]"`
- `"[:uhf]"`
- `"[default:uhf]"`

While the final three forms are allowed in conformance with [CURIE syntax](https://www.w3.org/TR/2010/NOTE-curie-20101216/#s_syntax), the SafeCURIE forms are superfluous in UHF-defined keys, because no IRIs are allowed as UHF-defined keys.  Nevertheless, conforming UHF parsers MUST correctly handle SafeCURIEs, even in UHF-defined keys.

## Extensions

Let's talk about what extensions need to define...

## Structure

A UHF resource is a JSON object conforming to the following rules:

A UHF resource MAY have a key which is a CURIE with the default prefix and a reference of "body".  Its value is not specified.

A UHF resource MAY have a key which is a CURIE with the default prefix and a reference of "head".  Its value is an array.

A UHF resource MUST have a key which is a CURIE with the default prefix and a reference of "uhf".  Its value is the CURIEs definition object.

A UHF resource MUST NOT have any key in the root context which incorporates a prefix other than the default prefix.

A UHF resource MUST NOT have in any object two or more specified keys for which CURIE processing produces a duplicate IRI.


## Keys

### body

The value of the `body` CURIE key, if present, is out of the scope of this specification. UHF parsers SHOULD NOT assume anything about the interior structure of `body`.

### head

The value of the `head` CURIE key, if present, is a JSON array of JSON objects, called the "`head` array".

An object in the `head` array MUST have a key which is a CURIE with the default prefix and a reference of "rel". Its value is a JSON array.

An object in the `head` array MAY have a key which is a CURIE with the default prefix and a reference of "title". Its value is a string.

An object in the `head` array MAY have a key which is a CURIE with the default prefix and a reference of "uri". Its value is a string.

An object in the `head` array MAY have other keys specified by expansions in the CURIEs definition object.

### rel

The value of the `rel` CURIE key is a JSON array of zero or more strings, called the "`rel` array".

Each string in the `rel` array MUST be one of:

- a [well-known link relation](https://www.iana.org/assignments/link-relations/link-relations.xhtml)
- an [IRI](https://www.ietf.org/rfc/rfc3987.txt)
- a [SafeCURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/#P_safe_curie) with a prefix known to the parser

### title

The value of the `title` CURIE key, if present, is a string.

Implementations SHOULD provide this string as a label, title, or context for whatever content is included or referenced by the surrounding context.

### uhf

The `uhf` CURIE key MUST have as its value an object, called the "CURIEs definition object".  The CURIEs definition object is a mapping from CURIE prefixes to IRI expansions. The CURIEs definition object contains no processable CURIEs.  Implementations MUST NOT process either keys or values of the CURIEs definition object as CURIEs themselves (no recursion).

Keys of the CURIEs definition object MUST conform to the requirement for [CURIE prefixes](https://www.w3.org/TR/2010/NOTE-curie-20101216/#s_syntax).

Values of the CURIEs definition object MUST conform to the requirement for [IRI](https://tools.ietf.org/html/rfc3987#section-2.2)s.

#### Scope

The `uhf` CURIE key MUST NOT occur in any object other than a conforming UHF resource.

CURIE mappings represented in the CURIEs definition object apply to the three root specified keys, and to specified keys in objects in the `head`, except where otherwise specified by an extension.

If an extension allows including UHF resources into other UHF resources, the extension MUST specify whether CURIEs definition objects in child resources override the CURIEs definition objects in parent resources.


### uri

The value of the `uri` CURIE key, if present, is a string.  This string MUST be one of:

- an [IRI](https://www.ietf.org/rfc/rfc3987.txt)
- a [SafeCURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/#P_safe_curie) with a prefix known to the parser



###### Examples

...
