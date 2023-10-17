---
title: Friendly syntax errors
description: Update to parsing functions to surface human-readable errors to the user
author: ~dozreg-toplud
status: Draft
type: Standards Track
category: Hoon
created: ~2023.10.17
---

<!--
  READ [UIP-1](./UIPs/UIP-0001.md) BEFORE USING THIS TEMPLATE!

  This is the suggested template for new UIPs. After you have filled in the requisite fields, please delete these comments.

  Note that an UIP number will be assigned by an editor. When opening a pull request to submit your UIP, please use an abbreviated title in the filename, `eip-draft_title_abbrev.md`.

  The title should be 44 characters or less. It should not repeat the UIP number in title, irrespective of the category.

  TODO: Remove this comment before submitting
-->

## Abstract

Syntax error messages are `tank`s pretty-printed in Dojo when a parsing caller (typically `scan`) fails to fully parse given input. At this moment those messages consist only of line and column numbers, pointing at the place in the parsed tape. The author proposes to make those printouts more verbose, so that the user could understand from the error message what the parser expected to receive and what it got instead.

## Motivation

Those changes would make it simpler to use Hoon parsers to process non-Hoon files. While simple error messages usually suffice for `.hoon` files thanks to Hoon's very regular and simple syntax, debugging parsing rules for e.g. binary files is a much harder task. During the development of Web Assembly interpreter in Hoon I had to write a parser for `.wasm` binary file and had to fix some bugs that would cause unexpected syntax errors. Not only were the laconic messages of practically no utility, but they would also treat `\0a` bytecode as a newline character, which is not what it means in the context of Wasm binary.

## Specification

# `edge` update

In addition to parsing result `edge` must also contain an error message if parsing failed:
```
+$  edge
  $:
    p=hair
    q=(each [p=* q=nail] p=tank)
  ==
```

# Standart library parsing rules

Each parsing rule should return an error message if it failed. Some examples:
```
++  fail                              ::  +fail is now a gate that takes a tank and returns a rule
  |=  a=tank
  |=(tub=nail [p=p.tub q=[%| a]])
::
++  just
  ~/  %just
  |=  daf=char
  ~/  %fun
  |=  tub=nail
  ^-  (like char)
  ?~  q.tub
    ((fail leaf+"want '{<daf>}', got end") tub)
  ?.  =(daf i.q.tub)
    ((fail leaf+"want '{<daf>}', got '{<i.q.tub>}'") tub)
  (next tub)
```

# Standart library parsing composers

Parsing composers should somehow combine the error messages, e.g. if `;~(pose (just 'a') (just 'b'))` failed to parse, we should get something like: `"want 'a' or 'b', got 'c'"`. That means that `edge` described above is not suitable, something else should be defined

## Rationale

<!--
  The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

TBD

## Backwards Compatibility

<!--

  This section is optional.

  All UIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their consequences. The UIP must explain how the author proposes to deal with these incompatibilities, and how developers can migrate the applications. This section may be omitted if the proposal does not introduce any backwards incompatibilities, but this section must be included if backward incompatibilities exist.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

No backward compatibility issues found.


## Security Considerations

<!--

  UIPs SHOULD contain a section that discusses the security implications/considerations relevant to the proposed change. Include information that might be important for security discussions, surfaces risks and can be used throughout the life-cycle of the proposal. E.g. include security-relevant design decisions, concerns, important discussions, implementation-specific guidance and pitfalls, an outline of threats and risks and how they are being addressed.

  The current placeholder is acceptable for a draft.

  TODO: Remove this comment before submitting
-->

Needs discussion.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
