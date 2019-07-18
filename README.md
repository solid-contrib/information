[![](https://img.shields.io/badge/project-Solid-7C4DFF.svg?style=flat-square)](https://github.com/solid/solid)

![Solid Logo](https://avatars3.githubusercontent.com/u/14262490?v=3&s=200)

Solid is a global technical standard for building interoperable software. It aims to rest control of where user data goes, how it is used and by whom to the users themselves. In being Solid-compliant, applications and services allow users to securely move to other services or application at will and take their data with them. It also gives them autonomy over how that data is accessed by others.

Solid is led by Sir Tim Berners-Lee, inventor of the World Wide Web, and originated at the Massachusetts Institute of Technology (MIT). It is a non-commerical, open source collaboration between thousands of contributors from across the globe.
Solid works on three levels:

1. Login: users identify themselves using a WebID which is a domain name based alternative to a password. Ideally the user owns the domain used to identify themselves. This avoids the need for user login credentials being held by 3rd parties.
Pods: users store their unique data on Pods (personal online data stores) rather than centralised data stores e.g. Google, Facebook. Users can have multiple Pods. Data generated while using Solid-compliant applications or services is stored on the 

2. Pod independently of the application or service. This means that data can be moved freely to other services or apps without constraint.

3. Apps: Solid-compliant apps and services have granular access to user data meaning they only get a specific type of data, for a specific purpose and for a set timeframe. This means users have much greater control over how their data is accessed.

Read more about the Solid [mission, vision, and values](https://github.com/solid/information/blob/master/solid-vision-mission-values.md). 

# Developing Solid

## Solid Specification
The Solid Specification describes a rest api that extends those [existing standards](https://github.com/solid/information/blob/master/standards-used.md), contains design notes on the individual components used, and is intended as a guide for developers who plan to build Pods or Solid applications.The Solid specification is modular and extensible and relies as much as possible on existing
[W3C](http://www.w3.org/) standards and protocols.

The Solid specifications can be found in the following repositories: 
* [Solid](https://github.com/solid/solid-spec)  
* [Web Access Control](https://github.com/solid/web-access-control-spec) 
* [WebID OIDC](https://github.com/solid/webid-oidc-spec)

[Read how to make a suggestion to the Solid specification](https://github.com/solid/information/blob/master/decision-making-processes.md#changes-to-the-solid-specification). 

## Weekly Updates 

# Connect

## Online Chat
The  Gitter chats associated to the [Solid GitHub account](https://github.com/solid) include 

* [solid/chat](https://gitter.im/solid/chat): a place to talk about all things Solid
* [solid/solid-spec](https://gitter.im/solid/solid-spec): a place to talk about the Solid specification. Note that actual changes to the Solid specification happen through the official processes. 

Groups implementing the Solid  have a dedicated channel based on the implemtatnion. 
* [solid/app-development](https://gitter.im/solid/app-development): Solid app developers can share tips and ask questions on this channel 
* [solid/pods](https://gitter.im/solid/pods): Solid Pod providers can share tips and ask questions on this channel 
* [solid/events](https://gitter.im/solid/solid-events): Solid Event organisers can share tips and ask questions on this channel 

Solid Projects have dedicated chat channels. 
* [site-and-docs](https://gitter.im/solid/site-and-docs): a place to talk about the Solid Project '[Solid Content Generation](https://github.com/orgs/solid/projects/6)'
* [solid/test-suite](https://gitter.im/solid/test-suite): a place to talk about the Solid Project '[Test Suite](https://github.com/orgs/solid/projects/5)'
* [solid/data-browser](https://gitter.im/solid/data-browser): a place to talk about the Solid Project '[Solid data browser UI/UX/DX](https://github.com/orgs/solid/projects/4)'
* [solid/node-solid-server](https://gitter.im/solid/node-solid-server): originally a place to talk about [Solid Project NSS-v5.0.0](https://github.com/orgs/solid/projects/1) and now a dedicated space for [Solid Project ASAP on Server](https://github.com/orgs/solid/projects/2)

Solid Challenges have dedicated channels associated to them
* [solid/chat-app](https://gitter.im/solid/chat-app): a place to talk about the [Solid Chat Challenge](https://github.com/solid/information/blob/master/solid-challenges.md#solid-chat-challenge)
* solid/blog-app: coming soon as a place to talk about the [Solid Blog Challenge](https://github.com/solid/information/blob/master/solid-challenges.md#solid-blog-challenge)

## Weekly W3C Solid Community Group Calls 

## Solid Events 
[Solid Events](https://github.com/solid/information/blob/master/solid-events.md) provide an opportunity for anyone to meet and talk about Solid in person. Anyone can organise a Solid Event. If you are thinking about running a Solid Event in your city below is some guidance from previous Solid Event Organisers. If you have run a Solid Event, please contribute to the guidance information and share your learnings with the Solid Manager to incorporate. If you are organising a Solid Event, let others know about it by publishing it. 

# Implementing Solid
Anyone can implement the Solid specifications and is encouraged to [join the W3C SOlid Coommunity Group](https://www.w3.org/community/solid/) where the the development of the Solid specifications as well as coordinating those who are implementing the Solid is discussed. [Minutes of previous conversations and agendas of upcoming conversations of the W3C Solid community Group](https://www.w3.org/community/solid/wiki/Meetings) are publicly available. You can find a [list of all organisations implementing the Solid specifications](https://github.com/solid/information/blob/master/solid-roadmap.md). 

To build on Solid a basic understanding of Linked Data is helpful. You can  find a list of [linked data learning resources](https://github.com/solid/information/blob/master/learning-linked-data.md).

### Solid Challenges 
Solid Challenges are set by a lead who can provide guidance to those who participate in a specific goal. Previous Solid Challenges include: 
* [Solid Chat](https://gitter.im/solid/chat-app) by [Jose Emilio Labra Gayo](https://github.com/labra)

### Solid Resources 
[Read more about resources](https://github.com/solid/information/blob/master/solid-resources.md) that could be suited to building on Solid. If you know of any grants or channels to apply to resources that would allow developers to focus on building their application on solid, please do share them by [adding them to the list](https://github.com/solid/information/blob/master/solid-resources.md). 

## Solid Logo and Terms 
You can read a complete set of standardised definitions in the [Solid dictionary](https://github.com/solid/information/blob/master/solid-dictionary.md) and the [Solid Logo usage guidelines](https://github.com/solid/information/blob/master/solid-logo-usage-guidelines.md). 

## License 
Each repository has a license of which you can get an [overview](https://github.com/solid/information/blob/master/license.md). 

## Identity Providers
Identity providers authenticate users with a WebID. The identity provider is the server hosting the profile documents to which the WebIDs refer used to authenticate for WebID-OIDC. Some identity providers are also Pod providers. Read a list of [identity providers](https://github.com/solid/solid-idp-list) as well as instructions on how to [become a Solid identity provider](https://github.com/solid/information/blob/master/webid-provider.md). 

 ## Pod Providers
Pod providers offer storage to the user. Some Pod providers are also identity providers. Read a list of [Pod Providers](https://github.com/solid/pods/tree/master) as well as instructions on [how to become a Pod Provider](https://github.com/solid/information/blob/master/pod-development.md) or [self host a Pod](https://github.com/solid/information/blob/master/self-hosting.md). 
 
 ## Solid Apps
Read a list of [Solid Apps](https://github.com/solid/solid-apps) as well as [instructions on how to build a Solid app](https://github.com/solid/information/blob/master/app-development.md). Solid applications are somewhat like multi-user applications where instances talk to each other through a shared filesystem, and the Web is that filesystem.


# Using Solid

There are multiple Solid prototypes that you can use and and explore already. In order to try out some of the Solid, you will first need to get a [WebID](https://github.com/solid/information/blob/master/pod-providers.md) and a [Pod](https://github.com/solid/information/blob/master/pod-providers.md). You can also [self host your Pod](https://github.com/solid/information/blob/master/self-hosting.md) rather than opt for a Pod provider. Once you have set your WebID and Pod you can use [Solid applications](https://github.com/solid/solid-apps). 

# FAQs 
Read all past questions or ask a new question on the [Frequently Unanswered Questions](https://github.com/solid/information/blob/master/frequently-unanswered-questions.md) page. 

# Press 
You can find a list of [mentions of Solid in the press](https://github.com/solid/information/blob/master/solid-press.md). The Solid Team and W3C Solid Community Group occasionally give talks or write articles. Find out about [upcoming Solid Team talks](https://github.com/solid/information/blob/master/solid-team-talks.md). Contact the Solid Team if you are interested in inviting them a a speaker at your event.   
