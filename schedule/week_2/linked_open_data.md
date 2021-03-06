# Linked Open Data

(summarized from https://www.w3.org/DesignIssues/LinkedData.html) 
1. Publish your data with an open license
2. Use machine-readable formats
3. Better, use machine-readable *open* formats
4. And use open web standards to describe your data
5. And best of all, do all the above and link to other people's data too

`#4 explicitly calls out the standards around the Resource Description Framework (RDF) and the query language defined for it, SPARQL. This are powerful and useful tools, but it's important not to put the cart before the horse. You should get your data in good shape before you start thinking about modeling it in RDF.

## The Foundations of Linked Open Data

* Open Licenses 
  * Can people use our data once they've got it? Can they re-publish it? Build on it?
* Stable URIs
  * Can people link to our resources and expect they will stay available?
* Open Standards 
  * Will users be able to understand and use the formats I publish in for the foreseeable future?
* Referenceable Data Resources 
  * Can people link to the parts of the data they're interested in? Is everything hidden behind a search interface?

## Open Licenses

Creative Commons licenses are generally good for content. The less restrictive, the better. Obviously, this hinges on copyright and what you're actually allowed to do with the data. Remember, you have to own the Copyright (or have the owner's permission) in order to be able to apply a license. You should think about the code as well as the data. There are licenses that work better for code, such as the GNU GPL, Apache, or ?

These are cultural, political decisions more than technological ones. What will the content owner allow? What will your institution allow? Funders will often insist on open licenses.


## Stable URIs

"Cool URIs don't change." (https://www.w3.org/Provider/Style/URI) 

The linking part of this requires that we spend some time understanding how links work. It's not as simple as it may at first appear. URIs are Uniform Resource Identifiers—they *identify*, that is, name, resources on the Web. They may do this by giving us an address, in which case they are Locators (URLs) or by simply naming the resource (URNs). URIs deal only in basic ASCII text, so if you want, say, a Greek character in your URI, you have to escape it. RDF technologies add another layer with *Internationalized* Resource Indentifiers (IRIs), which can have Unicode characters in them. If we're going to link to things, we want a) there to be web resources we can link to—so that we can retrieve them by clicking on a link and b) we want those links to persist. If they're going to last, we probably want the form of our URIs not to be too dependent on the technology used to present them, which has some design implications.

### URLs
#### `https://digitallatin.github.io/guidelines/LDLT-Guidelines.html#apparatus-criticus`

|protocol|server name|path|fragment identifier|
|--------|-----------|----|-------------------|
|https|digitallatin.github.io|/guidelines/LDLT-Guidelines.html|apparatus-criticus|

This example links to a particular section of an HTML document. The protocol tells the client (whether that be a web browser or something else) how to retrieve it, the server name tells it what system to ask for the resource, the path, what to ask that system for. As far as the server is concerned, that's it. It ignores the fragment identifier part. If you're using a web browser, it will probably scroll to that section of the document, but what to do with that part of the URL is a client concern. You may have used web applications call "single page applications" where what you see in the browser is entirely dependent on the content of the fragment identifier. What this means is that there is code running in the browser that either fetches additional data or decides from the data it has what to display based on the fragment id. This is ok for someone using a browser, but less so for other types of client. To an archiving service, for example, a single-page app looks like just that, a single web page. Not a rich set of web resources.

#### `http://papyri.info/search?STRING=(στρατηγ)&no_caps=on&no_marks=on&target=text&DATE_MODE=LOOSE&DOCS_PER_PAGE=15`
|protocol|server name|path|query parameters|
|--------|-----------|----|-------------------|
|http|papyri.info|/search|STRING=(στρατηγ), no_caps=on, no_marks=on, target=text, DATE_MODE=LOOSE, DOCS_PER_PAGE=15|

This example shows the use of query parameters in a URL. These *are* processed by the server. They're very common in search interfaces, such as the one above. In essence, the client is sending some information to the server, which is using it to tailor the response it sends. Linked Data URLs tend to look more like a) `https://example.com/resource/123` than b) `https://example.com/resource?id=123`. Partly, this is just convention. What happens on the server before a request is answered is totally opaque to the client. The server could be simply reading a file off disk and sending the data to the client, or it could be retrieving data from one or more databases, combining it, and then sending that. In general though, URLs with parameters like #b tend to be used for functions like search, and URLs like #a tend to be used for naming and retrieving resources.

### URNs
#### `urn:cts:latinLit:phi0830.phi001`

URNs uniquely name resources, but they don't contain within them instructions for retrieving the named resources. So they have to be plugged in to some sort of resolution service for you to actually get the thing they name. That doesn't stop them from being useful as identifiers though. And sometimes you want to talk about things that are not web resources. The URN above uses the [CTS](http://www.homermultitext.org/hmt-doc/cite/cts-urn-overview.html) naming scheme to identify the *Eclogues* of a Latin poet named Calpurnius Siculus. There are several editions of these, some available on the web, but this doesn't name a particular one of those. It refers to the abstract idea or group of physical and digital works with this author and name.

So, to sum up, URIs can be used not just to get resources on the web, but to uniquely identify them too. We want resource URIs to persist, because no-one likes broken links, and so it's a good idea to think a bit about how to design them so that they're independent of the underlying implementation. This is partially a technological question, but has more to do with long-term sustainability and management. Having stable URIs means having control over your web infrastructure and being able to do things like set up redirects from one URI to another. Again, there are political and cultural questions here. Will your system admins cooperate and let you design your URIs? Will the stakeholders be able to agree on what data gets a URI?

## Open Standards

Standards come into play throughout LOD, starting with the formats for storing data and continuing though the protocols and formats used to deliver data.

### Examples
* Formats
	* text: TEI, plain text, HTML 
  * images: JPEG, TIFF, PNG 
  * tabular data: CSV
* Protocols and delivery formats
	* HTTP/S (GET rather than POST) 
  * XML 
  * JSON-LD

Standards are definitely a technological question, but again, other factors come into play that may influence your decisions.

## Referenceable Resources

This is an information design question. What things do you want to represent in your website? Ideally, everything you might want to refer to should have its own (stable) URL.

### Examples
http://example.com/person/123 (info about a person)
http://example.com/document/456 (a document)
http://example.com/document/456#ch1 (section in a larger document) or maybe http://example.com/document/456/ch1 (chunked large document with referenceable sections)

Another important design question has to do with the "opacity" of your URIs. Should you refer to things using a human-readable name or a random id? Compare http://papyri.info/ddbdp/p.fay;;110 (P.Fay 110 is a citation to an edition of a papyrus in *Fayum Towns and their Papyri*, ed. B.P. Grenfell, A.S. Hunt and D.G. Hogarth. London 1900.) to http://papyri.info/hgv/10775. These resolve to the same document, which aggregates information about the papyrus. http://papyri.info/ddbdp/p.fay;;110 is the edition, and http://papyri.info/hgv/10775 provides data about the source document and the edition. The Duke Databank of Documentary Papyri (DDbDP) has human-legible identifiers and the Heidelberger Gesamtverzeichnis der griechischen Papyrusurkunden Ägyptens (HGV) uses opaque numbers. There's not a right or wrong answer here, but note that the DDbDP has to mint and redirect to a new URI when a new edition comes out, and HGV just has to update a record.





## A Recipe for Usable Linked Open Data

1. Get your data in shape
  1. How good is it? Do you have the metadata you need? Is it licensed appropriately?
  2. Will it be updated? Should you version it?
2. Think about how people will get and use your data
  1. Big chunks? 
  2. Individual records/documents?
  3. Both? It's common to provide data both as records or documents and in one big download.
3. Think about how users will navigate your data
  1. Links, both internal and external
  2. What kinds of links will you have?
4. NOW you can think about semantic web stuff


#### Exercise: look at <http://papyri.info/ddbdp/p.fay;;110>
 1. Open a terminal and try the following commands:
 2. `curl -v -L -H "Accept: text/turtle" "http://papyri.info/ddbdp/p.fay;;110"`
 3. `curl -v -L -H "Accept: text/turtle" "http://papyri.info/ddbdp/p.fay;;110/source"`

 ## RDF
 
 The command uses a program named [curl](https://curl.haxx.se/); the option `-v` tells it to be verbose (i.e. tell you what it's doing), so it will both log what it does and display the request and response headers; `-L` tells it follow redirects—if the server says "what you want is actually over *here*, curl will automatically get the resource at the new location; `-H "Accept: text/turtle"` tells it to send a request header informaing the server what format it wants as a response; and finally gives the URL to retrieve the data from. HTTP(S) communication takes the form of a header and a body. 

The response is in the form of an RDF *serialization* (a format for writing out the data) called Turtle. So we should talk about RDF a bit. RDF represents "facts" using lists of three-part statements. Statements consist of subject, predicate, object "triples", where the subject and predicate are URIs, and the object may be a URI or a "Literal" (some data, like a number or a string). The response you got back from #2 should look like 

```
@prefix dc:      <http://purl.org/dc/terms/> .
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix foaf:    <http://xmlns.com/foaf/0.1/> .
@prefix oa:      <http://www.openannotation.org/ns/> .
@prefix cito:    <http://purl.org/spar/cito/> .
@prefix rdf:     <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix lawd:    <http://lawd.info/ontology/> .

<http://papyri.info/ddbdp/p.fay;;110>
      foaf:topic    <http://papyri.info/ddbdp/p.fay;;110/source> ;
      foaf:topic    <http://papyri.info/hgv/10775/source> .

<http://papyri.info/ddbdp/p.fay;;110/annotation/1ada6e40334262008d8ca4840e01ef9f>
      oa:hasTarget  <http://papyri.info/ddbdp/p.fay;;110> .

<http://papyri.info/ddbdp/p.fay;;110/source>
      foaf:page     <http://papyri.info/ddbdp/p.fay;;110> .

<http://papyri.info/hgv/10775/source>
      foaf:page     <http://papyri.info/ddbdp/p.fay;;110> .
```

The prefixes are just a way to shorten commonly-used URIs. The predicates (or properties) in RDF tend to be repeated a lot, so we shorten them. So what is this saying? First, `http://papyri.info/ddbdp/p.fay;;110` is about (has as a `foaf:topic`, or `http://xmlns.com/foaf/0.1/topic` to give it its expanded form) `http://papyri.info/ddbdp/p.fay;;110/source` and `http://papyri.info/hgv/10775/source`. These two both have the same web page, `http://papyri.info/ddbdp/p.fay;;110`. And there's also an annotation, `http://papyri.info/ddbdp/p.fay;;110/annotation/1ada6e40334262008d8ca4840e01ef9f`, which also points at `http://papyri.info/ddbdp/p.fay;;110`. That's it. Just a list of facts. If we go ahead an interrogate one of the objects of `foaf:topic`, as example #3 did, we see more information. <http://papyri.info/ddbdp/p.fay;;110> is a web page that aggregates information from several sources, and <http://papyri.info/ddbdp/p.fay;;110/source> is one of those sources, a TEI edition. It has `dc:relation`s to the other documents aggregated with it, one of which, <http://www.trismegistos.org/text/10775>, is located on another website. It is `dc:isPartOf` the collection corresponding to the print volume in which it was originally published, <http://papyri.info/ddbdp/p.fay>. 

As we follow these chains of information around, we can start to see that RDF is a graph. Subjects in one triple can be objects in another, and vice-versa. Properties can be subjects or objects too, and in fact this is how RDF schemas work, by defining the properties and types of entities in an RDF graph. Because RDF triples have *direction* (subject, predicate, object), the kind of graph they instantiate is called a *directed* graph. And, because the URIs we're using in our RDF graph are also names for web resources, we're tying everything on our website together in a machine-readable way.

## Some Linked Data Sites

### Pleiades <https://pleiades.stoa.org/>
Pleiades is a digital gazetteer of the ancient world
 * See, e.g. <https://pleiades.stoa.org/places/736909>

### Geonames <http://www.geonames.org/>

### Pelagios <http://commons.pelagios.org/>
Pelagios focuses on linking distributed resources around geographic locations

### Nomisma <http://nomisma.org/>
 * e.g. <http://nomisma.org/id/ephesus>

### Perseus Catalog <http://catalog.perseus.org/>
 * e.g. <http://catalog.perseus.org/catalog/urn:cts:latinLit:phi0830.phi001>
 * and <http://catalog.perseus.org/catalog/urn:cite:perseus:author.317>

### Perio.do <http://perio.do/>
 * A gazetteer of period definitions for linking and visualizing data

### VIAF
 * e.g. <http://viaf.org/viaf/100165075> (Calpurnius Siculus again)

## Talk Lab: What are the things you want to name? (20 minutes)

## Vocabularies and RDF

### Some examples:

Dublin Core—basic metadata <http://dublincore.org/documents/dcmi-terms/>
 * `dcterms: <http://purl.org/dc/terms/>`

CiTO—citation typing <http://www.sparontologies.net/ontologies/cito/source.html>
 * `cito: <http://purl.org/spar/cito/>`

FOAF—Friend of a friend, linking people and web stuff <http://xmlns.com/foaf/spec/>
 * `foaf: <http://xmlns.com/foaf/0.1/>`

SKOS—making taxonomies <https://www.w3.org/2004/02/skos/>
 * `skos: <http://www.w3.org/2004/02/skos/core#>`

LAWD—I got angry and made my own <https://github.com/lawdi/LAWD>
 * `lawd: <http://lawd.info/ontology/>`

## Discussion: How are your things related? (20 minutes)

## RDF, SPARQL
 * Formats: N3, Turtle, RDF/XML
 * Try the following in the terminal:
  * `curl -L -H "Accept: text/n3" "http://papyri.info/ddbdp/p.fay;;110/source"`
  * `curl -L -H "Accept: application/rdf+xml" "http://papyri.info/ddbdp/p.fay;;110/source"`
  * and, just to compare: `curl -L -H "Accept: text/turtle" "http://papyri.info/ddbdp/p.fay;;110/source"`

### SPARQL Endpoints:
Nomisma: <http://nomisma.org/sparql>
The British Museum: <http://collection.britishmuseum.org/sparql>

### Wrap up. Don't overdo it!

<http://blogs.library.duke.edu/dcthree/2013/07/27/the-trouble-with-triples/>
