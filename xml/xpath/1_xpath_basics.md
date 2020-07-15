# XPath basics

XPath - XML Path language. XPath can be used to traverse through XML and HTML tree using path like (unix) syntax.

## XPath node types

![XPath tree](xpath_tree.png)

XPath has a implicit root node that is the root of all the elements in the XML/HTML document(This root is represented using first `/` similar to the way root directory is referenced in a unix path).

Most common XPath node types are (remember as CEAT)

* **C**omment node
* **E**lement node
* **A**ttribute node - Contains the attribute value.
* **T**ext node

Other nodes types are

* Namespace node
* processing-instruction node
* document node

## XPath Location step

XPath of the form `/html/head/title` are referred to as **location paths**.
Location path of the form `/step/step/...` can be split into multiple **location steps**. Syntax of each location step is `axisname::nodetest[predicate]`. Each step consists of

* an **axis** that defines the tree-relationship between the selected nodes and the current node
* a **node-test** that identifies a node within an axis
* zero or more **predicates** to further refine the selected node-set)

## Axis and node test

* `//title` - Select all nodes named `title` from anywhere in the hierarchy from the root node.

> The **axis** defines where in the tree the **node test** should be applied and the nodes that match the node test will be returned as a result.

```xpath
<!-- descendant-or-self and child are axes in this path -->
<!-- node and title are node tests in this path -->
/descendant-or-self::node()/child::title
```

* Axis defines the direction on which the node test needs to be applied. In a tree structure, we can move upwards (parents and ancestors), downwards (child, descendants) and on the same tree level(siblings).

```text
# Reference: https://www.w3.org/TR/2017/REC-xpath-31-20170321/#axes

ForwardAxis     ::=     ("child" "::")
| ("descendant" "::")
| ("attribute" "::")
| ("self" "::")
| ("descendant-or-self" "::")
| ("following-sibling" "::")
| ("following" "::")
| ("namespace" "::")  

ReverseAxis     ::=     ("parent" "::")
| ("ancestor" "::")
| ("preceding-sibling" "::")
| ("preceding" "::")
| ("ancestor-or-self" "::")  
```

## Node selection

Path starting with single `/` is considered as absolute path and starts from XPath root element.

* `//` - Selects nodes in the document from the current node that match the selection.
* `.` - Represents current node
* `..` - represents the parent node

Examples

* `/html/head` - Select `head` node under `html` node
* `//h2/a` - Selects **all** `a` nodes under all of the  `h2` nodes in the document(current node is h2).
* `//script` - Selects all `script` nodes anywhere within the document(implicitly root is assumed as the starting point).
* `/html/body//script` - Selects all `script` nodes anywhere within the `body` node.

## Node test expression

* `//comment()` - Selects only comment nodes.
* `//node()` - Selects any kind of node in the tree.
* `//text()` - Selects only text nodes
* `//attribute()` - Selects all the attribute nodes which contains the attribute values.
* `//element()` - Selects all the elements in the document along with the body of each element.
* `//*` -  Selects all nodes, except comment and text nodes.
* `//@*` - Selects any attribute node

## Predicate

* Predicate is defined inside `[]`. This is used to select elements defined before the predicate, if the predicate is satisfied.

* `//li[position() = 1]` or `//li[1]` - Selects the first `li` node in each of the `ul` or `ol` lists in the document.
* In case of lists, XPath position index **starts from 1 and not 0**.

* Relational operators `<, >, =, !=, <=, >=` can be used in predicates.
* Logical operators `or`, `and` are availabe.
* Arithmetic operators `+, -, *, mod(or)%, div` are available.

Examples

* `//li[position()%2=0]` -  Selects the `li` elements at even positions.
* `//li[a]` - Selects the `li` elements which enclose an `a` element.
* `//li[a or h2]`  Selects the `li` elements which enclose either `a` or `h2`
* `//li[ a [ text() = "link"]]` or `//li[a/text()="link"]` - Selects the `li` elements which enclose an `a` element whose text is "link". Here the predicates are read from innermost one to outer most one to make sense.
* `//li[last()]`  Selects the last `li` element in each of ordered and unordered lists in the document.
* `//p[@*]` - Selects all `p` nodes with atleast 1 attribute.

Comparisons can be used directly against the node instead of `node/text()`. Assume book contains title, year and price.

* `/bookstore/book[price/text() > 35]/price`, `/bookstore/book[price > 35]/price`, `/bookstore/book/price[text() > 35]` all return prices of book that costs more than 35. Notice the predicate `price > 35` where node is directly compared.
* `/bookstore/book[title="Harry Potter"]`, `/bookstore/book[title [ text() = "Harry Potter"]]`, `/bookstore/book[title/text() = "Harry Potter"]`

## Union of XPaths

* Multiple XPaths can be joined using `|` union. This can be used to select nodes from several paths.
* `//a | //h2` - Selects all `a` and `h2` nodes in the document.

## Attribute access

* Node attributes are accessed using the syntax `@attribute_name`

* `//a[starts-with(@href, "https")]` - Select all `a` nodes whose `href` attribute value starts with **https**
* `//a[@href="https://scrapy.org"]` - Selects all `a` nodes whose `href` value matches the given URL.
* `//a/@href` - Selects all the href attribute values
* `//li[@id]` - Select all `li` nodes with an id attribute

## Using Axis

First select the node(`h1`, `div[@id="footer"]`) from where you want to move your axis.

* `//h1/following-sibling::p[1]` - Selects the first paragraph node following `h1` node

* `//div[@id="footer"]/preceding-sibling::text()[1]` - Selects the text node that is an immediate preceding sibling(indicated by 1) to the `div` with `id="footer"`. If we use 2, then it will select the text node(if any) which is the second sibling preceding it.

* `//*[p/text()="Footer text"]` - Selects all the parent nodes(any node) which contain a `p` node with text **Footer text**. `//p[ text()="Footer text" ]/..` xpath also does the same. Notice `..` is used to fetch the parent. `//p[text()="Footer text"]/parent::node()`, `//p[text()="Footer text"]/parent::element()` also achieve the same results.

---

## References

* [An Introduction to XPath](https://blog.scrapinghub.com/2016/10/27/an-introduction-to-xpath-with-examples)
* [Xpath](https://scrapinghub.github.io/xpath-playground/?utm_campaign=BRA&utm_activity=BLO&utm_medium=ORG&utm_source=HUB&utm_content=BLP)
* [XPath Axes](https://www.w3schools.com/xml/xpath_axes.asp)
