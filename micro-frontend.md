# Micro Frontend

- <https://newsletter.systemdesign.one/p/micro-frontends>

MicroFE slices the app into self-contained, domain-driven micro apps. The micro apps then get built, tested, and deployed independently. DOM gets shared between Micro Frontends.

Domain-driven design (DDD) is the key principle behind Micro Frontends. Put another way, each Micro Frontend represents a sub-domain.

Micro Frontends are usually set up with the backend for frontend (BFF) pattern. BFF pattern creates tailored backends for each Micro Frontend. But there is no need for a dedicated backend if the Micro Frontend consumes only a simple API.

## Why

- Fast delivery due to isolated deployments
- Scalable
- Low initial load time because Micro Frontends get loaded on demand
- modules are independent => if one module is crashed, others still work well

## Communication between micro frontends

Micro Frontends shouldn't share their state but communicate via messages or events. Besides communication between Micro Frontends should be kept minimal to prevent coupling. The ways Micro Frontends could communicate with each other are:

- Custom events
- Passing callbacks
- Routing by using the address bar as a communication mechanism
- Web workers

## How to implement

A separate server can be set up to render Micro Frontends. And a container app server can be installed to interact with relevant Micro Frontend servers on demand.

<img src="https://i.imgur.com/5KfsFmY.png">

There are 2 ways to organize Micro Frontend code:

- Monorepo: uses a single repository to share code and libraries. It’s easier to track and manage. But there is a risk of accidental coupling if boundaries are not set correctly.
- Polyrepo: one repository per Micro Frontend. It keeps the code base independent. But it might become difficult to track and manage.

## MicroFE shared components

It is important to avoid sharing business logic to prevent coupling.

Also a single team should own the shared component for high quality and consistency. But it should be kept open to contributions from every team.