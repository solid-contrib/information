This is a roadmap of development and specification activities arround the Solid Community. It shows items of work, that are important to the community ''right now'', or in ''short term''.

(The order in the table is arbitrary)

| What's going on? | Why is it important? | Contact Person | More Info |
| ------------- | ------------- | ------------- | ------------- |
| [Solid Project: Improving UI/UX/DX of solid-panes/mashlib](https://github.com/orgs/solid/projects/4) | Needed to attract end users and developers | Arne Hassel (megoth) |  https://forum.solidproject.org/t/improving-ui-ux-dx-of-solid-panes-mashlib-aka-solid-data-browser/1541|
| [Solid Project: Test Suite](https://github.com/orgs/solid/projects/5) | automate tests of Solid servers | [Kjetil Kjernsmo](https://github.com/kjetilk) | ------------- |
| [Solid Project: Bugfixes on Node Solid Server](https://github.com/orgs/solid/projects/2) | Bugs annoy everybody and scare away people that give solid a try | Jackson Morgan| https://github.com/solid/node-solid-server/issues |
| [npm modules for Solid Servers](https://github.com/orgs/inrupt/projects/1)| Support development of other node-based Solid servers | Michiel de Jong | https://github.com/orgs/inrupt/projects/1 |
| Developing Solid apps | Have examples to show as well as  useful apps that attract end-users| https://gitter.im/solid/app-development / Maintainer of the specific app| https://github.com/solid/solid-apps |
| Developing Solid libraries
| Make it easier for  developers to create Solid apps
| Jeff Zucker|------------- |
| Solid Chat Challenge | Make a functioning chat app for the Solid community to use | Jose Emilio Labra | https://gitter.im/solid/chat-app |
| [Open Network](https://open-network.dev/) | Improve usablility & broader community with multiple Solid implementations. Aim is to improve accessibility for general consumer. | [Fabian Cook](https://open-network.dev/#contact) | [Open Network Development webite](https://open-network.dev/) |

Ideas for Roadmap: 
* Babies of 2020: the first piece of data a person generates is the ultrasound from their pregant mother. This is also a piece of data that is commonly shared socially as an announcement that a baby will be born. Babies of 2020 is a concept of providing a Solid compatible for the ultrasound of a baby along with the birth certificate. As the baby grows older more Solid compatible solutions could be provided for key life events such as joining a school, finding a job, opening a bank account etc. 
* the existence of good, usable apps; for that we need:
  * app developers; for that we need:
    * an unambiguous 0.8 spec with at least two working implementations
    * data conventions
    * tools and docs for app developers
    * a well-governed process to guarantee Solid stays open for permissionless innovation
    * users willing to use and test new apps
* usable access control; for that we need:
  * data scopes; for that we need:
    * a working group that adds those to the 0.9 spec
    * implementation in both server implementations
* efficient access to the web of data; for that we need:
  * apps that use background sync
  * research into indexing and caching strategies
* ability to move your pod from one pod provider to another without too much hassle; for that we need:
  * pod providers that allow using your own domain name
  * instructions on how to host your webid on a static server, outside your pod
  * app that can handle contacts with multiple frequently changing WebID's
