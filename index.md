# Introduction

XML Lines is an XML based format that does away with XML Documents as a method of representing XML Data. It retains all essential data but strips away all forms of superfluous pretty printing elements such as tabs, spaces and line feeds.

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
