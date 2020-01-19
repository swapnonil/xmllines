# Introduction

XML Lines is an XML based format that does away with XML Documents as a method of representing XML Data. It retains all essential data but strips away all forms of superfluous pretty printing elements such as headers, plural tags, tabs, spaces and line feeds.

## Motivation
XML Lines format is inspired from the excellent [JSON Lines Format](http://jsonlines.org) that has found broad acceptance and support within the Apache Spark Community. While libraries like [Databricks XML](https://github.com/databricks/spark-xml) to process XML in its native form exist, a format that does away with superfluous tags, making XML easy to validate and process in a distributed manner cannot be a bad thing.

## Example
XML Documents are normally represented as below

``` XML
<?xml version="1.0" encoding="UTF-8"?>
<header>
  <info>Below are all the book titles available at the Klingon Central Library.</info>
</header>
<books>
    <book>
      <authors>
        <author>Charles Dickens</author>
      </authors>
      <title>Great Expectations</title>
    </book>
    <book>
      <authors>
        <author>Jerome K. Jerome</author>
      </authors>
      <title>Three Men in a Boat</title>
    </book>
</books>
```
When expressed in terms of the XML Lines the same data becomes

```XML
<book><authors><author>Charles Dickens</author></authors><title>Great Expectations</title></book>
<book><authors><author>Jerome K. Jerome</author></authors><title>Three Men in a Boat</title></book>
```

No data is lost. Headers are not considered as interesting elements.

As is evident, XML processing instructions, headers containing mostly meta-data and first level plural tags are all stripped out. By doing so it makes files containing XML Line Records ideal for systems

1. That are built to process One line or One Record at time.
2. That support distributed reader processes. Readers can read and load non over-lapping chunks of XML in a parallel manner.
3. That support distributed writers. Writers that write out XML data in parallel without worrying as to which of those writers need to add the last plural tag (books in this case) at the end of the last file.

## Recommendations   
1. The structure and content of an XML Line Record can be defined using an XSD Schema. It is recommend to use XSD Schema based validation while reading an XML Line Record, especially when dealing with untrusted third parties.
2. XML is hierarchical and that is one of its great strengths. Do not flatten out XMLs just to fit it into tabular structures as Spark Dataframes. If necessary break XMLs into tabular structures, just as one does Object to Relational mapping.
