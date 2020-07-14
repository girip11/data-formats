# XSD include and import

* `xs:include` - Imports the names from the same namespace that are present in external schema files. It can also be used to import names from xsd files that have no namespace defined.
* `xs:import` - Import names from different namespace that are present in external schema files.

* Examples for `xs:include` and `xs:import` can be found at [order.xsd](../tryouts/order/order.xsd). These examples have comments explaining all key points.

## Chameleon include

* Importing names from XSD that does not define a namespace using `targerNamespace` or `xmlns` is referred to as chameleon include.

## Importing multiple namespaces

```XML
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns="http://datypic.com/ord"
           xmlns:prod="http://datypic.com/prod"
           xmlns:ext="http://datypic.com/ext"
           targetNamespace="http://datypic.com/ord">
    <xs:import namespace="http://datypic.com/prod"
                schemaLocation="prod.xsd"/>
    <xs:import namespace="http://datypic.com/ext"
                schemaLocation="extension.xsd"/>
    <!-- Types defined in http://datypic.com/prod 
    will be accessed using prefix "prod:type" while
    types defined in http://datypic.com/ext will be defined 
    using "ext:type". These namespace prefixes are defined in the 
    xs:schema element-->
</xs:schema>
```

## Multiple imports of same namespace

```XML
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns="http://datypic.com/root"
           xmlns:ord="http://datypic.com/ord"
           targetNamespace="http://datypic.com/root">
    <!--Notice Summary and Detail belong to same namespace-->
    <xs:import namespace="http://datypic.com/ord"
                schemaLocation="Summary.xsd"/>
    <xs:import namespace="http://datypic.com/ord"
                schemaLocation="Detail.xsd"/>
    <!--...-->
</xs:schema>
```

## Proxy Schema to avoid multiple imports

```XML
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns="http://datypic.com/root"
           xmlns:ord="http://datypic.com/ord"
           targetNamespace="http://datypic.com/root">

    <!-- If Orders.xsd includes names from 
    Summary.xsd and Details.xsd, then those names are also
    imported when we import Orders.xsd -->
    <xs:import namespace="http://datypic.com/ord"
             schemaLocation="Orders.xsd"/>

    <!-- XSD definitions -->
</xs:schema>

<!-- Orders.xsd contents -->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns="http://datypic.com/ord"
           targetNamespace="http://datypic.com/ord">
  <xs:include schemaLocation="Summary.xsd"/>
  <xs:include schemaLocation="Detail.xsd"/>
  <!-- ... -->
</xs:schema>
```

---

## References

* [`include` vs `import` in XSD](https://stackoverflow.com/a/47684642/2369053)
* [include and import examples from Definitive XML Guide](http://www.datypic.com/books/defxmlschema/chapter04.html)
