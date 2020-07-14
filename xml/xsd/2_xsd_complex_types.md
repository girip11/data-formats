# XSD Complex Elements and Types

Complex elements can be

* Empty elements with attributes
* Elements with nested elements and/or attributes
* Elements with attributes and text.
* Elements with text data and nested elements and/or attributes.

```XML
<employee>
    <firstname>John</firstname>
    <lastname>Smith</lastname>
</employee>

<xs:element name="employee">
    <!-- This complex type is defined within employee element only  -->
    <xs:complexType>
        <!-- sequence indicator -->
        <xs:sequence>
            <xs:element name="firstname" type="xs:string"/>
            <xs:element name="lastname" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

* Alternate syntax where the complex type definition can be placed separately from its usage.

```XML
<!-- This complex type can be referred at multiple places.-->

<xs:element name="employee" type="personInfo" />
<xs:element name="student" type="personInfo" />

<xs:complexType name="personInfo">
    <!-- sequence indicator -->
    <xs:sequence>
        <xs:element name="firstname" type="xs:string"/>
        <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
</xs:complexType>
```

* When elements are placed inside `xs:sequence`, those elements should appear in the same order as in schema.

## ComplexType inheritance

* `complexContent`, `extension`, elements are used when inheriting from another complex type.

```XML
<xs:element name="employee" type="fullpersoninfo"/>

<xs:complexType name="personinfo">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
  </xs:sequence>
</xs:complexType>

<xs:complexType name="fullpersoninfo">
  <xs:complexContent>
    <xs:extension base="personinfo">
      <xs:sequence>
        <xs:element name="address" type="xs:string"/>
        <xs:element name="city" type="xs:string"/>
        <xs:element name="country" type="xs:string"/>
      </xs:sequence>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
```

## Empty Complex elements

* `<product prodid="1345" />` can be represented in XSD as

```XML
<xs:element name="product">
    <xs:complexType>
        <xs:attribute name="prodid" type="xs:positiveInteger" />
    </xs:complexType>
</xs:element>
```

* If we need to use any restriction or extension on the attribute value, then the elements `xs:complexContent` and `xs:restriction`/`xs:extension` will be used.

```XML
<Tree id="100"></Tree>

<xs:element name="Tree">
    <xs:complexType>
        <xs:complexContent>
            <xs:restriction base="xs:integer">
                <xs:attribute name="id" type="xs:integer"/>
            </xs:restriction>
        </xs:complexContent>
    </xs:complexType>
</xs:element>
```

* Reusable representation of an empty element where attribute definitions can be reused.

```XML
<xs:element name="product" type="prodtype"/>

<xs:complexType name="prodtype">
  <xs:attribute name="prodid" type="xs:positiveInteger"/>
</xs:complexType>
```

## Complex element with only nested elements

```XML
<xs:element name="employee" type="personinfo"/>

<xs:complexType name="personinfo">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
  </xs:sequence>
  <!-- attributes will be present here -->
  <xs:attribute name = 'empid' type = 'xs:positiveInteger'/>
</xs:complexType>
```

## Complex element with text only

* Example - `<shoesize country="france">35</shoesize>`

* Under `xs:complexType` we will have `xs:simpleContent` followed by either `xs:extension` or `xs:restriction` depending on whether you need to extend or restrict a basetype.

```XML

<xs:element name="shoesize" type="shoesizetype" />

<xs:complexType name="shoesizetype">
    <xs:simpleContent>
        <xs:extension base="xs:integer">
            <xs:attribute name="country" type="xs:string" />
        </xs:extension>
    </xs:simpleContent>
</xs:complexType>
```

## Complex elements with text and child elements(mixed)

* `mixed` attribute of `xs:complexType` should be set to true in this case

```XML
<!-- Sample XML -->
<letter>
    Dear Mr. <name>John Smith</name>.
    Your order <orderid>1032</orderid>
    will be shipped on <shipdate>2001-07-13</shipdate>.
</letter>

<!-- Schema -->
<xs:element name="letter" type="lettertype"/>
<xs:complexType name="lettertype" mixed="true">
    <xs:sequence>
        <xs:element name="name" type="xs:string"/>
        <xs:element name="orderid" type="xs:positiveInteger"/>
        <xs:element name="shipdate" type="xs:date"/>
    </xs:sequence>
</xs:complexType>
```

**NOTE**: These definitions of different types of complex elements form the building block for building more complex elements.

---

## References

* [XSD Complex Elements](https://www.w3schools.com/xml/schema_complex.asp)
