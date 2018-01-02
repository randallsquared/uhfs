# UHF

This is a draft.   It was last updated Jan 1, 2018.

This site is the home of the UHF Hypermedia Format (UHF).

UHF is a deliberately simple format, introducing to JSON

1. three keys dividing a UHF document into three sections:
    - uhf
    - head
    - body
1. three keys that can only appear under `head`:
1. an extension / namespacing mechanism for additional capabilities

A UHF document can have a root-level key called `body`, and anything can go in here.   Your non-hypermedia API output, for example.

A UHF document can have a root-level key called `head`, which names an array of links that can each have a URI, a set of rels, and a title.

Every UHF document has a root-level key called `uhf`.  This is the core of the format.  The JSON object that `uhf` names is a mapping from "prefix" keys to partial URIs.

## CURIEs

Hypermedia formats inherently suffer to a degree from verbosity, since links, actions, and other affordances are inseparable from URIs.   CURIEs are a mechanism for shortening URIs, so that with a prefix definition of `g` meaning `http://www.example.com`, one can use `g:/about` instead of `http://www.example.com/about`.  (If you are familiar with the term "curies" from HAL, note that what HAL calls "curies" are really URI templates).

Every key that has meaning in UHF has a CURIE prefix and a CURIE reference. Allowed prefixes include all the keys in the object named by `uhf`.  Allowed references for keys are specified at the document retrieved at the prefix URI. Therefore, the prefix URI should always be a dereferenceable link, and will provide information about what references are defined by the extension specification.

The CURIE definition object at `uhf` may well have CURIEs defined that are not UHF, and not intended to define keys, since some values can also be CURIEs in some form.  This may seem ambiguous at first, but remember that the document producer does not need to discover what CURIEs they are using, and the document consumer only needs to match prefix URIs against local capabilities for keys they actually find: no one is ever in a position of having to distinguish UHF format CURIEs from other CURIEs.

Some last details about CURIEs: they can have surrounding square brackets (called SafeCURIEs, since you can use them in a place that might have a URI), and the prefix part is optional.  If the prefix part is missing, the colon is optional.  A missing prefix is considered to use the "default" prefix.  A format can define a default prefix, or a mechanism for determining that default prefix. UHF takes this option: the default prefix is the one that has a value of `http://uhfs.org/uhf`, right now.  There are some other details about versioning, but they won't matter until the specification is official and there's been a change to it.

In

```json
{ "uhf": { "a": "http:\/\/uhfs.org/uhf" } }
```

which is a useless document, but also the smallest valid UHF document, the default prefix is `a`.   The six core keys mentioned above use the default prefix, which means that the bare `uhf` key is fine, but it could also be written

```json
{ "a:uhf": { "a": "http:\/\/uhfs.org/uhf" } }
```
or

```json
{ "[a:uhf]": { "a": "http:\/\/uhfs.org/uhf" } }
```

or even

```json
{ ":uhf": { "a": "http:\/\/uhfs.org/uhf" } }
```

There are, in fact, six ways to write a valid key based on the default prefix `a` and a reference of `uhf`:


- `"uhf"`
- `"[uhf]"`
- `":uhf"`
- `"[:uhf]"`
- `"a:uhf"`
- `"[a:uhf]"`

Only the default key has this issue; for all other prefixes, there are only two ways of writing the key, `<prefix>:<reference>` and `[<prefix>:<reference>]`.  Since those are really the same UHF key, they should never both appear in the same object; if they do, it's not valid UHF.

## Back to keys

So, the `body` can be anything at all.   This is the UHF-approved place to put all your data, exactly as you'd format it for your own use, without any weird keys or required structures mixed in.  If there's no `body`, that doesn't necessarily mean there's no content, but it does mean that all the content is metadata, rather than data of a resource.

The `uhf` key is where prefixes are defined, and where you can memoize all those repetitive URLs you would otherwise want to use in your first, next, previous, last, etc, links.

The `head` key is where all other metadata is placed.

> A side note: what is data and what is metadata is a matter of opinion, basically, so it shouldn't surprise you that some information you might consider data could end up in `head`.  For example, the resource extension allows placing an entire request result inside a container in the `head`, which means there could be a `body` key under that container, but whatever is in that `body` key is **still** not UHF's business...


