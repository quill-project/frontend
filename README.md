# frontend
Automatically generated bindings for all major frontend browser APIs.

These bindings were machine generated using [`quill-project/webidl`](https://github.com/quill-project/webidl) from IDL files from [the Chromium sources](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/).

They attempt to match their respective Javascript APIs as closely as possible, which sadly isn't always possible or desired:
- All names are converted to their respective Quill conventions - `Element.textContent` is converted to `Element::text_content`.
- Attributes are converted to functions, where getting the value corresponds to the name of the attribute and assigning a new value corresponds to `T::set_*`.
- Function overloading does not exist in Quill, in the case where multiple overloads exist each variant is annotated with the argument types it receives - `Document.createElement` becomes `Document::create_element_str` and `Document::create_element_str_any` (where only the latter takes an options object).
- Arguments or properties that take multiple possible types instead take `js::JsValue` - `T::as_js` and `T::from_js` is implemented for most common types.

### Example

This example waits for the page to fully load and then programmatically creates a new `<h1>` containing the text "Hello, world!":
```
mod example

use js::*
use frontend::*

val WINDOW: mut Window = global("window")
    |> expect("in normal frontend context")
    |> Window::from_js()

fun onload(_: mut Event) {
    val document: mut Document = WINDOW |> document()
    val body: mut Element = document
        |> query_selector("body") 
        |> expect("Should have <body> tag")
    val header: mut Element = document 
        |> create_element_str("h1")
    header |> set_text_content(Option::Some("Hello, world!" |> as_js()))
    body |> append_child(header |> as_mnode())
}

fun main() {
    WINDOW |> add_event_listener("load", Option::Some(onload), UNDEF)
}
```