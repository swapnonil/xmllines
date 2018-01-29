# Apache Spark based Processing

## Reading
Reading XML Lines Records can be as simple as
1. Reading each line as a String. This results in a RDD of Strings.
2. Looping over this RDD of Strings.
3. Converting each String into an XML document. String to XML and XML to Strings functions exists with many libraries. Libraries such as JAXB and X-Stream take the pain out of parsing XML. JAXB can also be paired with XML Schemas to perform validations as well.

Consider the following XSD Schema fragment from [ESMA DRAFT15auth.016.001.01](https://www.iso20022.org/documents/messages/auth/schemas/auth.016.001.01.xsd)
```XML
<xs:simpleType name="ActiveCurrencyAnd13DecimalAmount_SimpleType">
    <xs:restriction base="xs:decimal">
        <xs:fractionDigits value="13"/>
        <xs:totalDigits value="18"/>
        <xs:minInclusive value="0"/>
    </xs:restriction>
</xs:simpleType>

<xs:simpleType name="CFIOct2015Identifier">
    <xs:restriction base="xs:string">
        <xs:pattern value="[A-Z]{6,6}"/>
    </xs:restriction>
</xs:simpleType>
<xs:simpleType name="CancelledStatusReason15Code">
    <xs:restriction base="xs:string">
        <xs:enumeration value="CANI"/>
        <xs:enumeration value="CSUB"/>
    </xs:restriction>
</xs:simpleType>
<xs:simpleType name="CountryCode">
    <xs:restriction base="xs:string">
        <xs:pattern value="[A-Z]{2,2}"/>
    </xs:restriction>
</xs:simpleType>
```
Neither JSON nor AVRO Schemas come close to matching the expressiveness of an XML Schema. This makes XML ideal as a format of data exchange in the wider world. And that's not going to change.
The advantage of such an expressive schema language means that validations on incoming data is performed by parser and validator libraries such as JAXP and JAXB automatically rather than developers wasting time and effort writing tones and tones of validation logic.

Below is an example of using JAXB to read XML Line Records.

```scala
val jAXBContext = JAXBContext.newInstance(classOf[Person])
val unmarshaller = jAXBContext.createUnmarshaller
val xmlLines = spark.sparkContext.textFile("person.xlr")

xmlLines.foreach((line: String) => {
  val myPersonClass = unmarshaller.unmarshal(new StringReader(line)).asInstanceOf[Person]
}
)
```
## Schema Validation
Schema validation can be easily added to JAXB while unmarshalling a XML Line Record. [Here is an Java example](http://blog.bdoughan.com/2010/12/jaxb-and-marshalunmarshal-schema.html)

## Writing
JAXB elements can just easily be converted back to String.
```java
private static void writeDocument() {
    try {
        JAXBContext context = JAXBContext.newInstance(PersonType.class, AddressType.class);
        Marshaller m = context.createMarshaller();
        m.setProperty(Marshaller.JAXB_FORMATTED_OUTPUT, Boolean.FALSE);
        StringWriter writer = new StringWriter();
        m.marshal(getPersonType(), writer);
        xmlString = writer.toString();
    } catch (JAXBException e) {
        e.printStackTrace();
    }
}
```
