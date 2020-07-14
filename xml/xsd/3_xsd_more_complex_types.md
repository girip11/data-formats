# Defining more complex elements in XSD

This article will help us construct more complex XML elements with the simple and complex types that we have seen before.

## Problem-1

Consider the following XML snippet `<question name="foo">yes</question>`. Construct a schema for the complex element `question` which can take text value of `yes`, `no`, `maybe` only and the length of the attribute value should not exceed 20 and min length should be 8.

## Solution

To solve this problem, we need to define to simple types, one for attribute type and one for the type of the value inside the `question` element.

Now using the above created simple types, we need to create a complex type for question containing a `simpleContent` (remember complex element types with text only category)

```XML
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

    <xs:element name="question" type="questionType" />

    <xs:ComplexType name="questionType">
        <xs:simpleContent>
            <xs:extension base="questionTextType">
                <xs:attribute name="name" type="nameAttrType">
            </xs:extension>
        </xs:simpleContent>
    </xs:ComplexType>

    <!-- type representing the text value of question element -->
    <xs:simpleType name="questionTextType">
        <xs:restriction base="xs:string">
            <xs:enumeration value="yes" />
            <xs:enumeration value="no" />
            <xs:enumeration value="maybe" />
        </xs:restriction>
    </xs:simpleType>

    <!--type with restrictions on attribute value-->
    <xs:simpleType name="nameAttrType">
        <xs:restriction base="xs:string">
            <xs:minLength value="8" />
            <xs:maxLength value="20" />
        </xs:restriction>
    </xs:simpleType>

</xs:schema>
```

---

## References

* [XSD for simplecontent with attribute and text](https://stackoverflow.com/questions/5457217/xsd-for-simplecontent-with-attribute-and-text)
* [How do I have a XSD complextype with *simple content* that has a restriction with enum values AND an extension](https://stackoverflow.com/questions/29767339/how-do-i-have-a-xsd-complextype-with-simple-content-that-has-a-restriction-wit)
* [XML Schema: element with attributes and text with restrictions](https://stackoverflow.com/questions/7918999/xml-schema-element-with-attributes-and-text-with-restrictions)
* [Defining Simple and ComplexTypes](https://docstore.mik.ua/orelly/xml/xmlnut/ch16_06.htm)
