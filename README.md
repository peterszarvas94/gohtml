# GoHTML - HTML formatter for Go

[![wercker status](https://app.wercker.com/status/926cf3edc004271539be40d705d037bd/s "wercker status")](https://app.wercker.com/project/bykey/926cf3edc004271539be40d705d037bd)
[![GoDoc](http://godoc.org/github.com/peterszarvas94/gohtml?status.png)](http://godoc.org/github.com/peterszarvas94/gohtml)

GoHTML is an HTML formatter for [Go](http://golang.org/). You can format HTML source codes by using this package.

## Install

```
go get -u github.com/peterszarvas94/gohtml
```

## Example

Example Go source code:

```go
package main

import (
	"fmt"

	"github.com/peterszarvas94/gohtml"
)

func main() {
	h := `<!DOCTYPE html><html><head><title>This is a title.</title><script type="text/javascript">
alert('aaa');
if (0 < 1) {
	alert('bbb');
}
</script><style type="text/css">
body {font-size: 14px;}
h1 {
	font-size: 16px;
	font-weight: bold;
}
</style></head><body><form><input type="name"><p>AAA<br>BBB></p></form><!-- This is a comment. --></body></html>`
	fmt.Println(gohtml.Format(h))
}
```

Output:

```html
<!DOCTYPE html>
<html>
	<head>
		<title>This is a title.</title>
		<script type="text/javascript">
			alert("aaa");
			if (0 < 1) {
				alert("bbb");
			}
		</script>
		<style type="text/css">
			body {
				font-size: 14px;
			}
			h1 {
				font-size: 16px;
				font-weight: bold;
			}
		</style>
	</head>
	<body>
		<form>
			<input type="name" />
			<p>
				AAA
				<br />
				BBB>
			</p>
		</form>
		<!-- This is a comment. -->
	</body>
</html>
```

## Output Formatted HTML with Line No

You can output formatted HTML source codes with line no by calling `FormatWithLineNo`:

```go
package main

import (
	"fmt"

	"github.com/peterszarvas94/gohtml"
)

func main() {
	h := `<!DOCTYPE html><html><head><title>This is a title.</title><script type="text/javascript">
alert('aaa');
if (0 < 1) {
	alert('bbb');
}
</script><style type="text/css">
body {font-size: 14px;}
h1 {
	font-size: 16px;
	font-weight: bold;
}
</style></head><body><form><input type="name"><p>AAA<br>BBB></p></form><!-- This is a comment. --></body></html>`
	fmt.Println(gohtml.FormatWithLineNo(h))
}
```

Output:

```sh
 1  <!DOCTYPE html>
 2  <html>
 3    <head>
 4      <title>
 5        This is a title.
 6      </title>
 7      <script type="text/javascript">
 8        alert('aaa');
 9        if (0 < 1) {
10        	alert('bbb');
11        }
12      </script>
13      <style type="text/css">
14        body {font-size: 14px;}
15        h1 {
16        	font-size: 16px;
17        	font-weight: bold;
18        }
19      </style>
20    </head>
21    <body>
22      <form>
23        <input type="name">
24        <p>
25          AAA
26          <br>
27          BBB>
28        </p>
29      </form>
30      <!-- This is a comment. -->
31    </body>
32  </html>
```

## Format Go html/template Package's Template's Execute Result

You can format [Go html/template package](http://golang.org/pkg/html/template/)'s template's execute result by passing `Writer` to the `tpl.Execute`:

```go
package main

import (
	"os"
	"text/template"

	"github.com/peterszarvas94/gohtml"
)

func main() {

	tpl, err := template.New("test").Parse("<html><head></head><body>{{.Msg}}</body></html>")

	if err != nil {
		panic(err)
	}

	data := map[string]interface{}{"Msg": "Hello!"}

	err = tpl.Execute(gohtml.NewWriter(os.Stdout), data)

	if err != nil {
		panic(err)
	}
}
```

Output:

```html
<html>
	<head> </head>
	<body>
		Hello!
	</body>
</html>
```

## Basic configuration

### Condense and inline tags

```go
// Enable condensing a tag with only inline children onto a single line, or
// completely inlining it with sibling nodes.
// Tags to be treated as inline can be set in `InlineTags`.
// Only inline tags will be completely inlined, while other condensable tags
// will be given their own dedicated (single) line.
gohtml.Condense = false

// Tags that are considered inline tags.
// Note: Text nodes are always considered to be inline
gohtml.InlineTags = map[string]bool{
	"a":      true,
	"code":   true,
	"em":     true,
	"span":   true,
	"strong": true,
}

// Maximum length of an opening inline tag before it's un-inlined
gohtml.InlineTagMaxLength = 40
```

## Indentation string

```go
// The indent string, defaults to two spaces
gohtml.IndentString = "  "
```

## Docs

- [GoDoc](https://godoc.org/github.com/peterszarvas94/gohtml)
