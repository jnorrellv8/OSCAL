---
title: Creating and Using Metadata in OSCAL
description: A tutorial that covers the usage of the metadata section in OSCAL
weight: 5
suppresstopiclist: true
toc:
  enabled: true
---

This tutorial covers the basics of the common OSCAL metadata section, and provides a walk-through on creating `metadata` for a new OSCAL document. Before reading this tutorial you should:

- Have some familiarity with the [XML](https://www.w3.org/standards/xml/core), [JSON](https://www.json.org/), or [YAML](https://yaml.org/spec/) formats.
- Have a basic understanding of OSCAL models and their overall structure. ([OSCAL High-Level Overview](/concepts/layer/overview/))

## What is the Metadata Section?

All OSCAL models share some common structure and elements, as discussed in [Common High-Level Structure](/concepts/layer/overview/#common-high-level-structure). The foremost of these is the metadata section, which includes important identifying and categorizing information. The metadata section contains several mandatory fields that are vital to the processing of OSCAL documents and follow requirements that help ensure interoperability, as well as optional fields that are designed to allow OSCAL content creators great flexibility in expressing additional information. 

As with all parts of OSCAL, the metadata section provides machine-readable formats in XML, JSON, and YAML, which support representing an equivalent set of information. Examples in this tutorial are provided for XML, JSON, and YAML to show the equivalent representations.

## Metadata Fields

The OSCAL metaschema reference ([XML](/reference/latest/complete/xml-definitions/#/assembly/oscal-metadata/metadata) | [JSON](/reference/latest/complete/json-definitions/#/assembly/oscal-metadata/metadata)) provides a comprehensive listing of the metadata section fields.  Below is the high-level structure of the metadata section in XML, JSON, and YAML followed by a listing of each fields' definition with a brief description.

{{< tabs XML JSON YAML >}}
{{% tab %}}
{{< highlight xml "linenos=table" >}}
<metadata xmlns="http://csrc.nist.gov/ns/oscal/1.0">
    <title>markup-line</title> [1]
    <published>datetime-with-timezone</published> [0 or 1]
    <last-modified>datetime-with-timezone</last-modified> [1]
    <version>string</version> [1]
    <oscal-version>string</oscal-version> [1]
    <revisions> … </revisions> [0 or 1]
    <document-id scheme="uri">string</document-id> [0 to ∞]
    <prop name="ncname" uuid="uuid" ns="uri"
          value="string" class="ncname"> … </prop> [0 to ∞]
    <link href="uri-reference" rel="ncname"
          media-type="string"> … </link> [0 to ∞]
    <role id="ncname"> … </role> [0 to ∞]
    <location uuid="uuid"> … </location> [0 to ∞]
    <party uuid="uuid" type="string"> … </party> [0 to ∞]
    <responsible-party role-id="ncname"> … </responsible-party> [0 to ∞]
    <remarks>markup-multiline</remarks> [0 or 1]
</metadata>
{{< /highlight >}}

Element definitions:

- [`<title>`](/reference/latest/complete/xml-reference/#/catalog/metadata/title) (required) - A human-readable title for the document, expressed as [Markdown content](/reference/datatypes/#markup-line).
- [`<published>`](/reference/latest/complete/xml-reference/#/catalog/metadata/published) (optional) - The date and time that this document was originally published, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone).
- [`<last-modified>`](/reference/latest/complete/xml-reference/#/catalog/metadata/last-modified) (required) - The date and time that this document instance was last modified, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone). If any part of the document is changed, this value should be updated to the current date and time.
- [`<version>`](/reference/latest/complete/xml-reference/#/catalog/metadata/version) (required) - A [simple String](/reference/datatypes/#string) that provides the version of the document instance. If any part of the document is changed, this version value should be incremented according to the versioning scheme used.
- [`<oscal-version>`](/reference/latest/complete/xml-reference/#/catalog/metadata/oscal-version) (required) - The version of OSCAL that this document instance was written against, expressed as a [simple String](/reference/datatypes/#string).
- [`<revisions>`](/reference/latest/complete/xml-reference/#/catalog/metadata/revisions/revision) (optional) - A list of revisions to the document to enable advanced change tracking.
- [`<document-id>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/document-id) (zero to many) - A unique ID that identifies a document, and ties all separate instances of the document into one document series. The `@scheme` attribute provides a URI to identify the scheme used to generate the ID. See the later [section](#using-the-document-id) for more information and usage guidance.
- [`<prop>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/prop) (zero to many) - Represents some arbitrary "property" of the document. This flexible element provides an extension point for adding additional metadata. See the later [section](#props) for more information and usage guidance.
- [`<link>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/link) (zero to many) - Provides a resolvable URI (the `@href` attribute) for some resource. The purpose of the link is given with the `@rel` attribute, and it's media type through the `@media-type` attribute. See the later [section](#links) for more information and usage guidance.
- [`<role>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/role) (zero to many) - Defines a function or duty assumed or expected to be assumed by a party (i.e., person or organization) in a specific situation.
- [`<location>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/location) (zero to many) - A location, with associated metadata that can be referenced.
- [`<party>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/party) (zero to many) - A responsible entity which is either a person or an organization that can be referenced.
- [`<responsible-party>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/responsible-party) (zero to many) - A reference to a set of organizations or persons that have responsibility for performing a referenced role in the context of the containing object.
- [`<remarks>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/remarks) (optional) - Markup formatted text consisting of notes for human readers of the content.

{{% /tab %}}
{{% tab %}}
{{< highlight json "linenos=table" >}}
{
"metadata" : {
    "title": "markup-line",
    "published": "dateTime-with-timezone",
    "last-modified": "dateTime-with-timezone",
    "version": "string",
    "oscal-version": "string",
    "revisions": "[ … ]",
    "document-ids": "[ … ]",
    "props": "[ … ]",
    "links": "[ … ]",
    "roles": "[ … ]",
    "locations": "[ … ]",
    "parties": "[ … ]",
    "responsible-parties": "[ … ]",
    "remarks": "markup-multiline"
    }
}
{{< /highlight >}}

Note that in JSON, any objects that may appear multiple times are contained in a JSON Array.

Field definitions:

- [`title`](/reference/latest/complete/json-reference/#/catalog/metadata/title) (required) - A human-readable title for the document, expressed as [Markdown content](/reference/datatypes/#markup-line).
- [`published`](/reference/latest/complete/json-reference/#/catalog/metadata/published) (optional) - The date and time that this document was originally published, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone).
- [`last-modified`](/reference/latest/complete/json-reference/#/catalog/metadata/last-modified) (required) - The date and time that this document instance was last modified, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone). If any part of the document is changed, this value should be updated to the current date and time.
- [`version`](/reference/latest/complete/json-reference/#/catalog/metadata/version) (required) - A [simple String](/reference/datatypes/#string) that provides the version of the document instance. If any part of the document is changed, this version value should be incremented according to the versioning scheme used.
- [`oscal-version`](/reference/latest/complete/json-reference/#/catalog/metadata/oscal-version) (required) - The version of OSCAL that this document instance was written against, expressed as a [simple String](/reference/datatypes/#string).
- [`revisions`](/reference/latest/complete/json-reference/#/catalog/metadata/revisions) (optional) - A list of revisions to the document to enable advanced change tracking.
- [`document-ids`](/reference/latest/catalog/json-reference/#/catalog/metadata/document-id) (optional, zero to many) - A unique ID that identifies a document, and ties all separate instances of the document into one document series. The `@scheme` attribute provides a URI to identify the scheme used to generate the ID. See the later [section](#using-the-document-id) for more information and usage guidance.
- [`props`](/reference/latest/catalog/json-reference/#/catalog/metadata/props) (optional, zero to many) - Represents some arbitrary "property" of the document. This flexible element provides an extension point for adding additional metadata. See the later [section](#props) for more information and usage guidance.
- [`links`](/reference/latest/catalog/json-reference/#/catalog/metadata/links) (optional, zero to many) - Provides a resolvable URI (the `href` property) to some resource. The purpose of the link is given with the `rel` attribute, and it's media type through the `media-type` property.  See the later [section](#links) for more information and usage guidance.
- [`roles`](/reference/latest/catalog/json-reference/#/catalog/metadata/roles) (optional, zero to many) - Defines a function or duty assumed or expected to be assumed by a party (i.e., person or organization) in a specific situation.
- [`locations`](/reference/latest/catalog/json-reference/#/catalog/metadata/locations) (optional, zero to many) - A location, with associated metadata that can be referenced.
- [`parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/parties) (optional, zero to many) - A responsible entity which is either a person or an organization that can be referenced.
- [`responsible-parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/responsible-parties) (optional, zero to many) - A reference to a set of organizations or persons that have responsibility for performing a referenced role in the context of the containing object.
- [`remarks`](/reference/latest/catalog/json-reference/#/catalog/metadata/remarks) (optional) - Markup formatted text consisting of notes for human readers of the content.

{{% /tab %}}
{{% tab %}}
{{< highlight yaml "linenos=table" >}}
metadata :
    title: markup-line
    published: dateTime-with-timezone
    last-modified: dateTime-with-timezone
    version: string
    oscal-version: string
    revisions: …
    document-ids: …
    props: …
    links: …
    roles: …
    locations: …
    parties: …
    responsible-parties: …
    remarks: markup-multiline
{{< /highlight >}}

Note that in YAML, any objects that may appear multiple times are contained in a YAML Array.

Field definitions:

- [`title`](/reference/latest/complete/json-reference/#/catalog/metadata/title) (required) - A human-readable title for the document, expressed as [Markdown content](/reference/datatypes/#markup-line).
- [`published`](/reference/latest/complete/json-reference/#/catalog/metadata/published) (optional) - The date and time that this document was originally published, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone).
- [`last-modified`](/reference/latest/complete/json-reference/#/catalog/metadata/last-modified) (required) - The date and time that this document instance was last modified, expressed as a [DateTime](/reference/datatypes/#datetime-with-timezone). If any part of the document is changed, this value should be updated to the current date and time.
- [`version`](/reference/latest/complete/json-reference/#/catalog/metadata/version) (required) - A [simple String](/reference/datatypes/#string) that provides the version of the document instance. If any part of the document is changed, this version value should be incremented according to the versioning scheme used.
- [`oscal-version`](/reference/latest/complete/json-reference/#/catalog/metadata/oscal-version) (required) - The version of OSCAL that this document instance was written against, expressed as a [simple String](/reference/datatypes/#string).
- [`revisions`](/reference/latest/complete/json-reference/#/catalog/metadata/revisions) (optional) - A list of revisions to the document to enable advanced change tracking.
- [`document-ids`](/reference/latest/catalog/json-reference/#/catalog/metadata/document-id) (optional, zero to many) - A unique ID that identifies a document, and ties all separate instances of the document into one document series. The `@scheme` attribute provides a URI to identify the scheme used to generate the ID. See the later [section](#using-the-document-id) for more information and usage guidance.
- [`props`](/reference/latest/catalog/json-reference/#/catalog/metadata/props) (optional, zero to many) - Represents some arbitrary "property" of the document. This flexible element provides an extension point for adding additional metadata. See the later [section](#props) for more information and usage guidance.
- [`links`](/reference/latest/catalog/json-reference/#/catalog/metadata/links) (optional, zero to many) - Provides a resolvable URI (the `href` property) to some resource. The purpose of the link is given with the `rel` attribute, and it's media type through the `media-type` property.  See the later [section](#links) for more information and usage guidance.
- [`roles`](/reference/latest/catalog/json-reference/#/catalog/metadata/roles) (optional, zero to many) - Defines a function or duty assumed or expected to be assumed by a party (i.e., person or organization) in a specific situation.
- [`locations`](/reference/latest/catalog/json-reference/#/catalog/metadata/locations) (optional, zero to many) - A location, with associated metadata that can be referenced.
- [`parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/parties) (optional, zero to many) - A responsible entity which is either a person or an organization that can be referenced.
- [`responsible-parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/responsible-parties) (optional, zero to many) - A reference to a set of organizations or persons that have responsibility for performing a referenced role in the context of the containing object.
- [`remarks`](/reference/latest/catalog/json-reference/#/catalog/metadata/remarks) (optional) - Markup formatted text consisting of notes for human readers of the content.

{{% /tab %}}
{{% /tabs %}}

## Creating a Metadata Section

Lets start with a nominal example of an OSCAL catalog document to demonstrate how to create the metadata section.

{{% callout note %}}
The data structure of the metadata section is identical in all OSCAL models, so the demonstrated constructs in this tutorial apply to all OSCAL models.
{{% /callout %}}

### Document UUID

All OSCAL documents use a randomly generated globally unique identifier (UUID) to provide a stable and unique way to refer to a given instance of an OSCAL document. UUIDs are generated when the OSCAL document is created or revised.

{{< tabs XML JSON YAML >}}
{{% tab %}}
{{< highlight xml "linenos=false" >}}
<catalog uuid="c3da6d1d-c20c-4c7c-ae73-4010167a186b">
{{< /highlight >}}

Although technically outside of the metadata section, the `@uuid` of a document is always given as an attribute on the root element, and shares its structure across all OSCAL document types. This is provided using the [uuid](/reference/datatypes/#uuid) datatype. Many tools provide easy ways to generate version 4 UUIDs. For our example we have generated one using a trivial UUIDv4 generator.

{{% /tab %}}
{{% tab %}}
{{< highlight json "linenos=table" >}}
{
  "catalog": {
    "uuid": "c3da6d1d-c20c-4c7c-ae73-4010167a186b"
  }
}
{{< /highlight >}}

Although technically outside of the metadata section, the `uuid` of a document is always given as a property of the root object, and shares its structure across all OSCAL document types. This is provided using the [uuid](/reference/datatypes/#uuid) datatype. Many tools provide easy ways to generate version 4 UUIDs. For our example we have generated one using a trivial UUIDv4 generator.

{{% /tab %}}
{{% tab %}}
{{< highlight yaml "linenos=table" >}}
---
catalog:
  uuid: c3da6d1d-c20c-4c7c-ae73-4010167a186b

{{< /highlight >}}

Although technically outside of the metadata section, the `uuid` of a document is always given as a property of the root object, and shares its structure across all OSCAL document types. This is provided using the [uuid](/reference/datatypes/#uuid) datatype. Many tools provide easy ways to generate version 4 UUIDs. For our example we have generated one using a trivial UUIDv4 generator.
{{% /tab %}}
{{% /tabs %}}

### Creating Metadata Fields

The metadata section contains fields which may be categorized as:
- required
- recommended
- extensions
- and other optional fields

The subsequent tutorial sections demonstrates how the aforementioned field categories can be used to support specific use cases.

#### Using Required and Optional Recommended Fields

First, we start with the document's title, which is intended to be brief, human-readable text that will help a reader understand the context of the document.

The following is an example of what a title might look like expressed in OSCAL:

{{< tabs XML JSON YAML >}}
{{% tab %}}
{{< highlight xml "linenos=table" >}}
<title>Example OSCAL Catalog</title>
{{< /highlight >}}

Next, we have the published and last modified date/time fields that represent when the document was published for the first time and most recently changed respectively. These field values are expressed using the OSCAL [dataTime-with-timezone](/reference/datatypes/#datetime-with-timezone) data type, which requires that the time zone offset is included to provide a localized time.

Lets look at a scenario where an OSCAL document was:
- published on January 1st, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from Coordinated Universal Time (UTC)
- updated on January 5th, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from UTC

This information would be expressed in OSCAL as follows in the metadata section:

{{< highlight xml "linenos=table" >}}
<published>2021-01-01T00:00:00-5:00</published>
<last-modified>2021-01-05T00:00:00-5:00</last-modified>
{{< /highlight >}}

The `<published>` element gives the date and time of when the document was published for the first time. This element is <em>not</em> required, but it is strongly recommended to include it to help the OSCAL document consumer understand when the document was originally published.

The `<last-modified>` element provides the the date and time of the most recent change to this document. If a document was never previously updated, the `<last-modified>` value should be identical to the one given by `published`.

Thirdly, we must provide a `<version>` element. This refers to the version of the OSCAL document itself, not the version of any other content. OSCAL does not place requirements on the version string itself, as versioning is a complicated process that differs from content creator to content creator. Since this is the first time we've created this document, some variation on a "1.0" would be appropriate.

{{< highlight xml "linenos=false" >}}
<version>1.0.0</version>
{{< /highlight >}}

This version will be incremented whenever the OSCAL document is updated.

Where possible, use well formatted versions with a clear and well defined syntax. The NIST OSCAL team uses [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) as the versioning scheme for all schema, metaschema, and OSCAL documents we maintain.

The final required element is `<oscal-version>`, which indicates what version of OSCAL was used to create this document.

{{< highlight xml "linenos=table" >}}
<oscal-version>1.0.4</oscal-version>
{{< /highlight >}}

We have now covered all required fields of the metadata section, and could publish our document as a valid OSCAL document. Here is what it would look like all together:

{{< highlight xml "linenos=table" >}}
<catalog uuid="c3da6d1d-c20c-4c7c-ae73-4010167a186b">
  <metadata>
      <title>Example OSCAL Document</title>
      <published>2021-01-01T00:00:00-5:00</published>
      <last-modified>2021-01-05T00:00:00-5:00</last-modified>
      <version>1.0.0<version>
      <oscal-version>1.0.4</oscal-version>
  </metadata>
  ...
</catalog>
{{< /highlight >}}
{{% /tab %}}
{{% tab %}}
{{< highlight json "linenos=table" >}}
{
"title":"Example OSCAL Catalog"
}
{{< /highlight >}}

Next we have the published and last modified date/time fields that represent when the document was published for the first time and most recently changed respectively. These field values are expressed using the OSCAL [dataTime-with-timezone](/reference/datatypes/#datetime-with-timezone) data type, which requires that the time zone offset is included to provide a localized time.

Lets look at a scenario where an OSCAL document was:
- published on January 1st, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from Coordinated Universal Time (UTC)
- updated on January 5th, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from UTC

This information would be expressed in OSCAL as follows in the metadata section:

{{< highlight json "linenos=table" >}}
{
"published": "2021-01-01T00:00:00-05:00",
"last-modified": "2021-01-05T00:00:00-05:00"
}
{{< /highlight >}}

The `published` field gives the date and time of when the document was published for the first time. This field is <em>not</em> required, but it is strongly recommended to include it to help the OSCAL document consumer understand when the document was originally published.

The `last-modified` field provides the the date and time of the most recent change to this document. If a document was never previously updated, the `last-modified` value should be identical to the one given by `published`.

Thirdly we must provide a `version` field. This refers to the version of the OSCAL document itself, <em>not</em> the version of any other content. OSCAL does not place requirements on the version string itself, as versioning is a complicated process that differs from content creator to content creator. Since this is the first time we've created this document, some variation on a "1.0" would be appropriate.

{{< highlight json "linenos=table" >}}
{
"version":"1.0.0"
}
{{< /highlight >}}

This version will be incremented whenever the OSCAL document is updated.

Where possible, use well formatted versions with clear and defined rules. This version will be incremented whenever the OSCAL document is updated.

The final required field is `oscal-version`, which indicates what version of OSCAL was used to create this document.

{{< highlight json "linenos=table" >}}
{
"oscal-version":"1.0.4"
}
{{< /highlight >}}

We have now covered all required fields of the metadata section, and could publish our document as a valid OSCAL document. Here is what it would look like all together:

{{< highlight json "linenos=table" >}}
{
  "catalog": {
    "uuid": "c3da6d1d-c20c-4c7c-ae73-4010167a186b"
    "metadata": {
      "title":"Example OSCAL Catalog",
      "published":"2021-01-01T00:00:00-5:00",
      "last-modified":"2021-01-05T00:00:00-5:00",
      "version":"1.0.0",
      "oscal-version":"1.0.4"
    }
  }
}
{{< /highlight >}}
{{% /tab %}}
{{% tab %}}
{{< highlight yaml "linenos=table" >}}
  title: Example OSCAL Catalog
{{< /highlight >}}
Next we have the published and last modified date/time fields that represent when the document was published for the first time and most recently changed respectively. These field values are expressed using the OSCAL [dataTime-with-timezone](/reference/datatypes/#datetime-with-timezone) data type, which requires that the time zone offset is included to provide a localized time.

Lets look at a scenario where an OSCAL document was:
- published on January 1st, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from Coordinated Universal Time (UTC)
- updated on January 5th, 2021 at "midnight" or 12:00AM with a time offset of 5 hours from UTC

This information would be expressed in OSCAL as follows in the metadata section:

{{< highlight yaml "linenos=table" >}}
  published: 2021-01-01T00:00:00-05:00
  last-modified: 2021-01-05T00:00:00-05:00
{{< /highlight >}}

The `published` key gives the date and time of when the document was published for the first time. This key is <em>not</em> required, but it is strongly recommended to include it to help the OSCAL document consumer understand when the document was originally published.

The `last-modified` key provides the the date and time of the most recent change to this document. If a document was never previously updated, the `last-modified` value should be identical to the one given by `published`.

Thirdly we must provide a `version` field. This refers to the version of the OSCAL document itself, <em>not</em> the version of any other content. OSCAL does not place requirements on the version string itself, as versioning is a complicated process that differs from content creator to content creator. Since this is the first time we've created this document, some variation on a "1.0" would be appropriate.

{{< highlight yaml "linenos=table" >}}
  version: 1.0.0
{{< /highlight >}}

Where possible, use well formatted versions with clear and defined rules. This version will be incremented whenever the OSCAL document is updated.

The final required field is `oscal-version`, which indicates what version of OSCAL was used to create this document.

{{< highlight yaml "linenos=table" >}}
  oscal-version: 1.0.4
{{< /highlight >}}

We have now covered all required fields of the metadata section, and could publish our document as a valid OSCAL document. Here is what it would look like all together:

{{< highlight yaml "linenos=table" >}}
---
  catalog: 
    uuid: c3da6d1d-c20c-4c7c-ae73-4010167a186b
    metadata: 
      title: Example OSCAL Catalog
      published: 2021-01-01T00:00:00-5:00
      last-modified: 2021-01-05T00:00:00-5:00
      version: 1.0.0
      oscal-version: 1.0.4
{{< /highlight >}}
{{% /tab %}}
{{% /tabs %}}

However, the metadata section has several other optional but important fields which we will cover in the following sections.

#### Using the Document-Id

OSCAL documents, by their nature, may often require updates of varying impact. It is important to track these updates in a way that can be automated and managed by a wide array of systems and users. To that end, we must cover the concepts of "Document Series" and "Document Instances".

A "Document Series" is defined as a document and all of its updates and versions. In more human terms, if a content author writes "Document 1 version 1", "Document 1 version 2", and "Document 1 version 3.4", we could say that the "Document Series" is simply "Document 1".

A "Document Instance" is defined as a single document that is part of a "Document Series". Using the above example, "Document 1 version 2" is a "Document Instance".

{{< tabs XML JSON YAML >}}
{{% tab %}}

In OSCAL we use `<document-id>` to track "Document Series", and `@uuid` to track "Document Instance".

Continuing with our OSCAL catalog example, we have already generated a uuid to identify the particular instance that we are publishing. Since we are publishing the first document in a series, we should also define a `<document-id>`. The `@scheme` attribute is a [URI](/reference/datatypes/#uri) that identifies the naming scheme we are using for the ID. While there are no strict requirements around the scheme and the ID, it is recommended that an existing identification scheme is used. For our example we will use [DOI](https://www.doi.org/).

{{< highlight xml "linenos=table" >}}
<document-id scheme="https://www.doi.org/">10.1000/182</document-id>
{{< /highlight >}}

In the future, any updated versions of this document we publish will have the same `<document-id>` value, but a different `@uuid`.

In the case that a document does not explicitly provide a `<document-id>` value, it is given one implicitly that is equal to it's `@uuid`. This means that if we publish a new document without a `<document-id>`, then later need to publish an update to it, we can tie it back to the original document.

Multiple `<document-id>` elements denote that a document is a part of multiple Document Series. This is a rare but valid use case.
{{% /tab %}}

{{% tab %}}

In OSCAL we use `document-ids` to track "Document Series", and `uuid` to track "Document Instance".

Continuing with our OSCAL catalog example, we have already generated a uuid to identify the particular instance that we are publishing. Since we are publishing the first document in a series, we should also define a `document-ids`. The `scheme` field is a [URI](/reference/datatypes/#uri) that identifies the naming scheme we are using for the ID. While there are no strict requirements around the scheme and the ID, it is recommended that an existing identification scheme is used. For our example we will use [DOI](https://www.doi.org/).

{{< highlight json "linenos=table" >}}
{
"document-ids": [
    {
    "scheme":"https://www.doi.org/",
    "value":"10.1000/182"
    }
  ]
}
{{< /highlight >}}

In the future, any updated versions of this document we publish will have the same `document-ids` values, but a different `uuid`.

In the case that a document does not explicitly provide at least one object inside `document-ids`, it is given a value implicitly that is equal to it's `uuid`. This means that if we publish a new document without a `document-ids`, then later need to publish an update to it, we can tie it back to the original document.
{{% /tab %}}

{{% tab %}}

In OSCAL we use `document-id` to track "Document Series", and `uuid` to track "Document Instance".

Continuing with our OSCAL catalog example, we have already generated a uuid to identify the particular instance that we are publishing. Since we are publishing the first document in a series, we should also define a `document-id`. The `scheme` field is a [URI](/reference/datatypes/#uri) that identifies the naming scheme we are using for the ID. While there are no strict requirements around the scheme and the ID, it is recommended that an existing identification scheme is used. For our example we will use [DOI](https://www.doi.org/).

{{< highlight yaml "linenos=table" >}}
document-ids:
  - scheme: https://www.doi.org/
    value: 10.1000/182
{{< /highlight >}}

In the future, any updated versions of this document we publish will have the same `document-id` value, but a different `uuid`.

In the case that a document does not explicitly provide a `document-id` value, it is given one implicitly that is equal to it's `uuid`. This means that if we publish a new document without a `document-id`, then later need to publish an update to it, we can tie it back to the original document.
{{% /tab %}}
{{% /tabs %}}

### Using Optional Extensions

#### Props
{{< tabs XML JSON YAML >}}
{{% tab %}}
The metadata section may include any number of `<prop>` elements.  The `<prop>` element provides a key-value pairing to provide additional information alongside the well-defined elements of the metadata section. Some common use cases include labels, sorting tags, and classification. There are no restrictions on using custom `@name` and `@value` pairs making `<prop>` an excellent place to extend OSCAL models to meet proprietary use cases. The metadata section may include any number of `<prop>` elements.  

{{% callout note %}}
More information on the use of `<prop>` is provided in the [Props and Links tutorial](/learn/tutorials/extensions/#props).
{{% /callout %}}

For this example, we will create a `<prop>` to mark an OSCAL document with a [Traffic Light Protocol (TLP)](https://en.wikipedia.org/wiki/Traffic_Light_Protocol) classification.

{{< highlight xml "linenos=table" >}}
<prop name="marking" value="red" class="TLP"/>
{{< /highlight >}}

The `@name` attribute is the key, and the `@value` attribute is the value. Together they form a key-value pair to facilitate automated data lookup. Here we use an optional `@class` attribute to provide an additional layer of information, noting that the marking we are using is the TLP protocol.  
The [Props and Links tutorial](/learn/tutorials/extensions/#using-an-existing-prop) provides more details on these attributes.
  
{{% /tab %}}
{{% tab %}}
The `props` object array provides any number of key-value pairing to provide additional information alongside the well-defined fields of the metadata section. Some common use cases include labels, sorting tags, and classification. There are no restrictions on using custom `name` and `value` pairs making `props` an excellent place to do extension of the OSCAL model to meet proprietary use cases. 

{{% callout note %}}
More information on the use of `props` is provided in the [Props and Links tutorial](/learn/tutorials/extensions/#props).
{{% /callout %}}

For this example, we will create a `props` array to mark an OSCAL document with a [Traffic Light Protocol (TLP)](https://en.wikipedia.org/wiki/Traffic_Light_Protocol) classification.

{{< highlight json "linenos=table" >}}
{
  "props": [
    {
    "name":"marking",
    "value":"red",
    "class":"TLP"
    }
  ]
}
{{< /highlight >}}

The `name` field is the key, and the `value` field is the value. Together they form a key-value pair to facilitate automated data lookup. Here we use an optional `class` field to provide an additional layer of information, noting that the marking we are using is the TLP protocol.  The [Props and Links tutorial](/learn/tutorials/extensions/#using-an-existing-prop) provides more details on these fields.

{{% /tab %}}
{{% tab %}}
The `props` array provides any number of key-value pairings to provide additional information alongside the well-defined fields of the metadata section. Some common use cases include labels, sorting tags, and classification. There are no restrictions on using custom `name` and `value` pairs making `prop` an excellent place to do extension of the OSCAL model to meet proprietary use cases. 

{{% callout note %}}
{{% /callout %}}

For this example, we will create a `props` to mark an OSCAL document with a [Traffic Light Protocol (TLP)](https://en.wikipedia.org/wiki/Traffic_Light_Protocol) classification.

{{< highlight yaml "linenos=table" >}}
  props:
  -  name: marking
     value: red
     class: TLP
{{< /highlight >}}

The `name` field is the key, and the `value` field is the value. Together they form a key-value pair to facilitate automated data lookup. Here we use an optional `class` field to provide an additional layer of information, noting that the marking we are using is the TLP protocol.  The [Props and Links tutorial](/learn/tutorials/extensions/#using-an-existing-prop) provides more details on these fields.

{{% /tab %}}
{{% /tabs %}}

#### Links

{{< tabs XML JSON YAML >}}
{{% tab %}}
Alongside the `<prop>` element, `<link>` is a way of providing additional information in the metadata section that is not covered by the other elements. Each `<link>` must contain a single `@href` attribute, which contains a resolvable URI. Each `<link>` should also include a `@rel` attribute, which describes the type of relationship provided by the `@href`.  

{{% callout note %}}
The [Props and Links tutorial](/learn/tutorials/extensions/#links) provides comprehensive examples of how to implement a `<link>`.
{{% /callout %}}

For this example, we will create a "latest-version" link, which provides a static link to the latest version of this Document Series. By pointing to a static URL, but updating the contents of that URL as the document is updated, any user that has an old version of the document can quickly and easily update.

{{< highlight xml "linenos=table" >}}
<link rel="latest-version"
      href="https://www.examplecompany.com/catalog/exampleoscalcatalog/latest"/>
{{< /highlight >}}

The "predecessor-version" and "successor-version" links could be used in tandem with the above to create a navigable web of document versions.

By using other custom `@rel` values, any relevant or related resource can be described and linked to in the OSCAL document's metadata section.

{{% /tab %}}

{{% tab %}}
Alongside the `props` array, the `links` array is a way of providing additional information in the Metadata section that is not covered by the other fields. Each object inside the `links` array must contain a single `href` field, which contains a resolvable URI. Each object should also include a `rel` field, which describes the type of relationship provided by the `href`. 

{{% callout note %}}
The [Props and Links tutorial](/learn/tutorials/extensions/#links) provides comprehensive examples of how to implement a `link`.
{{% /callout %}}

For this example, we will create a "latest-version" link, which provides a static link to the latest version of this Document Series. By pointing to a static URL, but updating the contents of that URL as the document is updated, any user that has an old version of the document can quickly and easily update.

{{< highlight json "linenos=table" >}}
{  
  "links": [
    {
    "rel":"latest-version",
    "href":"https://www.examplecompany.com/catalog/exampleoscalcatalog/latest"
    }
  ]
}
{{< /highlight >}}

The "predecessor-version" and "successor-version" links could be used in tandem with the above to create a navigable web of document versions.

By using other custom `rel` values, any relevant or related resource can be described and linked to in the OSCAL document's metadata section.

{{% /tab %}}

{{% tab %}}
Alongside the `props` array, the `links` array is way of providing additional information in the Metadata section that is not covered by the other fields. Each object inside the `links` array must contain a single `href` field, which contains a resolvable URI. Each object should also include a `rel` field, which describes the type of relationship provided by the `href`. 

{{% callout note %}}
The [Props and Links tutorial](/learn/tutorials/extensions/#links) provides comprehensive examples of how to implement a `link`.
{{% /callout %}}

For this example, we will create a "latest-version" link, which provides a static link to the latest version of this Document Series. By pointing to a static URL, but updating the contents of that URL as the document is updated, any user that has an old version of the document can quickly and easily update.

{{< highlight yaml "linenos=table" >}}
  links:
  - rel: latest-version
    href: https://www.examplecompany.com/catalog/exampleoscalcatalog/latest
{{< /highlight >}}

The "predecessor-version" and "successor-version" links could be used in tandem with the above to create a navigable web of document versions.

By using other custom `rel` values, any relevant or related resource can be described and linked to in the OSCAL document's metadata section.

{{% /tab %}}
{{% /tabs %}}

### Using Other Optional Fields

{{< tabs XML JSON YAML >}}
{{% tab %}}

The remainder of this tutorial briefly covers the other optional elements inside the metadata section. While not required by the specification, these elements provide invaluable information, particularly for provenance and authorship data. This information is declared in the metadata section, and is often passed by-reference to other parts of the OSCAL document. Each element can be clicked to get additional information.

[`<role>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/role) - Roles define some function or purpose that is to be assigned to some entity later in the document. Role elements have an `id` attribute with a [NCName](/reference/datatypes/#ncname) that is used to reference the role elsewhere in the OSCAL document. A number of pre-defined roles exist in OSCAL, but differ depending on context. In the metadata section they are as follows:

- prepared-by: Indicates the organization that created this content.
- prepared-for: Indicates the organization for which this content was created.
- content-approver: Indicates the organization responsible for all content represented in the "document".
Other roles can be locally defined by the content creator.

[`<location>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/location) - Geographic data defined by a street address. Locations have a `@uuid` attribute that allow them to be referenced elsewhere in the OSCAL document. Includes elements to describe a variety of data describing the location in question.

[`<party>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/party) - Defines some entity, either a person or an organization. Has a `@uuid` attribute that allows for references to this party elsewhere in the OSCAL document. Includes elements to describe a variety of data describing the party in question, including a location uuid.

[`<responsible-party>`](/reference/latest/catalog/xml-reference/#/catalog/metadata/responsible-party) - Explicitly declares a party that is responsible for a given role. The `@role-id` attribute references the role that the party is fulfilling, and is either a custom role locally defined or one of the core-defined roles; see the `<role>` section above for details. Uses a party's uuid to link the given role to the given party.

[`<remarks>`]((/reference/latest/catalog/xml-reference/#/catalog/metadata/remarks)) - [Markup](/reference/datatypes/#markup-data-types) formatted plaintext with notes and remarks regarding the metadata section of this document.
{{% /tab %}}

{{% tab %}}

The remainder of this tutorial will briefly cover the other optional objects inside the metadata section. While not required by the specification, these objects provide invaluable information, particularly for provenance and authorship data. This information is declared in the metadata section, and is often passed by-reference to other parts of the OSCAL document. Each object can be clicked to get additional information.

[`roles`](/reference/latest/catalog/json-reference/#/catalog/metadata/roles) - An array of Role objects. Roles define some function or purpose that is to be assigned to some entity later in the document. Role objects have an `id` field with a [NCName](/reference/datatypes/#ncname) that is used to reference the role elsewhere in the OSCAL document. A number of pre-defined roles exist in OSCAL, but differ depending on context. In the metadata section they are as follows:

- prepared-by: Indicates the organization that created this content.
- prepared-for: Indicates the organization for which this content was created.
- content-approver: Indicates the organization responsible for all content represented in the "document".
Other roles can be locally defined by the content creator.

[`locations`](/reference/latest/catalog/json-reference/#/catalog/metadata/locations) - An array of location objects. Geographic data defined by a street address. Locations have a `uuid` field that allow them to be referenced elsewhere in the OSCAL document. Includes fields to describe a variety of data describing the location in question.

[`parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/parties) - An array of party objects. Defines some entity, either a person or an organization. Has a `uuid` field that allows for references to this party elsewhere in the OSCAL document. Includes fields to describe a variety of data describing the party in question, including a location uuid.

[`responsible-parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/responsible-parties) - An array of responsible-party objects. Explicitly declares a party that is responsible for this a given role. The `role-id` field references the role that the party is fulfilling, and is either a custom role locally defined or one of the core-defined roles; see the `roles` section above for details. Uses a party's uuid to link the given role to the given party.

[`remarks`](/reference/latest/catalog/json-reference/#/catalog/metadata/remarks) - [Markup](/reference/datatypes/#markup-data-types) formatted plaintext with notes and remarks regarding the metadata section of this document.
{{% /tab %}}

{{% tab %}}

The remainder of this tutorial will briefly cover the other optional objects inside the metadata section. While not required by the specification, these objects provide invaluable information, particularly for provenance and authorship data. This information is declared in the metadata section, and is often passed by-reference to other parts of the OSCAL document. Each object can be clicked to get additional information.

[`roles`](/reference/latest/catalog/json-reference/#/catalog/metadata/roles) - Roles define some function or purpose that is to be assigned to some entity later in the document. Role objects have an `id` field with a [NCName](/reference/datatypes/#ncname) that is used to reference the role elsewhere in the OSCAL document. A number of pre-defined roles exist in OSCAL, but differ depending on context. In the metadata section they are as follows:

- prepared-by: Indicates the organization that created this content.
- prepared-for: Indicates the organization for which this content was created.
- content-approver: Indicates the organization responsible for all content represented in the "document".
Other roles can be locally defined by the content creator.

[`locations`](/reference/latest/catalog/json-reference/#/catalog/metadata/locations) - Geographic data defined by a street address. Locations have a `uuid` field that allow them to be referenced elsewhere in the OSCAL document. Includes fields to describe a variety of data describing the location in question.

[`parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/parties) - Defines some entity, either a person or an organization. Has a `uuid` field that allows for references to this party elsewhere in the OSCAL document. Includes fields to describe a variety of data describing the party in question, including a location uuid.

[`responsible-parties`](/reference/latest/catalog/json-reference/#/catalog/metadata/responsible-parties) - Explicitly declares a party that is responsible for a given role. The `role-id` field references the role that the party is fulfilling, and is either a custom role locally defined or one of the core-defined roles; see the `role` section above for details. Uses a party's uuid to link the given role to the given party.

[`remarks`](/reference/latest/catalog/json-reference/#/catalog/metadata/remarks) - [Markup](/reference/datatypes/#markup-data-types) formatted plaintext with notes and remarks regarding the metadata section of this document.
{{% /tab %}}

{{% /tabs %}}

## Putting It All Together

Finally, we have a basic example of an OSCAL metadata section below:

{{< tabs XML JSON YAML >}}
{{% tab %}}
{{< highlight xml "linenos=table" >}}
<catalog uuid="c3da6d1d-c20c-4c7c-ae73-4010167a186b">
  <metadata>
      <title>Example OSCAL Document</title>
      <published>2021-01-01T00:00:00-5:00</published>
      <last-modified>2021-01-05T00:00:00-5:00</last-modified>
      <version>1.0.0</version>
      <oscal-version>1.0.4</oscal-version>
      <document-id scheme="https://www.doi.org/">10.1000/182</document-id>
      <prop name="marking" class="TrafficLightProtocol" value="red"/>
      <link rel="latest-version"
        href="https://www.examplecompany.com/catalog/exampleoscalcatalog/latest"/>
      <party uuid="15d2e37c-0452-4695-9c6a-ddc1ff15397b" type="organization">
        <name>Example Company</name>
      </party>
      <responsible-party role-id="prepared-by">
        <party-uuid>15d2e37c-0452-4695-9c6a-ddc1ff15397b</party-uuid>
      </responsible-party>
  </metadata>
  ...
</catalog>
{{< /highlight >}}
{{% /tab %}}
{{% tab %}}
{{< highlight json "linenos=table" >}}
{
  "catalog": {
    "uuid": "c3da6d1d-c20c-4c7c-ae73-4010167a186b",
    "metadata": {
      "title":"Example OSCAL Catalog",
      "published":"2021-01-01T00:00:00-5:00",
      "last-modified":"2021-01-05T00:00:00-5:00",
      "version":"1.0.0",
      "oscal-version":"1.0.4",
      "document-ids": [
        {
        "scheme":"https://www.doi.org/",
        "value":"10.1000/182"
        }
      ],
      "props": [
        {
        "name":"marking",
        "value":"red",
        "class":"TLP"
        }
      ],
      "links": [
        {
        "rel":"latest-version",
        "href":"https://www.examplecompany.com/catalog/exampleoscalcatalog/latest"
        }
      ],
      "parties": [
        {
          "uuid":"15d2e37c-0452-4695-9c6a-ddc1ff15397b",
          "type":"organization",
          "name":"Example Company"
        }
      ],
      "responsible-parties":[
        {
          "role-id":"prepared-by",
          "party-uuid":"15d2e37c-0452-4695-9c6a-ddc1ff15397b"
        }
      ]
    }
  }
}
{{< /highlight >}}
{{% /tab %}}
{{% tab %}}
{{< highlight yaml "linenos=table" >}}
---
  catalog:
    uuid: c3da6d1d-c20c-4c7c-ae73-4010167a186b
    metadata:
      title: Example OSCAL Catalog
      published: 2021-01-01T00:00:00-5:00
      last-modified: 2021-01-05T00:00:00-5:00
      version: 1.0.0
      oscal-version: 1.0.4
      document-ids:
      - scheme: https://www.doi.org/
        value: 10.1000/182
      props:
      - name: marking
        value: red
        class: TLP
      links:
      - rel: latest-version
        href: https://www.examplecompany.com/catalog/exampleoscalcatalog/latest
      parties:
      - uuid: 15d2e37c-0452-4695-9c6a-ddc1ff15397b
        type: organization
        name: Example Company
      responsible-parties:
      - role-id: prepared-by
        party-uuid: 15d2e37c-0452-4695-9c6a-ddc1ff15397b
{{< /highlight >}}
{{% /tab %}}
{{% /tabs %}}

## Summary

This concludes the tutorial. At this point you should be familiar with:

- The basic structure of the metadata section.
- How to provide the basic metadata required to be included in an OSCAL document.
- How to use and understand UUIDs and document-ids to track document instances
- How to use optional fields to express additional metadata and extend the metadata section
