---
name: Search the DNAnexus platform and paginate the whole result set
description: Use the find* system methods with glob/regex name matching and $and/$or filters on types, tags and properties, and drain every page with the limit/starting/next cursor without dropping or repeating results.
api: https://api.dnanexus.com
docs: https://documentation.dnanexus.com/developer/api/search
method: generated
generated: '2026-08-04'
grounded_in: https://documentation.dnanexus.com/developer/api/api-directory
operations:
  - /system/findDataObjects
  - /system/findExecutions
  - /system/findProjects
  - /system/describeDataObjects
  - /system/resolveDataObjects
  - /org-xxxx/findProjects
---

# Search DNAnexus and paginate correctly

## Pagination contract

Send `limit` (maximum results in this page) and, from the second call onward, `starting` set to the previous response's `next` value. When `next` comes back `null` the result set is exhausted. This is the same cursor contract on every `find*` method.

```
POST /system/findDataObjects
{"class": "file", "scope": {"project": "project-…"}, "limit": 1000}
→ {"results": [...], "next": "<cursor>"}

POST /system/findDataObjects
{"class": "file", "scope": {"project": "project-…"}, "limit": 1000, "starting": "<cursor>"}
→ {"results": [...], "next": null}      # done
```

Keep every other input field identical across pages. Changing a filter mid-walk invalidates the cursor's meaning.

## Filtering

- `class` — one of `record`, `file`, `applet`, `workflow`.
- `state` — `open`, `closing`, `closed`, `any` (default `any`).
- `visibility` — `hidden`, `visible` (default), `either`.
- `name` — an exact case-sensitive string, or a mapping with `glob` **or** `regexp` (PCRE), plus `flags: "i"` for case-insensitive regex. Glob wildcards are `*` (any run) and `?` (one char); `[]` brackets are not supported. Escape a literal `*`, `?` or `\` with a backslash — and remember JSON doubles the backslash, so `"\\*"` matches a literal `*`.
- `id` — up to 1000 object IDs; inaccessible IDs are silently omitted.
- `type`, `tags`, `properties` — a bare string for exact match, or boolean composition: `{"$and": ["gene", "coding"]}`, `{"$or": ["gene", "transcript"]}`, and nesting: `{"$or": ["gene", {"$and": ["transcript", "protein"]}]}`. For `properties`, `true` means the key exists with any value and `false` means it must be absent.
- `region` — a string or array; results are restricted to those regions.

## Steps

1. Scope the search as tightly as you can — `scope.project` in particular, or the same object appears once per accessible project.
2. Drain the cursor until `next` is `null`.
3. Batch-hydrate results with `POST /system/describeDataObjects` rather than one `describe` per object; this is the difference between one call and thousands against a 200-call/second budget.
4. Use `POST /system/resolveDataObjects` when you start from `project:/folder/name` paths instead of IDs.
5. `POST /system/findExecutions` and `POST /org-xxxx/findProjects` follow the identical cursor contract.

## Rules

- Ordering: data objects from the same project appear together, newest-modified first, unless overridden with `sortBy`; ties break by ascending ID.
- Never emulate offset pagination by re-issuing page 1 with a larger `limit` — follow the cursor.
- `RateLimitConditional` (429) means you exceeded 200 calls/second; back off, do not parallelize harder.
