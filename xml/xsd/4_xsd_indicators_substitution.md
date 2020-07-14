# XSD Indicators and Substitution

There are 3 categories of indicators and 7 total indicators

* Order indicators (applicable to XML elements)
* Occurrence indicators (these are XML attributes)
* Group indicators (applicable to elements and attributes)

**NOTE**: By default `minOccurs` and `maxOccurs` indicators are set to 1 for all other indicators.

## Order Indicators

* These indicators are used in complex type elements containing other elements(includes the mixed case as well)
* `all`, `choice` and `sequence` indicators are order indicators

### `all` indicator

* `all` - all child elements should appear only once and in any order.

### `choice` indicator

* Only one child element among the listed ones can appear.

### `sequence` indicator

* `sequence` - all child elements should in the same order as defined in the schema and exactly once(by default)

## Occurence indicator

* If `minOccurs` and `maxOccurs` are set to 0, then the element cannot be present in the target XML document.
* `minOccurs` is 0 and `maxOccurs` is 1, then the element becomes optional.
* For `all`, `minOccurs` can be 0 or 1 and `maxOccurs` can be 1. No other values are permitted.
* If `maxOccurs` is set to `unbounded`, the element can occur infinitely. Useful in lists, where child element is marked with `maxOccurs="unbounded"` so that the list can contain any number of child elements.

```XML
<!-- sample xml conforming to the schema -->
<person>
  <full_name>Tove Refsnes</full_name>
  <children>
  <child_name>Hege</child_name>
  <child_name>Stale</child_name>
  <child_name>Jim</child_name>
  <child_name>Borge</child_name>
  </children>
</person>

<!-- schema -->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:element name="person">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="full_name" type="xs:string"/>
                <xs:element name="children">
                    <xs:complexType>
                        <xs:sequence>
                        <xs:element name="child_name" type="xs:string" minOccurs="0"   maxOccurs="unbounded" />
                        </xs:sequence>
                    </xs:complexType>
                </xs:element>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## Group indicators

* Group indicators can be applied to elements as well as to attributes

* Elements group should contain either `all`, `choice` or `sequence`

```XML
<!-- element group groups associated elements together -->
<xs:group name="groupname">
    <xs:sequence>
        <!-- elements to group -->
    </xs:sequence>
</xs:group>

<!-- These groups can be referred in the complexTypes -->
<xs:complexType name="complexType">
  <xs:sequence>
    <xs:group ref="groupname"/>
    <!-- other elements in the sequence -->
  </xs:sequence>
</xs:complexType>
```

* `xs:attributeGroup` defines an attribute group.

```XML
<xs:attributeGroup name="attrGroupName">
    <!-- contains xs:attribute nodes-->
</xs:attributeGroup>

<!-- these can be used in complexTypes -->
<xs:element name="someElement">
  <xs:complexType>
    <xs:attributeGroup ref="attrGroupName"/>
  </xs:complexType>
</xs:element>
```

## `<any>` element

* Marks an XML element for extension. The XML element can contain new elements that are not in the schema.

```XML
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
      <xs:any minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

## `<anyAttribute>` element

* Makes an XML element to contains new attributes that are not defined in the schema.

```XML
<xs:element name="person">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="firstname" type="xs:string"/>
            <xs:element name="lastname" type="xs:string"/>
        </xs:sequence>
        <!-- person element can contain any number of new attributes -->
        <xs:anyAttribute/>
    </xs:complexType>
</xs:element>
```

---

## References

* [Indicators in XSD](https://www.w3schools.com/xml/schema_complex_indicators.asp)
* [`<anyAttribute>`](https://www.w3schools.com/xml/schema_complex_anyattribute.asp)
