# Code to Webflow Converter

**[Try the live tool](https://html-to-webflow-converter-4ne.pages.dev)** — no installation required.

Convert raw HTML, embedded CSS, and JavaScript into a Webflow clipboard payload, plus separate head and footer code. Supported elements and class styles can be edited in the Designer. Styles that remain custom CSS must be edited in code.

This is a migration aid, not a complete browser-to-Webflow translator. Check the imported result in the Designer and on the published site.

## What it converts

### Elements and attributes

- Maps standard HTML into Webflow elements, including headings, paragraphs, images, links, and line breaks.
- Preserves classes, IDs, `data-*` attributes, and ARIA attributes in the generated payload.
- Exports `hidden` as a custom attribute so scripts can toggle it after publishing.
- Packages SVG, video, audio, canvas, and iframe elements as HTML embeds. Their contents remain code, rather than individually editable Designer elements.
- Uses stable class IDs derived from class names. This makes generated IDs consistent across conversions; it does **not** guarantee that Webflow will never rename or duplicate classes in an existing project.

### Native class styles

The converter handles:

- Individual class selectors, such as `.card`.
- Comma-separated individual class selectors, such as `.card, .panel`.
- `:hover`, `:active`, and `:focus` states on individual classes.
- Exact `max-width` queries at **991px**, **767px**, and **479px**, mapped to Webflow breakpoint variants.
- Font and inset shorthand expansion, plus repeated declaration cleanup so the last declaration for a property wins.

Input CSS is parsed in an isolated document so it cannot restyle the converter itself.

### Custom CSS and head dependencies

The head export retains rules and declarations that are not converted to native styles, including:

- Compound, descendant, attribute, and other complex selectors.
- Unsupported pseudo-classes and pseudo-elements.
- Other media queries and conditional rules.
- CSS variable definitions and declarations containing `url()`.
- Font-family assignments, with resolvable CSS variables replaced by their declared values.
- Rules for embedded graphics and classes absent from the initial HTML, including elements created later by JavaScript.

Stylesheet and preconnect links from the input head are included in the head export. Linked external stylesheets are **not** fetched or converted into Designer styles.

The current build also includes compatibility handling for isolation resets and selected cloud-animation classes. It does not perform general cascade-equivalence analysis for arbitrary pages.

### JavaScript

Inline scripts and external script references are extracted in their original document order into the footer export. Script bodies remain unchanged.

The converter does not rewrite application logic, remove animation assets, bundle dependencies, or automatically deduplicate scripts already loaded by Webflow. Moving scripts from the source head to the footer can affect code that depends on its original location or execution timing.

## How to use it

1. Paste a complete HTML document or HTML fragment into **Paste Raw HTML**. Include any `<style>` and `<script>` blocks you want converted or extracted.
2. Click **Convert to Webflow** and review the conversion notes.
3. Click **Copy Converted HTML**, select a suitable container in the Webflow Designer, and paste with `Cmd+V` or `Ctrl+V`.
4. Copy the **head code** into the page's **Inside `<head>` tag** field. This output can contain dependency links as well as custom CSS.
5. Copy the **JavaScript** into **Before `</body>` tag**. Check for libraries already provided by your project before adding duplicate imports.
6. Publish and verify layout, typography, responsive behavior, interactive states, and animation hooks.

The converter reports when a head or JavaScript output exceeds **50,000 characters**. It does not automatically split or host oversized output.

### Native Webflow fonts

To supply fonts through Webflow's font settings instead of exported Google Fonts links, add this to your source document's head:

```html
<meta name="webflow-font-provider" content="native">
```

With this marker, the converter omits Google Fonts stylesheet and preconnect links from the head export. Configure the required font families, weights, and italics in Webflow yourself. Font-family assignments still remain in the exported custom CSS.

### Full original CSS fallback

The **Keep full original CSS as a fallback** option is off by default. Enable it to retain the original inline stylesheets in the head export while also generating native class styles.

This preserves the original inline CSS rule order, but those rules can override edits made in the Designer. It does not guarantee that converted markup will behave exactly like the source document.

## Limitations

- Webflow's clipboard format is undocumented. Successful conversion does not guarantee successful import or an identical published result.
- Imported styles can interact with existing tag styles, classes, and project settings. Check typography and element defaults, especially headings and blockquotes.
- Native and custom styles are exported separately. This can change cascade relationships; complex combinations of classes and responsive rules need verification.
- CSS nesting and arbitrary breakpoint systems are not translated into native Designer styles.
- Embed internals, custom CSS, and extracted JavaScript remain code-based.
- Image URLs are preserved; assets are not uploaded to the Webflow asset library.
- Inline `style` attributes are not converted into native class styles.
- Source `<html>` and `<body>` classes are not applied to Webflow's document root. Put important component styling on an imported wrapper.
- Text nodes are trimmed, which can affect spaces between inline elements.
- Form conversion is limited. The current parser substitutes divs for `input`, `textarea`, and `hr`; it does not generate complete native Webflow forms.
- The converter does not enforce Client-First naming or automatically turn arbitrary class combinations into a managed combo-class system.

## Diagnostics

### Conversion notes

The output reports oversized code, conditional media queries retained as custom CSS, native-font configuration, and certain embedded restoration payloads. These notes are guidance, not a complete validation of the generated page.

### Test Native Snippet

Copies a small reference clipboard payload to help troubleshoot paste failures. If it pastes but your converted output does not, investigate the generated payload. If neither pastes, check clipboard permissions and the Webflow session as well as payload compatibility.

### Bug Report Generator

Packages your input, generated output, a clipboard example copied from Webflow, and any console errors you provide.

1. Build and copy a small comparable element in Webflow.
2. Paste it into **Paste Webflow's Version** to capture its clipboard JSON.
3. Add relevant console errors.
4. Click **Generate & Copy Bug Report**.

Review the report before sharing it: source code and clipboard content may contain private text, URLs, or other project data.

## Tutorial and guide

[Read the guide on bydan.us](https://www.bydan.us/resources/html-to-webflow-converter)

[![Watch the tutorial](https://img.youtube.com/vi/3NaCTm4DuB8/maxresdefault.jpg)](https://youtu.be/3NaCTm4DuB8)

## License

Released under the **MIT License**. See the repository's license file for its terms.

## Disclaimer

Created by [Dan Design](https://bydan.us). This is an independent tool and is not affiliated with or endorsed by Webflow.

Provided as-is, without warranty. Keep a backup and test changes before using them on an important page. Clipboard-format changes may require updates to the converter.
