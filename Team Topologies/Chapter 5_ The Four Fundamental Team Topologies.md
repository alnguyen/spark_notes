# Chapter 5: The Four Fundamental Team Topologies
____

- Four fundamental team topologies:
  - Stream-aligned team
  - Enabling team
  - Complicated-subsystem team
  - Platform team
- These four team topologies should act as "magnets" for all team types.
- We should prefer these types and aim to adopt the purpose, role, responsibility, and interaction behavior of these fundamental types for every team in the organization.
- There is no "ops" or "support" team in these fundamental topologies--this is deliberate.

### Stream-Aligned Teams

- Stream--continuous flow of work aligned to a business domain or organizational capability.
- A stream-aligned team is a team aligned to a single, valuable stream of work
  - A single product or service
  - A single set of features
  - A single user journey
  - A single user persona
- The team is empowered to build and eliver as quickly, safely, and indepedently as possible.
- Primary team type
- Other fundamental team topologies purposes are to reduce the burden on the stream-aligned team.
  - e.g. Enabling team helps by acquiiring missing capabilities
  - e.g. Platform team reduce cognitive load by off-loading lower level detailed knowledge and providing easy-to-consume services around the stream-aligned team.
- Closer to the customer by necessity.
- Can react to system problems in near real-time
- Different streams can coexist in an organization
  - specific customer streams
  - business-area streams
  - geography streams
  - product streams
  - user-persona streams
  - compliance streams
- Team is funded in a long-term, sustainable manner as part of a portfolio or program of work.
- Flow of work is clear

#### Capabilities within a Stream-Aligned Team

- Each team requires a set of capabilities to progress work from initial stages to production
  - Application security
  - Commercial and operational viability analysis
  - Design and architecture
  - Development and coding
  - Infrastructure and operability
  - Metrics and monitoring
  - Product management and ownership
  - Testing and quality assurance
  - User experience
- May mean having a mix of generalists and specialists

#### Why Stream-Aligned Team, Not "Product" or "Feature" Team?

- "Continuous design" is imporant
  - Treats ideas such as service, feedback, failure, and learning as first-class concepts
- Not all situations need products or features

#### Expected Behaviors

- Mission is to ensure smooth flow of work for a given stream
- Goals
  - aims to produce a steady flow of feature delivery
  - quick to course correct based on feedback from the latest changes
  - uses an experimental approach to product evolution, expecting to constantly learn and adapt
  - has minimal hand-offs of work to other teams
  - is evaluated on the sustainable flow of change it produces
  - must have time and space to address code quality changes (tech debt) to ensure that changing the code remains safe and easy to do
  - proactively and regularly reaches out to the supporting fundamental-topologies teams
  - members feel they have achieved or are in the path to achieving "autonomy, mastery, and purpose"

### Enabling Teams

- Composed of specialists in a given technical domain.
- Cross-cut to the stream-aligned teams and have the required bandwidth to research, try out options, and make informed suggestions on adequate tooling, practices, frameworks, and any of the ecosystem choices around the application stack.
- Strong collaborative nature
- Actively avoids becoming "ivory towers" of knowledge, dictating technical choices, while helping teams to understand and comply with org-wide technology constraints.
- End goal is to increase autonomy of stream-aligned teams by growing their capabilities with a focus on their problems first, not the solutions per se.
- There should not be a permanent dependency on an enabling team.

#### Expected Behaviors

- Mission is to help stream-aligned teams acquire missing capabilities
- Outcomes we expect to see:
  - Proactively seeks to understand the needs of stream-aligned teams, establishing regular checkpoints and jointly agreeing when more collaboration is needed.
  - Stays ahead of the curve in keeping abreast of new approaches, tooling, and practices in their area of expertise, well before an actual need is expected from stream aligned teams.
  - Acts as a messenger of both good news and bad news to help with management of the technology life cycle.
  - Occasionally act as a proxy for external (or internal) services that are currently too difficult for stream-aligned teams to use directly.
  - Promotes learning inside the team but across stream-aligned teams, acting as a curator that facilitates appropriate knowledge sharing inside the organization.
- Primary purpose is to help stream-aligned teams deliver working software in a sustainable, responsible way.
- Do not exist to fix problems spawned from poor practices, prioritization choices, or poor code quality.

**Enabling Team versus Communities of Practice (CoP)**

> CoP create the right environment for social learning, experiential learning, and a rounded curriculum, leading to accelerated learning for members.

- ET and CoP have slightly different purposes and dynamics
- ET
  - small
  - long-lived
  - specialists
  - focused on building awareness and capability for a single team
- CoP
  - Seeks to have more widespread effects
  - diffusing knowledge across many teams

### Complicated-Subsystem Teams

- Respomsible for building and maintaining a part of the system that depends heavily on specialist knowledge
- Goal is to reduce cognitive load of stream-aligned teams working on systems that include or use the complicated subsystem
- Difference between this and a component team is that the complicated-subsystem team is created only when a subsystem needs mostly specialized knowledge.
  - Driven by cognitive load, not perceived opportunity to share the component.

#### Expected Behaviors

