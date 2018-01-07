## UHF Hypermedia Format: General

UHF is a JSON hypermedia format.  UHF's media type is `application/vnd.uhf+json`.  For more information see [http://uhfs.org](http://uhfs.org).

This is an extension to UHF, providing rules for a few generally-useful names.

The names described herein are:

- `content-type`
- `contentType`
- `label`
- `name`
- `value`

## Definitions

Terms in this document are used as in [[UHF]](http://uhfs.org/uhf).

## Contexts

This extension defines no contexts.

## Names

### content-type

If present, the value of `content-type` MUST be an array of zero or more strings. Each string MUST be a [media type](https://tools.ietf.org/html/rfc2046).

Clients MUST NOT infer semantic meaning from the order of items in this array.

Extensions which define contexts MAY restrict allowed array items in contexts they define.

### contentType

The name `contentType` is an alternate spelling for the name `content-type`.

### label

If present, the value of `label` MUST be a string.

There are no restrictions on the content of the string.   UHF clients SHOULD present `label` as a title or label referring to the surrounding context.

Extensions which define contexts MAY restrict the allowed values of `label` in contexts they define.

### name

If present, the value of `name` MUST be a string.

There are no restrictions on the content of the string.

Extensions which define contexts MAY restrict the allowed values of `name` in contexts they define.

### value

If present, the value of `value` MUST be one of array, string, number, true, false, null.

If an array, a `value` array MUST contain zero or more of string, number, true, false, null.

Clients MUST NOT infer semantic meaning from the order of items in a `value` array.

Extensions which define contexts MAY restrict allowed values for `value` in contexts they define.
