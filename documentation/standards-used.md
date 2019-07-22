The Solid specification uses the following standards.

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
