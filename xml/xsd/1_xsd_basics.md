# XML Schema Definition(XSD)

* XSD describes the structure of the XML document.

## XSD vs DTD

* XSD is written in XML
* XSD supports datatypes and namespaces
* XSD is extensible

## XSD Schema

XML schema contains

* namespace of the target XML document
* the elements and attributes present in a XML document.
* number of child elements
* data types of elements and attributes
* default and fixed values for elements and attributes as well as restrictions on those values.

## Example

```XML
<?xml version="1.0"?>
<!-- We mention the 
1. namespace of the schema element
2. target and default namespace of the target XML document
-->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
targetNamespace="https://www.w3schools.com"
xmlns="https://www.w3schools.com"
elementFormDefault="qualified">

<!-- complexType for nodes contains child nodes -->
<xs:element name="note">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="to" type="xs:string"/>
      <xs:element name="from" type="xs:string"/>
      <xs:element name="heading" type="xs:string"/>
      <xs:element name="body" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>

</xs:schema>
```

* Reference to XSD in XML

```XML
<?xml version="1.0"?>

<!-- 1. Define the default namespace for note. This has to be defined
because XSD has the xmlns attribute defined. 
2. Define the XML schema instance.
3. SchemaLocation contains the namespace and the XSD file URL
-->
<note
xmlns="https://www.w3schools.com"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="https://www.w3schools.com/xml note.xsd">
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

* `xsi:schemaLocation="namespace xsd_location_uri"`. If no namespace is present, then `xsi:noNamespaceSchemaLocation="xsd_location_uri"` is used.

## `<schema>` element

`<schema>` is the root element of the XSD document. It contains the following attributes

* `xmlns:xs=""http://www.w3.org/2001/XMLSchema"` - This defines the namespace for this schema document and the namespace prefix is `xs`

* `targetNamespace="https://www.example.com"` -  is an attribute on the `xs:schema` root element of an XSD which specifies the namespace of the root element of the XML document instances the XSD is intended to govern. It must match the **default or explicit namespace of those XML document's** root element.

* `xmlns="https://www.example.com"` - Default namespace of the target XML document.

* `elementFormDefault="qualified"` - Elements of the target XML document must be namespace qualified.

**!!ExtremelyImportant!!**: All types defined in XSD using `simpleType` or `complexType` tags belong to the namespace defined by `targetNamespace` or `xmlns` defined in the schema file. If none of the two above mentioned attributes are defined in the schema file, then the types defined in the schema file do not belong to any namespace. This will come handy when learning about `xs:include` and `xs:import`

## Global elements

Global elements are elements that are immediate children of the `<xs:schema>` element. Local elements are elements nested within other elements.

## XSD simple elements

* Simple element is an element that only contains some data.
* Simple element cannot have other elements or any attributes inside it.

```XML
<!-- simple element in an empty xml element -->
<xs:element name="node_name" type="node_type" />
```

* XML schema has many built-in types. Simple types are `xs:boolean`, `xs:integer`, `xs:decimal`, `xs:string`, `xs:date`, `xs:time`

* Simple elements can have default or fixed values.

```XML
<xs:element name="color" type="xs:string" default="red"/>

<!-- For fixed element we cannot assign a value -->
<xs:element name="color" type="xs:string" fixed="red"/>
```

* XML element with attributes is considered to be a **complex element/type**.

* We can control the number of occurrences of an element using the `maxOccurs` and `minOccurs` attributes. [By default both are set to 1, which makes the simple element required to be present exactly once.](https://stackoverflow.com/a/33686479/2369053)

## XSD attributes

* Attributes in XML document can hold only data of the simple types.

```XML
<!-- attribute in an XSD is defined as -->
<xs:attribute name="attr_name" type="one_of_simple_types_or_derived_from_simple_type" />
```

* Attributes can also have default and fixed values similar to simple elements.

```XML
<xs:attribute name="lang" type="xs:string" default="EN"/>

<!-- we cannot assign some other value. Like const -->
<xs:attribute name="lang" type="xs:string" fixed="EN"/>
```

* Attributes can be marked as **optional which is default** and as required.

```XML
<xs:attribute name="lang" type="xs:string" use="required"/>
```

## Restriction on values

* Simple elements and attributes can have custom types that are derived from simple types with restrictions or extensions added.

```XML
<!-- restrict age value between 0 and 120 -->
<xs:element name="age">
    <xs:simpleType>
        <xs:restriction base="xs:integer">
            <xs:minInclusive value="0"/>
            <xs:maxInclusive value="120"/>
        </xs:restriction>
    </xs:simpleType>
</xs:element>

```

* If we want to define reusable restrictions, then we can use the below syntax

```XML
<!-- any other simple element can use its type as the carType -->
<xs:element name="car" type="carType"/>

<!-- This defines a custom simple type -->
<xs:simpleType name="carType">
    <xs:restriction base="xs:string">
        <xs:enumeration value="Audi"/>
        <xs:enumeration value="Golf"/>
        <xs:enumeration value="BMW"/>
    </xs:restriction>
</xs:simpleType>
```

* Documentation of wide range of restrictions on different data types can be [found here](https://www.w3schools.com/xml/schema_facets.asp)

## Builtin data types

* [String based data types](https://www.w3schools.com/xml/schema_dtypes_string.asp) like `normalizedString`, `token` etc

* [Date and time data types](https://www.w3schools.com/xml/schema_dtypes_date.asp)

* [Numeric data types](https://www.w3schools.com/xml/schema_dtypes_numeric.asp)

* [Other data types](https://www.w3schools.com/xml/schema_dtypes_misc.asp)

---

## References

* [XSD](https://www.w3schools.com/xml/schema_intro.asp)
* [XSD reference](https://www.w3schools.com/xml/schema_elements_ref.asp)
* [xmlns, xmlns:xsi, xsi:schemaLocation, and targetNamespace?](https://stackoverflow.com/questions/34202967/xmlns-xmlnsxsi-xsischemalocation-and-targetnamespace)
