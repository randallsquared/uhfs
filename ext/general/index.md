## UHF Hypermedia Format: General

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing rules for a few generally-useful CURIE keys.

The keys described herein are:

- `content-type`
- `label`
- `name`
- `value`

## Definitions

The terms `array`, `number`, and `object`, `true`, `false`, and `null` are used as in [[RFC7159]](https://tools.ietf.org/html/rfc7159).

The term `context` refers to the contents of an object defined by UHF or an extension to UHF.

The term `prefix` refers to the CURIE prefix which is assigned to this extension by the CURIEs definition context.

The term `key` refers to the JSON key for which this extension defines the CURIE reference.

> If this extension is prefixed as `g`, then "the `label` key" refers to `g:label`.

## Contexts

This extension defines no contexts.

## Keys

### content-type

The value of the `content-type` key, if present, MUST be an array.

The `content-type` array MAY contain one or more strings.  Each string MUST be a [media type](https://tools.ietf.org/html/rfc2046).

Clients MUST NOT infer semantic meaning from the order of items in this array.

Extensions which define contexts MAY restrict allowed array items in contexts they define.

### label

The value of the `label` key, if present, MUST be a string.

There are no restrictions on the content of the string.   The `label` SHOULD provide a title or label for the user, about the surrounding context.

Extensions which define contexts MAY restrict the allowed values of `label` in contexts they define.

### name

The value of the `name` key, if present, MUST be a string.

There are no restrictions on the content of the string.

Extensions which define contexts MAY restrict the allowed values of `name` in contexts they define.

### value

The value of the `value` key, if present, MUST be an array.

The `value` array MAY contain one or more items of any of string, number, true, false, null.

Clients MUST NOT infer semantic meaning from the order of items in this array.

Extensions which define contexts MAY restrict allowed array items in contexts they define.
