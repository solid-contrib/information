# Introduction to the Solid Specification

Solid is a set of modular specifications, which build on, and extend the founding technology of the world wide web (HTTP, REST, HTML). They are 100% backwards compatible with the existing web. Each spec, taken in isolation, provides extra features to an existing system.  However, when used in combination, they enable exciting new possibilities for web sites and applications.

Read the [Solid specification](https://github.com/solid/specification). 

The main enhancement is that the web becomes a collaborative read-write space, passing control from owners of a server, to the users of that system. The Solid specification provides this functionality.

In order to write data as well as to read it, and to protect privacy, it is important to control who has access to what. This is managed using the web access control list specification.

For permissions it is essential to have a concept of identity, for which the WebID Identity spec is used.  This is a minimalistic identity spec, which allows a URI to denote a user, and return back machine readable data.

Authentication of that identity is provided using WebID-TLS and WebID-OIDC right now, but other strategies, such as key fobs, or two factor authentication, could be added to depending on system needs.

Discovery is the final piece, and allows the ability to tie all of these things together, and enables both humans and machines to participate in a rich ecosystem, leading to emergent and self organizing growth. The key tool for facilitating this is the URI specification. This forms the basis of the Linked Data philosophy which glues all of our data together, to create a complete Web Operating System.

## Standards Used in the Solid Specification

* [RDF 1.1 (Resource Description Framework)](http://www.w3.org/RDF/)
  (see also [RDF Primer](http://www.w3.org/TR/rdf11-concepts/)) is heavily
  used in Solid data models. By default, the *preferred* RDF serialization
  format is [Turtle](http://www.w3.org/TR/turtle/). Alternative serialization
  formats such as [JSON-LD](http://www.w3.org/TR/json-ld/) and
  [RDFa](http://www.w3.org/TR/rdfa-primer/) can also be used.

* The [WebID 1.0 (Web Identity and
  Discovery)](http://www.w3.org/2005/Incubator/webid/spec/identity/)
  standard is used to provide universal usernames/IDs for Solid apps, and to
  refer to unique Agents (people, organizations, devices). See also the
  [WebID interoperability notes](http://www.w3.org/2005/Incubator/webid/wiki/Identity_Interoperability)
  for an overview of how WebID relates to other authentication and identity
  protocols.

* WebIDs, when accessed, yield
  [WebID Profile](http://www.w3.org/2005/Incubator/webid/spec/identity/#dfn-webid_profile)
  documents (in Turtle and other RDF formats).

* The [FOAF vocabulary](http://xmlns.com/foaf/0.1/) is used both in WebID
  profiles, and in specifying Access Control lists (see below).

* Authentication (for logins, page personalization and more) is done via the
  [WebID-TLS protocol](http://www.w3.org/2005/Incubator/webid/spec/tls/).
  WebID-TLS extends WebID Profiles to include references to the subject's
  [public keys](https://en.wikipedia.org/wiki/Public-key_cryptography) in
  the form of [X.509 Certificates](https://en.wikipedia.org/wiki/X.509), using
  [Cert Ontology 1.0](http://www.w3.org/ns/auth/cert) vocabulary.
  The authentication sequence is done using the
  [HTTP over TLS](https://tools.ietf.org/html/rfc2818) protocol. Unlike normal
  HTTPS use cases, WebID-TLS is done without referring to
  [Certificate Authority](https://en.wikipedia.org/wiki/Certificate_authority)
  hierarchies, and instead encourages host server-signed (or self-signed)
  certificates.

* In Solid, certificate creation is typically done in the browser using the
  HTML5 [keygen
  element](http://www.w3.org/TR/html5/forms.html#the-keygen-element),
  to provide a one-step creation and certificate publication user experience.

* Authorization and access lists are done using
  [Basic Access Control ontology](http://www.w3.org/ns/auth/acl) (see also the
  [WebAccessControl wiki page](http://www.w3.org/wiki/WebAccessControl) for
  more details).

* Solid uses the [Linked Data Platform (LDP)](http://www.w3.org/TR/ldp/)
  standard (see also [LDP Primer](http://www.w3.org/TR/ldp-primer/))
  extensively, as a standard way of reading and writing generic Linked Data
  resources.