- Missions is to off-load work from stream-aligned teams on particularly complicated subsystems that need to be developed by a group of specialists.
- Outcomes to be expected
  - Mindful of the current stage of development of the subsystem and acts accordingly
  - High collaboration with stream-aligned teams during early exploration and development phases
  - Reduced interaction and focus on the subsystem interface and feature evolution and usage during later stages, when subsystem has stabilized
  - Delivery speed and quality for the subsystem is clearly higher than if/when the subsystem was being developed by stream-aligned team
  - Correctly prioritizes and delivers upcoming work respecting the needs of the stream-aligned teams that use the complicated subsystem.

### Platform Teams

- Purpose is to enable stream-aligned teams to deliver work with substantial autonomy.
- Provides internal services to reduce the cognitive load that would be required from stream-aligned teams to develop these underlying services.
- Team's knowledge is best made available via self-service capabilities via a web portal and/or programmable API that stream-aligned teams can easily consume.
- Focus on providing a smaller number of servives of acceptable quality rather than a large number of services with many resilience and quality problems.

#### Expected Behaviors

- Mission is to provide the underlying internal services required by stream-aligned teams to deliver higher level services or functionalities, thus reducing their cognitive load.
- Expected outcomes
  - Uses strong collaboration with stream-aligned teams to understand their needs
  - Relies on fast prototyping techniques and involves stream-aligned team members for fast feedback on what works and what does not
  - Has a strong focus on usability and reliability for their services
  - Regularly assesses if the services are still fit for purpose and usable
  - Leads by example
    - uses the services the provide internally
    - partnering with stream-aligned teams and enabling teams, consuming lower level platforms whenever possible
  - Understands that adoption of internal new services, like new technologies, is not immediate, but instead evolves along an adoption curve

### Compose the Platform from Groups of Other Fundamental Teams

- Large organizations will sometimes need more than one team to build and run a platform; sometimes there may be multiple platforms
- In this situation, a paltform is composed of groups of other fundamental team types: stream aligned, enabling, complicated subsystem and platform.
- Streams for platform teams are different from the streams for teams building the main products and services.
- For a platform, the streams relate to services and products within the platform.
  - Like logging and monitoring services, APIs for creating test environments, facilities for querying resource usage, and so on.
- Inner topologies--nested teams within the platform
- From the Dev teams perspective, the platform is a single entity that provides them with a service that they simply consume via API:
  - machine or container provisioning
  - network config
  - etc
- Inside the platform team, there can be several distinct teams that collaborate with or provide a service to other platform teams.
- Similar to the "layered Ops" approach
  - One team provides the base infrastructure (physical and virtual)
  - Second team focuses on running supporting services on top of the base infrastructure

### Avoid Team Silos in the Flow of Change

- Traditionally, organizations created silos of functional expertise by grouping the staff:
  - QA
  - DBA
  - UX
  - Architecture
  - Data processing (ETL)
- Organizations that optimize for safe and rapid flow of  change tend to use mixed-discipline or cross-functional teams
- Work is never handed off to another team for a later stage in the flow.

### A Good Platform is "Just Big Enough"

- A well-designed and well-run platform can be a significant force multiplier for software delivery.
- A good platform:
  - Provides standards, templates, APIs, and well-proven best practices for Dev teams to use to innovate rapidaly and effectively

#### The Thinnest Viable Platform

- Simplest platform is a list on a wiki page of underlying components or services used by consuming software.
- Avoid letting platform dominate the discourse.

#### Cognitive Load Reduction and Accelerated Product Development

- Successful platforms drive to simplify the developer's life and reduce cognitive load
- A good platform helps Dev teams focus on the germane (differentiating) aspects of a problem, increasing personal and team-level flow, and enabling the whole team to be more effective.

#### Compelling, Consistent, Well-Chosen Constraints

- Ensure the platform teams have a focus on UX and particularly developer experience/

#### Built On an Underlying Platform

- Each platform is built on another platform.

#### Manage as a Live Product or Service

- Treat the platform as a live/production system, with any downtime planned and managed
- Use software-product-management and service-management techniques
- The platform team's main clientele is the product teams
- The platform needs a roadmap
- Evolution of the platform "product" curated and shaped to meet Dev teams' needs in the longer  term.

#### Convert Common Team Types to the Fundamental Team Topologies

#### Move to Mostly Stream-Aligned Teams for Longevity and Flexibility

#### Infrastructure Teams to Platform Teams

#### Component Teams to Platform or Other Team Types

#### Tooling Teams to Enabling Teams or Part of the Platform

#### Converting Support Teams

#### Converting Architecture and Architects

- Most effective pattern for an architecture team is as a part-time enabling team.
- Many decisions should be taken by implementing teams rather than left to the architecture team.
- Rather than imposing designs or technology choices, architecture should support other teams.

### Summary: Use Loosely Coupled, Modular Groups of Four Specific Team Types

- Restrict teams to just four fundamental types
  - Stream aligned
  - Enabling
  - Complicated subsystem
  - Platform
- Majority of teams need to be loosely coupled, aligned to the flow of change, and capable of delivering a useful increment in the product
- 