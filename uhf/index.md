# UHF Hypermedia Format: Default

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.

## Definitions

The terms `value`, `array`, `number`, `object`, `true`, `false`, and `null` are used as in [[RFC7159]](https://tools.ietf.org/html/rfc7159#section-2).

The term `name` refers to a [CURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/). If the default prefix is set in the uhf context as "u", then all of the following possible JSON names would be written in this document as `example`.

- `"example"`
- `":example"`
- `"u:example"`
- `"[example]"`
- `"[:example]"`
- `"[u:example]"`

UHF parsers MUST treat these as the same UHF name.  UHF parsers MAY process all names to derive canonical IRIs or CURIEs.  UHF contexts MUST NOT have names which represent the same IRI.  It is not allowed to have both `:body` and `[body]` in the same context, though either of those is itself an allowed spelling of the `body` name.

The term `context` refers to an object which has name/value pairs specified by UHF or a UHF extension.

## Names

### body

If present, the value of `body` is out of the scope of this specification. UHF extensions SHOULD NOT specify anything about its interior structure.  The value of the `body` name is not a UHF context even if it is an object.

The name `body` MUST NOT appear in any UHF context except a root context.

### head

The value of `head` is an array of zero or more objects.  Each such object is a head context.

The name `head` MUST NOT appear in any UHF context except a root context.

### rel

The value of `rel` is an array of zero or more strings.

Each such string MUST be one of:

- a [registered link relation](https://www.iana.org/assignments/link-relations/link-relations.xhtml)
- an [IRI](https://tools.ietf.org/html/rfc3987)
- a [SafeCURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/#P_safe_curie) with a prefix known to the parser

The `rel` name MAY appear in a head context, or anywhere under `head` if allowed by an extension.

### uhf

The value of `uhf` is the uhf context.

The name `uhf` MUST NOT appear in any UHF context except a root context.

### uri

If present, the name `uri` MUST have a string value.  This string MUST be one of:

- an [IRI](https://www.ietf.org/rfc/rfc3987.txt)
- a [SafeCURIE](https://www.w3.org/TR/2010/NOTE-curie-20101216/#P_safe_curie) with a prefix known to the parser

The `uri` name MAY appear in a head context, or anywhere under `head` if allowed by an extension.


## Contexts

This specification defines three contexts.

### root

The root context is an object which conforms to this specification.

The root context MAY have a name `body` with the default prefix.

The root context MAY have a name `head` with the default prefix.

The root context MUST have a name `uhf` with the default prefix.

The root context MUST NOT have any other name.

If an extension provides a context which is a UHF root context (an embedded UHF resource), the extension MUST specify whether prefixes named in child resources override prefixes named in parent resources which include such a context.

### uhf

The uhf context MUST have a name/value pair in which the value is the string `"http://uhfs.org/uhf"`. The name of this pair is the default prefix for the UHF names defined in this specification. Future changes to this specification will require different strings for the default prefix value.

Other name/value pairs MAY appear in this context.

For other name/value pairs appearing in this context:

- Each name MUST be a [CURIE prefix](https://www.w3.org/TR/2010/NOTE-curie-20101216/#s_syntax).
- Each value MUST be an [IRI](https://tools.ietf.org/html/rfc3987), and MUST NOT be a CURIE or SafeCURIE.

### head

Objects in the head array MUST have a name `rel` with the default prefix.

Objects in the head array MAY have a name `uri` with the default prefix.

Objects in the head array MUST NOT have any name with the default prefix other than `rel` AND `uri`.

Objects in the head array MAY have one or more names which are CURIEs with other prefixes named in the uhf context.

Objects in the head MUST NOT have any other names.

## Extensions

A document defining an extension to UHF MUST have at least the following sections:

### Definitions

This section should define any terms in the extension specification which might otherwise be ambiguous.

### Names

This section should list any name/value pairs described by this extension.

The extension MUST provide the JSON type or types which may appear as the value of a name.

The extension MAY provide other restrictions concerning a value which are not expressible as JSON type information.

The extension SHOULD provide guidance on the usage of a name in context.

The extension MAY provide guidance or restrictions on the usage of a name in other extensions.

The extension SHOULD provide information about the semantic meaning and/or intended use of a name.

### Contexts

This section should list any UHF contexts described by this extension.

A name which has (or may have) an object as its value MAY describe a context for other UHF keys.

The extension MUST provide information about what names may or may not appear in a context.

The extension SHOULD provide information about the semantic meaning and/or intended use of a context.
