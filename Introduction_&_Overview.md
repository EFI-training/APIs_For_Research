# Introduction & Overview

## Table of Contents

- [What is an API?](#what-is-an-api)
- [Web APIs & Software libraries](##web_apis_&_software_library_apis)
- [API types](##api_types)
- [REST APIs](#rest-apis)
- [Example APIs](####-example-apis)
- [Documentation](#-documentation)
- [File Formats](#-file-formats)
- [Use cases for Research](#-use-cases-for-research)
- [Further resources](#-resources)

# APIs for Research

Application programming interface (API)s are communication tools between software platforms, that allow different programs to interoperate with each other.
In digital humanities research, they are regularly used for accessing and retrieving data from digital collections with open access (archives, libraries, museums) and linking these entities to ongoing research. 

APIs have many use cases outside of this for relevant parties like developers, institutions wishing to create and share resources and so forth, we will mention these but not delve into them. The focus of the workshop is applied uses of APIs for researchers and practical guidance for interacting with existing APIs and collecting data. 

# What is an API?

An API is essentially a set of rules and endpoints that let software retrieve or interact with data from a remote system without needing to scrape websites or manually download files. The URLs we examine are an important feature in using APIs when accessing web APIs specifically.

Each endpoint typically corresponds to a certain type of resource or function (e.g.
an endpoint for “objects” might return a list of objects in a collection). The server responds with a representation of the data, usually in a structured format like JSON (JavaScript Object Notation), which is a lightweight text format for data exchange.

# Why use an API instead of browsing a website?
A website is usually designed for people to browse and read. A web API is designed for software to request information in a predictable, machine-readable form. This makes it useful for:
Collecting records systematically rather than copying information page by page.
Repeating a search or updating a dataset when new records appear.
Combining information from different sources using stable identifiers, such as DOIs or ORCID iDs.
Building reproducible workflows that connect data collection, cleaning, analysis and visualisation.
Creating new services or interfaces for a particular research community.
However, an API is not always the best choice. For very large datasets, a downloadable archive or data dump may be more efficient and better aligned with the provider's terms of use. Always check the options offered by the source.

## Web APIs & Software Library APIs

APIs can be sorted into two categories: Web based, and software library based.

# How a web API works
In a typical client–server interaction, your program (the client) sends a request over the internet to a service (the server). The server checks the request, retrieves or processes the relevant information and returns a response. The best way to work with a specific API that you are aware of is to look at the documentation, the way a request is made, and the categories available is typically described in the documentation. The below is an example

```text
Researcher / Python notebook
          |
          |  HTTP request: GET /objects?keyword=maps
          v
   Web API endpoint
          |
          |  Validates request, permissions and inputs
          v
 Service logic / database
          |
          |  Structured HTTP response (often JSON)
          v
 Researcher / Python notebook
          |
          v
 Cleaning -> analysis -> interpretation -> documented outputs
```


### Understanding the components of an API request

When interacting with a web API, several components determine where a request is sent, what information is requested, and how the server responds.

| Component | What this is | Example |
|---|---|---|
| **Base URL** | The main address of the API service. | `https://api.example.org` |
| **Endpoint / path** | The specific resource or operation you want to access. | `/v1/objects` |
| **HTTP method** | Indicates what action you want to perform. | `GET` (retrieve), `POST` (submit) |
| **Path parameter** | Identifies a particular resource within the URL. | `/v1/objects/123` |
| **Query parameter** | Provides options for searching, filtering, sorting or limiting results. | `?q=maps&limit=10` |
| **Request headers** | Supply additional information about the request, such as accepted formats or credentials. | `Accept: application/json` |
| **Authentication** | Verifies the caller's identity or permissions when access is restricted. | `Authorization: Bearer <token>` |
| **Request body** | Contains information sent to the server, often with a `POST` request. | JSON containing a prompt or new record |
| **Response** | The server's reply, including a status code, headers and usually returned data. | `200 OK` with JSON data |

#### Putting the components together

Consider this example URL:

`https://api.example.org/v1/objects?q=maps&limit=10`

It contains:

- **Base URL:** `https://api.example.org`
- **Endpoint:** `/v1/objects`
- **Query parameter 1:** `q=maps` requests results matching "maps".
- **Query parameter 2:** `limit=10` requests a maximum of 10 results.

The question mark (`?`) marks the beginning of the query parameters. The ampersand (`&`) separates individual parameters.

These parameters are interpreted according to the API's documentation.

**Tip:** In Python, the `requests` library can construct and encode query parameters for you, reducing mistakes when building URLs manually.


### API types
> A non-exhaustive list 
GraphQL: Client-defined queries for precise data retrieval.
SPARQL: Query language for RDF and semantic web data.
REST: Web service style using HTTP methods for resource access

#### Example APIs
1. Europeana: Online portal to Europe’s cultural heritage collections.
2. Open Library: Open, editable catalog of books from around the world.
3. ORCID: Persistent researcher ID system for scholarly identity.
4. Crossref: Organisation providing DOIs and metadata for scholarly works.
5. Smithsonian: U.S. group of museums and research centers with vast collections.
6. Victoria & Albert Museum: UK museum focused on art, design, and decorative arts.
7. The British Library: National library of the UK with extensive global collections.


### Other useful things to know
- IIIF: A standardised framework for delivering high-quality images and metadata, allowing museums, libraries, and archives to share, compare, and display digital images using common APIs

### REST APIs
REST (Representational State Transfer) is a popular architectural style for web services. A REST API is an interface that uses standard HTTP protocols (the same that power the web) to allow clients to retrieve or manipulate data. In practice, this means the API exposes one or more endpoints – specific URLs that represent data resources – and clients make HTTP requests (like GET or POST requests) to those endpoints.

## Approaches and Standards

Web APIs do not all work in the same way. Different approaches, protocols and standards have been developed to support particular ways of accessing, exchanging and organising information.

Some are designed for general-purpose communication between software systems, while others support specialised research needs, such as retrieving cultural heritage images, accessing scholarly metadata or querying linked datasets.

The following approaches are not interchangeable, and a single service may support more than one.

| Approach or standard | Main idea | Research relevance |
|---|---|---|
| **REST** (Representational State Transfer) | An architectural style that commonly uses URLs to identify resources and HTTP methods to interact with them. | Frequently used for museum collections, scholarly metadata and government datasets. |
| **GraphQL** | A query language and runtime that allows users to request specific fields from structured data. | Useful when working with complex or interconnected datasets containing many fields. |
| **SPARQL** | A query language for retrieving and exploring information represented using RDF (Resource Description Framework). | Useful for knowledge graphs, linked open data, controlled vocabularies and semantic-web collections. |
| **OAI-PMH** (Open Archives Initiative Protocol for Metadata Harvesting) | A protocol that allows systems to collect structured metadata records from repositories. | Widely used by institutional repositories, scholarly databases and metadata aggregators. |
| **IIIF** (International Image Interoperability Framework) | A set of interoperability standards for delivering images and describing digital objects. | Particularly useful for manuscripts, artworks, historical maps, photographs and digitised collections. |
| **SOAP** (Simple Object Access Protocol) | An XML-based messaging protocol used to exchange structured information between software systems. | Sometimes encountered in established institutional, enterprise and legacy systems. |

### Can an API use more than one approach?

Yes. These technologies serve different purposes and may be combined within the same digital infrastructure.

For example, a museum or digital archive might provide:

- A **REST API** for searching its collection and retrieving object metadata.
- **IIIF services** for displaying high-resolution images of manuscripts or artworks.
- A **SPARQL endpoint** for exploring relationships between objects, creators, places and historical periods.
- **OAI-PMH** for allowing other institutions to harvest its collection metadata.
### Status codes
Can spell trouble or success. Status codes when working with HTTPS based API requests let you know what the status of a given request is. Here are some examples:

- 200 - OKAY
- 404 - Not found
- 403 - Forbidden

Status codes are explained further in Documentation (See below), and are useful to quote for crowdsourced troubleshooting.

# Documentation
Documentation forms the user manual for an Application Programming Interface. They provide clear, structured instructions that explain how to use an API. It typically includes:
What the API does;
Available endpoints or functions;
Required inputs (parameters);
What the outputs look like;
Examples of requests and responses

In practice:
- Specific coding packages for R and Python that are built for accessing APIs have their own documentation that is required reading
E.g. tubeR - an R package for accessing the Youtube Data API; python-youtube - a Python Package for the same.


# File formats 

## JSON JavaScript Object Notation, XML Extensible Markup Language and RDF
J-SON are a common file format for configuration, APIs, and data exchange

```json
{
  "name": "Alice",
  "age": 25,
  "languages": ["English", "Spanish"]
}
```


XML is a text based file format 
A text-based format for storing and transporting structured data using custom tags. It’s both human-readable and machine-readable, and is commonly used in web services, configuration files, and data exchange.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Person>
    <Name>Alice</Name>
    <Age>25</Age>
    <Languages>
        <Language>English</Language>
        <Language>Spanish</Language>
    </Languages>
</Person>
```


RDF (Resource Description Framework) File Format

RDF is a standard model for representing structured information about resources on the web. It expresses data as triples: subject – predicate – object, which makes it easy to link and integrate data across different sources
It is ideal for describing relationships between resources, e.g., authors, books, and institutions
For example: 

```turtle
@prefix ex: <http://example.org/> .

ex:Alice ex:hasAge 25 .
ex:Alice ex:speaksLanguage ex:English .
ex:Alice ex:speaksLanguage ex:Spanish .
```
# Working with API documentation

API documentation is the technical and practical guide to using a service. Before writing a script, look for:
1. Purpose and coverage: What data or operations does the service expose? What does it omit?
2. Base URL and API version: Are you using a current endpoint or a deprecated one?
3. Endpoints and methods: Which routes exist, and what does each return?
4. Parameters: Which inputs are required? How do filtering, sorting and pagination work?
5. Authentication and permissions: Is a key, token, OAuth flow or institutional approval required?
6. Examples and response schemas: What do successful and failed requests look like?
7. Limits and conditions: Rate limits, quotas, licences, attribution, privacy and redistribution.
8. Maintenance: Change logs, service status, contact details and support channels.


# Use cases for Research

Public APIs are the most ideal and those issued by established institutions, public and private have formed incredible resources for researchers.

- (GLAM) - there has been a steady rise in digitisation in the public sector and many organisations - national and regional - offer APIs and open digital resources.
- The NHS - The NHS offers multiple different APIs for aggregated healthcare data 
- Government portals - UK regional and national governments offer API data on different socio-economic, climatic and other data through different organisations. 

In addition - private enterprise use APIs and many social media companies offer these to researchers. Many require a research application and are costed in some cases:
- Google
- META
- X
- TikTok

# Other use cases

Other than generating data, developers  are interested in the integration of multiple APIs to create dynamic environments pulling in data from various sources, adding new features, extrapolating on existing frameworks and general flexibility that APIs offer them.

For example Tools like Dimensions automatically pull a researcher’s publications and citations linked to their ORCID ID.
Google Arts & Culture uses Smithsonian data to showcase artworks, artifacts, and images in digital exhibits.


# Limitations & considerations

Rate limits - The amount of individual calls a program can make to an API. Researchers are often given a quota (e.g. 200 calls every minute) that limits access.
- Note in our workbook we will encounter some of these limits (ie.e the Met museum recommends no more than 80 requests per second)


Running locally versus on cloud based platforms
Locally: Convenient for development and testing; may face network or resource limitations if running on a lite-machine e.g. a notebook

Cloud: Scales better for large datasets and multiple users; may incur costs or require specific permissions. Many Cloud platforms are free or low cost

# How APIs are designed and built
Researchers often use an existing API, but they may also need to create one. A bespoke API makes a dataset, model, process or set of research outputs available through a consistent interface to collaborators or other software.

For example, a research group might want an API that lets authorised collaborators search an annotated oral-history collection, retrieve metadata for a digitised archive or request predictions from a research model. It does not have to expose the underlying database or unpublished information directly.

Behind each endpoint, the server typically performs a sequence of operations:

```text
Request -> validate input -> check permissions -> apply research logic
        -> query database / run model -> format output -> return response
```
For an archive, the research logic might filter records and remove restricted fields. For a computational model, it might validate a supplied input and return a prediction together with a model version and limitations. The API design should make important research context visible

## choices for research APIs
Identifiers and provenance. Stable IDs let other researchers join and cite records. Include the original source, collection date, version, licence and other provenance where appropriate.

Search, pagination and filtering. Decide whether users need individual records, full datasets or reproducible searches. Paginated responses should have clearly documented ordering and continuation rules.
Authentication versus authorisation. Authentication establishes who or what is making the request; authorisation decides what that user or program is allowed to do.
Versioning and change management. Changing field names or endpoint behaviour can break research scripts. Document breaking changes and provide a transition period where feasible.

Metadata and ethical design. An API should not inadvertently reveal personally identifiable, sensitive or otherwise restricted research material. Design privacy and access rules before publishing the service.

Documentation as part of the API. A service is difficult to reuse if others cannot interpret its fields, permitted methods, error messages or terms of use. An OpenAPI specification can help describe the interface consistently.

> Imagine your research group has an annotated collection of 500 publicly shareable oral-history catalogue records. What three endpoints would be most useful to others? What data would you deliberately \*\*not\*\* include, and why?

# Resources
API Documentation

GraphQL Docs
 – Official guide to GraphQL queries.
 - [GraphQL](https://graphql.org/learn)

SPARQL Tutorial
 – Learn querying RDF datasets.
 - [SPARQL](https://www.w3.org/TR/rdf-sparql-query/)

REST Guide
 – Principles and best practices for REST APIs.
 - [REST](https://restfulapi.net)

Digital Libraries & Collections

Europeana Collections
 – Europe’s cultural heritage portal.
 - [Europeana Collections:](https://www.europeana.eu/en)

Open Library
 – Free, editable catalog of books.
 - [Open Library](https://openlibrary.org)

Smithsonian Open Access
 – Access images & metadata from Smithsonian museums.
 - [Smithsonian Open Access](https://www.si.edu/openaccess)

V&A Museum API
 – V&A museum collection data.
 - [V&A Museum API:](https://developers.vam.ac.uk/)

British Library Digital Collections
 – Digitized books, manuscripts, and images.
 - [British Library Digital Collections:](https://bl.iro.bl.uk/collections/64e3804a-788a-4c4b-962c-ae180d955455?locale=en)

Geospatial and environmental data
Spatial features, climate or weather observations and place-based research.
- OpenstreetMap - [link] (https://www.openstreetmap.org/#map=5/54.91/-3.43) -> https://wiki.openstreetmap.org/wiki/Overpass_API
-[OpenMateo weather API](https://open-meteo.com/)

## Research Tools

ORCID
 – Persistent researcher identifiers.
 - [ORCID](https://orcid.org/)

CrossRef
 – Metadata and DOI registry for scholarly works.
 - [CrossRef](https://www.crossref.org/)

Standards & Frameworks

IIIF
 – Standardized APIs for high-quality images.
 - [IIIF](https://iiif.io/)

JSON Guide
 – Overview of JSON structure and usage.
 - [JSON](https://www.json.org/json-en.html)

XML Guide
 – Reference for Extensible Markup Language.
 - [XML:](https://www.w3.org/XML/)

## Developer Tools

Postman
 – API testing and development tool.
 - [Postman](https://www.postman.com/)

Swagger/OpenAPI
 – Tools for designing and documenting APIs
 - [Swagger / OpenAPI](https://swagger.io/)


## Further tools
- Python: `requests` for HTTP requests; `pandas` for tabular data; `FastAPI` for building APIs.
- R: `httr2` for HTTP requests; `jsonlite` for JSON; `plumber` for building APIs.
- Notebook environments (if you prefer to scripts): Jupyter notebooks and VS Code for documenting and testing an analysis alongside its code.
- API exploration: Postman and interactive OpenAPI/Swagger documentation for inspecting requests and responses.







