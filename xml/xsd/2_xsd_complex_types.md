# XSD Complex Elements and Types

* A complex element contains other elements and/or attributes.
* Empty elements with attributes are also complex elements.

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

* If we need to use any restriction on the attribute value, then the elements `xs:complexContent` and `xs:restriction` will be used

* Reusable representation of an empty element where attribute definitions can be reused.

```XML
<xs:element name="product" type="prodtype"/>

<xs:complexType name="prodtype">
  <xs:attribute name="prodid" type="xs:positiveInteger"/>
</xs:complexType>
```

## Complex text only elements

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

## Complex types with text and child elements

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

---

## References

* [XSD Complex Elements](https://www.w3schools.com/xml/schema_complex.asp)
