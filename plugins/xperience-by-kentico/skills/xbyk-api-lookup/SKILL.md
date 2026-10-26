---
name: xbyk-api-lookup
description: Look up Xperience by Kentico (XbyK) API types, methods, interfaces, services, and namespaces directly from the official API reference (api-reference.kentico.com). WHEN: "find XbyK API", "look up Kentico class", "find XbyK method signature", "search Xperience by Kentico API reference", "ContentRetriever API", "IContentQueryExecutor", "ContentItemQueryBuilder", "IContentQueryResultMapper", "PageBuilder API", "widget property model", "IWidgetProperties", "ContentItemData", "IWebPageDataContextRetriever", "IWebsiteChannelContext", "IPageBuilderDataContextRetriever", "IContentItemCodeNameProvider", "IContentItemAssetUrlRetriever", "find Kentico interface", "find Kentico namespace", "XbyK service registration", "inject XbyK service", "DI service XbyK", "CMS namespace", "headless XbyK API". NOT for conceptual/how-to guides (use Kentico's documentation site).
---

# Xperience by Kentico API Lookup

XbyK is closed source, but the official API reference at `api-reference.kentico.com` is
generated from the public API surface — it's fast, accurate, and covers every publicly
supported type, method, and namespace. Always query it directly.

> **Scope:** this skill is strictly for **API reference lookups** — resolving the exact
> shape of a type, member, or namespace. It does not cover conceptual guides, tutorials, or
> "how do I configure X" walkthroughs; use Kentico's documentation site for those.

## Context Budget Rule

Raw API reference pages are large and full of navigation/boilerplate. If your environment
supports delegating work to a background/exploration agent, fetch pages through that agent
and have it return only the distilled fields listed below — don't let full page HTML flood
your main context.

## Lookup Procedure

### Step 1 — Fetch the API Reference page

URL pattern:

```
https://api-reference.kentico.com/api/{Namespace}.html              ← all types in namespace
https://api-reference.kentico.com/api/{Namespace}.{TypeName}.html   ← specific type
```

Entry point to discover all top-level namespaces: `https://api-reference.kentico.com/api/CMS.html`

**To look up a specific type**, fetch its type page and extract only:
- Type declaration (class/interface/enum) with its namespace
- Base class and implemented interfaces
- All public constructors with parameter names and types
- All public methods: name, parameters (name + type), return type
- All public properties: name and type
- Any important remarks or exceptions

**To discover types in a namespace**, fetch the namespace page and extract a flat list of
type names (classes, interfaces, enums) defined in that namespace.

Common type URL examples:

```
https://api-reference.kentico.com/api/CMS.ContentEngine.IContentQueryExecutor.html
https://api-reference.kentico.com/api/CMS.ContentEngine.ContentItemQueryBuilder.html
https://api-reference.kentico.com/api/CMS.Websites.IWebPageDataContextRetriever.html
https://api-reference.kentico.com/api/Kentico.PageBuilder.Web.Mvc.html
```

## Common Lookup Patterns

### Finding a service / interface for DI injection

1. Look up the interface directly in the API reference, e.g. `CMS.ContentEngine.I<TypeName>.html`.
2. Confirm the constructor/method signatures match what you intend to call.
3. Check the same namespace's listing for the concrete implementation type (typically the
   non-`I`-prefixed class) to confirm it implements the interface you're injecting.

### Finding extension methods

Check the namespace listing (Step 1, "discover types in a namespace") for static extension
classes (e.g., `ContentRetrieverExtensions`, `QueryBuilderExtensions`).

## Quality Check

Before writing code that calls an XbyK API:
- [ ] Confirmed the type exists in the API reference
- [ ] Confirmed the method signature matches (parameter names and types)
- [ ] Confirmed DI registration name matches the injected interface
- [ ] Checked for nullable annotations or required parameters
