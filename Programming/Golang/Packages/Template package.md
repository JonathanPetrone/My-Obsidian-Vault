In Go's template engine (`html/template` or `text/template`), **all plain text outside of the `{{ }}` tags is directly copied** into the final rendered output.

<"html">
    <"body">
        <"p">Items in your list:<"/p">
        {{ range .Items }}
            <"p">{{ . }}<"/p">
        {{ end }}
        <"p">Thank you for visiting!<"/p">
    <"/body">
<"/html">

- Any text not inside `{{ }}` is considered **verbatim text** and goes directly to the output.
- Template tags (`{{ }}`) are used to inject variables or logic, but everything outside of them is treated as simple text.

#### Nested template definitions
When parsing a template, another template may be defined and associated with the template being parsed. Template definitions must appear at the top level of the template, much like global variables in a Go program.

The syntax of such definitions is to surround each template declaration with a "define" and "end" action.

The define action names the template being created by providing a string constant. Here is a simple example:

{{define "T1"}}ONE{{end}}
{{define "T2"}}TWO{{end}}
{{define "T3"}}{{template "T1"}} {{template "T2"}}{{end}}
{{template "T3"}}

This defines two templates, T1 and T2, and a third T3 that invokes the other two when it is executed. Finally it invokes T3. If executed this template will produce the text

#### template.Must 
`template.Must` is a convenience function that **wraps a call to template parsing** and panics if there is an error during parsing. It is intended to be used during template initialization.

### When Should You Use It?
- When you are **absolutely sure** the template must exist and be valid for your application to run.
- In **initialization** code where panicking is acceptable if something is wrong.
- For **startup validation**, where you want to catch issues right away



