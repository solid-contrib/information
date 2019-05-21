# About
[![](https://img.shields.io/badge/project-Solid-7C4DFF.svg?style=flat-square)](https://github.com/solid/solid)

![Solid Logo](https://avatars3.githubusercontent.com/u/14262490?v=3&s=200)

Solid (derived from "**so**cial **li**nked **d**ata") aims to decentralise power on the web to ensure that the web is used for the global public interest by providing the Solid specification based on
[Linked Data](http://www.w3.org/DesignIssues/LinkedData.html) principles. Read more about the [Solid values](https://github.com/solid/information/blob/master/solid-values.md).

## Solid Specification
The Solid Specification describes a rest api that extends those [existing standards](https://github.com/solid/information/blob/master/standards-used.md), contains design notes on the individual components used, and is intended as a guide for developers who plan to build Pods or Solid applications.The Solid specification is modular and extensible and relies as much as possible on existing
[W3C](http://www.w3.org/) standards and protocols.

The Solid specifications can be found in the following repositories: 
* [Solid](https://github.com/solid/solid-spec)  
* [Web Access Control](https://github.com/solid/web-access-control-spec) 
* [WebID OIDC](https://github.com/solid/webid-oidc-spec)

[Read how to make a suggestion to the Solid specification](https://github.com/solid/information/blob/master/decision-making-processes.md#changes-to-the-solid-specification). 

## Solid Test Suite 
[The Solid Test Suite is currently being developed](https://github.com/orgs/solid/projects/5). Once the Solid Test suite is ready you will be able to test if your Solid solution is adhering to the Solid specification with the Solid Test suite. 

## Solid Projects 
[Solid Projects](https://github.com/orgs/solid/projects) are implementations of the Solid specification within Solid Github account. Anyone can submit a project proposal to the Solid Team including a description of the project name, scope, manager, contributors and names of associated repositories needed. The Project Manager is responsible for determining the scope of a [project](https://github.com/orgs/solid/projects) as well as supporting the coordination of the Project Team Member. The Project Manager will ensure that any issues and pull requests related to their project are assigned to Project Team Member, ensure that those assigned are on the case, and are responsible for merging pull requests and closing issues associated to their project. A Project Manager has admin rights over the project they manage. A Project Team Member is working on a specific official Solid [project](https://github.com/orgs/solid/projects). Project Team Members may have issues assigned to them by the Project  Manager. Project Team Members have admin rights of projects they are working on. If you would like to join an existing project, reach out to corresponding Project Manager. If you would like to start a new project, reach out to the Solid Manager with your project proposal including a defined aim, Project Manager, and Project Team.

The implementation of the Solid specifications can be found in the following repos:
oidc-auth-manager, solid-multi-rp-client, folder-pane, pane-registry, oidc-rs, keychain, solid-pane, solid-notifications, solid-profile-ui, solid-connections-ui, pane-source, jose, solid-inbox, oidc-op, solid-tif, solid-client, oidc-rp, issue-panes, solid, solid-idp-list, kvplus-files, solid-email, oidc-web, solid-sign-up, solid, takeout-import, node-solid-ws, solid-auth-tls,  solid-auth-oidc, meeting-pane, solid-dips, solid-cli, solid-web-client, solid-permissions, acl-check, node-solid-server, solid-auth-client, wac-allow, mavo-solid, solid-auth-client, ldflex-playground, query-ldflex, react-components, profile-viewer-react, solid, solid-panes, solid-ui, mashlib

# Use 
Solid is not ready to be used, although there are prototypes which you can explore. In order to try out some of the Solid, you will first need to get a [WebID](https://github.com/solid/information/blob/master/pod-providers.md) and a [Pod](https://github.com/solid/information/blob/master/pod-providers.md). You can also [self host your Pod](https://github.com/solid/information/blob/master/self-hosting.md) rather than opt for a Pod provider. Once you have set your WebID and Pod you can use [Solid applications](https://github.com/solid/solid-apps). 

# Build
Anyone can implement the Solid specifications and is encouraged to [join the W3C SOlid Coommunity Group](https://www.w3.org/community/solid/) where the the development of the Solid specifications as well as coordinating those who are implementing the Solid is discussed. [Minutes of previous conversations and agendas of upcoming conversations of the W3C Solid community Group](https://www.w3.org/community/solid/wiki/Meetings) are publicly available. You can find a list of all organisations implementing the Solid specifications. 

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

# Team
The [Solid GitHub account](https://github.com/solid) was started by the [Solid academic project at MIT](https://solid.mit.edu) and today is run by the [Solid Team](https://github.com/solid/information/blob/master/solid-team.md) using [defined processes](https://github.com/solid/information/blob/master/decision-making-processes.md). 

# FAQs 
Read all past questions or ask a new question on the [Frequently Unanswered Questions](https://github.com/solid/information/blob/master/frequently-unanswered-questions.md) page. 

# Press 
You can find a list of [mentions of Solid in the press](https://github.com/solid/information/blob/master/solid-resources.md). The Solid Team and W3C Solid Community Group occasionally give talks or write articles. Find out about [upcoming Solid Team talks](https://github.com/solid/information/blob/master/solid-team-talks.md). Contact the Solid Team if you are interested in inviting them a a speaker at your event.   

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
