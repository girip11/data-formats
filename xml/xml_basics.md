# XML

XML - Extensible Markup Language

## Syntax

```XML
<!-- XML prolog, optional but recommended -->
<?xml version="1.0" encoding="UTF-8"?>
<!-- This is a comment in XML. It can span multiple lines. -->
<!-- A document should have only one root element with any name-->
<root>
  <!--child is referred to as xml element or node or tag -->
  <child>
    <!--Attributes are optional and elements can have many attributes-->
    <subchildnoattr>content</subchildnoattr>
    <!--Empty elements don't have any content-->
    <emptychild attribute="value" another-attribute="value"/>
    <subchild attribute="value">content</subchild>
  </child>
  <!-- element names are case sensitive -->
  <emptychild />
  <!-- xml preservers whitespace -->
  <anotherchild>
    SomeContent
  </anotherchild>
  <!-- another and yetanotherchild are not same due to whitespaces -->
  <yetanotherchild>somecontent</yetanotherchild>
</root>
```

## Syntax Rules

* XML documents must contain exactly one root element.
* XML prolog if present, should be the first line in the document.
* All xml elements must be closed with either `</element>` or `/>`
* XML element names are case sensitive.
* XML supports nesting of elements
* XML attribute values must always be quoted usin `'` or `"` unlike HTML
* Characters like `<, >, &, ', "` should be escaped to `&lt;  &gt; &amp; &apos; &quot;` when part of the content just like HTML.
* `\n` is the newline character in XML.
* XML elements can be **extended without breaking**. New child nodes can be added to existing nodes without any issues.

## XML elements naming convention

* Use any on among the lowercase, UPPERCASE, snake_case, PascalCase or camelCase for the XML tags. But be consistent.
* Use short, simple but descriptive names.
* Avoid `- . :` in the names.

## XML attributes

* Attributes are not easily expandable like XML tags
* Attributes cannot be nested.
* Attributes can hold values of simple types like string, integer, decimal, boolean, date, time.
* A complex value can be stored as csv string in the attribute and then later parsed. **But the csv structure cannot be validated using as XSD**.
* To store multiple values in attributes, we have to use some sort of encoding like csv, or space separated.

**RECOMMENDATION:** Data should be stored as child nodes, while metadata of a node should be stored in its attributes.

## XML Namespaces

* Two XML elements with same name but different data and nested elements, results in parsing conflict.
* Name conflicts can be resolved using namespaces.
* Namespace declared using the **xmlns** attribute. Syntax `xmlns:prefix="URI"`
* URI is used to give the namespace a unique name. Common practice is to assign URI a webpage URL that contains documentation on that namespace.
* Namespaces can also be defined at the root element.
* When a namespace is defined for an element, all child elements with the same prefix are associated with the same namespace.

```XML
<!-- <root xmlns:h="http://www.w3.org/TR/html4/"
xmlns:f="https://www.w3schools.com/furniture"> -->
<root>
    <h:table xmlns:h="http://www.w3.org/TR/html5/">
        <h:tr>
            <h:td>Apples</h:td>
            <h:td>Bananas</h:td>
        </h:tr>
    </h:table>

    <f:table xmlns:f="https://www.w3schools.com/furniture">
        <f:name>African Coffee Table</f:name>
        <f:width>80</f:width>
        <f:length>120</f:length>
    </f:table>
</root>
```

* Default namespace is defined using the syntax `xmlns="namespaceURI"`. With this we don't need to prefix all the elements.

```XML
<table xmlns="http://www.w3.org/TR/html4/">
  <tr>
    <td>Apples</td>
    <td>Bananas</td>
  </tr>
</table>

<table xmlns="https://www.w3schools.com/furniture">
  <name>African Coffee Table</name>
  <width>80</width>
  <length>120</length>
</table>
```

* Namespaces are used in **XSLT (Extensible Stylesheet Language Transformations)**. XSLT is used to convert XML to other document formats like HTML.

* Thus XSLT can be used to style an XML document(by converting to HTML and applying styling)

## XML DOM

* XML DOM - **Document Object Model**.
* XML text is parsed as a **tree-structure** in an XML DOM object. Using this object we can access and manipulate XML elements.

## XML well-formedness

* [XML Validation online tool](https://www.xmlvalidation.com/) can be used to check the well-formedness of an XML document.

## XML validation w.r.t to Schema

* **Document Type Definitions(DTDs)** can be used to check if an XML document adheres to a schema.
* DTD is replaced with **XSD(XML Schema Definitions)**. XSDs support namespaces.
* Using XSD we can validate the schema of the XML document.

---

## References

* [XML](https://learnxinyminutes.com/docs/xml/)
* [XML Syntax rules](https://www.w3schools.com/xml/xml_syntax.asp)
* [XML tags naming convention](https://www.w3schools.com/xml/xml_elements.asp)
* [XML DTD](https://www.w3schools.com/xml/xml_dtd.asp)
