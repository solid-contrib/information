[Solid Developer Tools](https://github.com/solid/information/blob/master/developer-tools.md)

[Learn Solid](https://github.com/learnsolid) 

# Apps

https://github.com/solid/information/blob/master/app-development.md 

Client development began with pure javascript apps with few or no external dependencies and javascript Angular apps such as [Warp](https://github.com/linkeddata/warp) and [Plume](https://github.com/deiu/solid-plume).  Plume now has different implementations, one of which [uses the Safe network](https://github.com/theWebalyst/solid-plume).  A client library abstracting interaction with pods, called [solid-auth-client](https://github.com/solid/solid-auth-client), was developed.  Originally client apps used a library for [RDF](https://www.w3.org/TR/rdf-primer/) serialization/deserialization and manipulation called [Rdflib.js](https://github.com/linkeddata/rdflib.js/).  A library for HTML5 user interface widgets for Solid, which uses solid-auth-client, was developed, called [solid-ui](https://github.com/solid/solid-ui) which is integrated with RDF using Rdflib.js, and it is used as the default user interface library via the [Data Browser aka Mashlib](https://github.com/linkeddata/mashlib) which is included with Solid pods.

Rdflib.js is still used for many client apps, but another library for RDF has been developed called [Comunica](https://github.com/comunica), which is a modular implementation of a [Triple Pattern Fragments](https://www.hydra-cg.com/spec/latest/triple-pattern-fragments/) client. A Triple Pattern Fragment is data of a size situated midway between a full data dump, which is highly available to a client but comes at the high cost of downloading, and individual [SPARQL](https://www.w3.org/TR/rdf-sparql-query/) queries, which have a high server cost and servers are often unavailable. Triple pattern fragments fill a need to make linked data queries more efficient.  Comunica is a modular framework, and it has an actor implementation that uses solid-auth-client available.

Both Rdflib.js and Comunica implement an emerging standard for RDF interaction called [Rdf/js](https://github.com/rdfjs), which has a community group at the W3C, and they “strive to design interface specifications with the goal that different JavaScript implementations of RDF concepts can interoperate”.  They have a specification for a data model.  Both Rdflib.js and Comunica support Rdf/js.

The [React application generator](https://github.com/inrupt/generator-solid-react) and React examples are built on [LDflex](https://github.com/RubenVerborgh/LDflex), which is built on Comunica.  Solid-panes, which is included as the default user interface for Solid pods via the Data Browser, is built on solid-ui, which is built on Rdflib.js.

# Pods

 https://github.com/solid/information/blob/master/pod-development.md
 
 https://github.com/solid/information/blob/master/self-hosting.md 

# Identity Providers 

