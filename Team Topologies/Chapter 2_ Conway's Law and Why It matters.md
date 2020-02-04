# Chapter 2: Conway's Law and Why It matters
----

#### Understanding and Using Conway's Law

- Novelties can help teams improve locally
  - Microservices
  - Cloud
  - Containers
  - Serverless

> ... a product's architecture tends to mirror the structure of the organization in which it is developed.

> If the architecture of the system and the architecture of the organization are at odds, the architecture of the organization wins.

- An organization that is arranged in functional silos is unlikely to produce software systems that are well architected for end-to-end flow.
- An organization that is arranged primarily around sales channels for different geographic regions unlikely to produce effective software architecture that provides multiople different software services to all global regions.

> Given any particular team organization, there is a class of design alternatives which cannot be effectively pursued by such an organization because the necessary communication paths do not exist

#### The Reverse Conway Maneuver

- AKA inverse Conway maneuver
- A way to increase an organization's chances of building effective software systems optimized for flow.
> Organizations should evolve their team and organizational structure to achieve the desired architecture. The goal is for your architecture to support the ability of teams to get their work done--from design through to deployment--without requiring high-bandwidth communication between teams.
- Accruing technical debt like this curtails an organization's ability to move faster and make a difference against competitors.

#### Software Architectures that Encourage Team-Scope

- Conway's Law tells us that we need to understand what software architure is _before_ we organize our teams
> Team assignments are the first draft of the architecture.
- Software-architecture good practices:
  1. Loose coupling--components do not hold strong dependencies on other components
  2. High cohesion--components have clearly bounded responsibilities, and their internal elements are strongly related
  3. Clear and appropriate version compatibility
  4. Clear and appropriate cross-team testing

- At a conceptual level, software architectures should resemble the flows of change they enable
- Instead of a series of interconnected components, we should be designing flows on top of an underlying platform
- Architecture for participation
  - Promotes ease of understanding by limiting module size
  - Promotes easy of contribution by minimizing the propagation of design changes

#### Organization Design Requires Technical Expertise

- Anyone who makes decisions about the shape and placement of engineering teams is strongly influencing the software systems architecture
> If we have managers deciding ... which services will be built, by which teams, we implicitly have managers deciding on the system architecture.
> Someone who claims to be an Architect needs both technical and social skills, they need to understand people and work within the social framework. They also need a remit that is broader than pure technology--they need to have a say in organizational structures and personnel issues, ie. they need to be a manager too.
> Departments and divisions, systems, and business processes ... can be designed independently as long as interfaces and boundaries with the wider organization form part of the design.

#### Restrict Unnecessary Communication

- Not all communication and collaboration is good
- More communication is not always better
- What's needed is _focused_ communication between specific teams
> Managers should focus their efforts on understanding the causes of unaddressed design interfaces ... and unpredicted team interactions ... across modular systems.
- Communication between most teams should be low bandwidth
- If a large team regularly deals with two separate areas of the system, it can be useful to split this team into two smaller teams dedicated to each part

#### Everyone Does Not Need to Communicate with Everyone

- Ability to communicate with anyone can derive unintended communication paths, thus unintended consequences for the software systems.
- Many-to-many communication will tend to produce monolithic, tangled, highly coupled, interdependent systems that do not support fast flow.

#### Beware: Naive Uses of Conway's Law

**Tool Choices Drive Communication Patterns**

- Restricted access to the tool is likely to drive a communication gap between teams with access and teams without.
- Don't elect a single tool for the whole organization without considering team inter-relationships first.

**Many Different Component Teams**

- AKA complicated subsystem teams
- Occasionally needed but only for exceptional cases, where very detailed expertise is required

**Repeated Reoganizations that Create Fiefdoms or Reduce Headcount**

- Regular reoganizations for the sake of management convenience or reducing headcount actively destroy the ability of organizations to build and operate software systems effectively.
- Reorganizations that ignore Conway's law, team cognitive load, and related dynamics risk acting like open heart surgery performed by a child: highly destructive

#### Summary: Conway's Law is Critical for Efficient Team Design in Tech

- Fast flow requires restricting communication between teams.
- Collaboration is important for gray areas of development.
- Discovery and expertise is needed to make progress.