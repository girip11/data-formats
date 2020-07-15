# XSD Element substitution

## Element substitution

Head element and substitutable elements should be declared as global element for substitution to work.

```XML
<!-- name is called as the head element -->
<xs:element name="name" type="xs:string"/>

<!-- Elements marked with substitutionGroup attribute should have same type
or a derived type of the head element.
Type is not mentioned below because, name and navn have same type.
If substitutable element has a derived type, it is mentioned using the 
type attribute-->
<xs:element name="navn" substitutionGroup="name"/>

<xs:complexType name="custinfo">
  <xs:sequence>
    <!-- Notice reference to an existing element or group using ref attribute -->
    <xs:element ref="name"/>
  </xs:sequence>
</xs:complexType>

<!-- customer is called as the head element -->
<xs:element name="customer" type="custinfo"/>
<xs:element name="kunde" substitutionGroup="customer"/>
```

With above XSD, the following XML snippets are valid.

```XML
<customer>
  <name>John Smith</name>
</customer>

<kunde>
  <navn>John Smith</navn>
</kunde>
```

Very useful in localizations.

## Blocking element substitution

```XML
<!-- This will make the name element unsubstitutable -->
<xs:element name="name" type="xs:string" block="substitution"/>
```

---

## References

* [XSD Element substitution](https://www.w3schools.com/xml/schema_complex_subst.asp)
