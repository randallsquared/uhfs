
This is a draft.   It was last updated Jan 6, 2018.

This site is the home of the UHF Hypermedia Format (UHF).

UHF is a deliberately simple format.  It consists of five JSON keys (with various spellings) and usage rules that permit some extensions. Together, these fully cover only the most basic hypermedia use case: linking. The only H Factor supported is [[LO]](http://amundsen.com/hypermedia/hfactor/#LO).

1. three keys dividing a UHF document into three sections:
    - uhf
    - head
    - body
1. two keys that can only appear under `head`:
    - rel
    - uri
1. an extension / namespacing mechanism for additional capabilities

A UHF document can have a root-level key called `body`, and anything can go in here.   Your non-hypermedia API output, for example.

A UHF document can have a root-level key called `head`, which names an array of links that can each have a URI, and must have a set of rels.

Every UHF document has a root-level key called `uhf`.  This is the core of the format.  The JSON object that `uhf` names is a mapping from "prefix" keys to partial URIs.

## CURIEs

Hypermedia formats inherently suffer to a degree from verbosity, since links, actions, and other affordances are inseparable from URIs.   CURIEs are a mechanism for shortening URIs, so that with a prefix definition of `g` meaning `http://www.example.com`, one can use `g:/about` instead of `http://www.example.com/about`.  (If you are familiar with the term "curies" from HAL, note that what HAL calls "curies" are really URI templates).

Every key that has meaning in UHF has a CURIE prefix and a CURIE reference. Allowed prefixes include all the keys in the object named by `uhf`.  Allowed references for keys are specified at the document retrieved at the prefix URI. Therefore, the prefix URI should always be a dereferenceable link, and will provide information about what references are defined by the extension specification.

The CURIE definition object at `uhf` may well have CURIEs defined that are not UHF, and not intended to define keys, since some values can also be CURIEs in some form.  This may seem ambiguous at first, but remember that the document producer does not need to discover what CURIEs they are using, and the document consumer only needs to match prefix URIs against local capabilities for keys they actually find: no one is ever in a position of having to distinguish UHF format CURIEs from other CURIEs.

Some last details about CURIEs: they can have surrounding square brackets (called SafeCURIEs, since you can use them in a place that might have a URI), and the prefix part is optional.  If the prefix part is missing, the colon is optional.  A missing prefix is considered to use the "default" prefix.  A format can define a default prefix, or a mechanism for determining that default prefix. UHF takes this option: the default prefix is the one that has a value of `http://uhfs.org/uhf`, right now.  There are some other details about versioning, but they won't matter until the specification is complete and there's been a change to it.

In

```json
{ "uhf": { "a": "http://uhfs.org/uhf" } }
```

which is a useless document, but also the smallest valid UHF document, the default prefix is `a`.   The six core keys mentioned above use the default prefix, which means that the bare `uhf` key is fine, but it could also be written

```json
{ "a:uhf": { "a": "http://uhfs.org/uhf" } }
```
or

```json
{ "[a:uhf]": { "a": "http://uhfs.org/uhf" } }
```

or even

```json
{ ":uhf": { "a": "http://uhfs.org/uhf" } }
```

There are, in fact, six ways to write a valid key based on the default prefix `a` with a reference of `uhf`:

- `"uhf"`
- `"[uhf]"`
- `":uhf"`
- `"[:uhf]"`
- `"a:uhf"`
- `"[a:uhf]"`

Only the default prefix has this many; for all other prefixes, there are only two ways of writing the key, `<prefix>:<reference>` and `[<prefix>:<reference>]`.  Since those are really the same UHF key, they should never both appear in the same object; if they do, it's not valid UHF.

## Back to keys

So, the `body` can be anything at all.   This is the UHF-approved place to put all your data, exactly as you'd format it for your own use, without any weird keys or required structures mixed in.  If there's no `body`, that doesn't necessarily mean there's no content, but it does mean that all the content is metadata, rather than resource data.

The `uhf` object is where prefixes are defined, and where you can memoize all those repetitive URLs you would otherwise want to use in your first, previous, next, last, etc, links.

The `head` array is where all other metadata is placed.

> **IMPORTANT**: no order should be implied by the order of items in the `head`.  All the detail about each entry in this list is contained within the object itself.

An example:

```json
{
  "uhf": {
    "u": "http://uhfs.org/uhf",
    "wh": "/warehouse/",
    "local": "/rels/",
    "ord": "/orders/"
  },
  "head": [
    { "rel": ["self", "[local:order]"], "uri": "[ord:523]" },
    { "rel": ["next", "[local:order]"], "uri": "[ord:524]" },
    { "rel": ["prev", "[local:order]"], "uri": "[ord:522]" },
    { "rel": ["warehouse"], "uri": "[wh:13]" },
    { "rel": ["warehouse"], "uri": "[wh:58]" },
    { "rel": ["warehouse"], "uri": "[wh:143]" },
    { "rel": ["invoice"], "uri": "/invoices/873" }
  ],
  "body": {
    "currency": "USD",
    "status": "shipped",
    "total": 10.20
  }
}
```

From the top, we can see:

- we have a `uhf` object, which is mandatory for UHF documents

    ...well, it could be called, in this document, something like `"[:uhf]"` or `"u:uhf"`, as described above.
- we have four CURIEs defined
- the mandatory default CURIE is defined with `u` (because it has the value of the URI to the UHF base specification).

    This just means that we could only use the `u`-prefixed form of keys for "u:uhf", "u:head", "u:body", "u:rel", or "u:uri".  Because UHF allows the document writer to choose the actual default prefix, it can always be something that doesn't conflict with any prefix the writer would prefer to use for their own URIs.  Also, we expect that mostly the document writer will just leave off the prefix and colon when writing UHF base keys, as they did in this example
- the `head` array of links and other affordances, *in which order is of no significance*
- the first entry in `head` happens to be a link with the "self" relation, meaning it is a link to this very document
- the other relation of that first entry is a SafeCURIE, expanding through the `local` prefix to "/rels/order", which is hopefully a dereferenceable URI based on the host we got this document from, telling us how to understand the `body` data.  Note that we couldn't use "local:order" without brackets, here, because it's not possible to tell mechanically if that's a URI or a CURIE, and so the `rel` array is defined as allowing only SafeCURIEs, IRIs (URIs), and registered link relations
- the `uri` of that first entry similarly uses the SafeCURIE format to avoid repeating "/orders/", questionable though that decision might be in this particular case
- the value of `body` happens to be an object with three keys, but UHF specifies nothing about the value of `body`, so this could be any valid JSON value, and the document would still be valid UHF.  Describing how to process the `body` should be handled at some URI found as a companion link relation to "self", and this document does have such a link, so visiting that would be our next step in understanding this UHF.


## Questions

1. Q: Why is a URI optional?

    Answers:
    1. A missing URI could be considered to be the empty relative URI, meaning this document.
    1. Some extensions, including at least one that UHF launches with, [template](/ext/template), define other keys that fill the role of having a link.
