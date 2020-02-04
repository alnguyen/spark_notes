# Chapter 6: Choose Team-First Boundaries
____

- What are suitable boundaries within and across software systems that enable teams to own and evolve their part of the system effectively and sustainably in ways that encourage flow?

#### A Team-First Approach to Software Responsibilities and Boundaries

- Tightly coupled architectures negatively influence the capacity of having autonomous teams with clear responsibilities.
- Splitting a monolith in the wrong places can occur without accounting for the effect on teams.
- "Distributed monoliths" result in teams lacking autonomy over their services.

#### Hidden Monoliths and Coupling

#### Application Monolith

- A single, large application with many dependencies and responsibilities that possibly exposes many services and/or different user journeys.
- Can cause headaches
  - Application is down during deployment
  - Production env is a moving target

#### Joined-at-the-Database Monolith

- Composed of several applications or services
  - All coupled to the same database schema, making them difficult to change, test, and deploy separately.
- Results from organization viewing the DB, not the services, as the core business engine.

#### Monolithic Builds (Rebuild Everything)

- Uses one big continuous-integration build to get a new version of a component.

#### Monolithic (Coupled) Releases

- A set of smaller components bundled together into a "release".
- While builds for smaller components can be independent in CI, they must all be deployed together.

#### Monolithic Model (Single View of the World)

- Software that attempts to force a single domain language and representation (format) across many different contexts.
- Can inadvertently start imposing constraints on architecture and implementation

#### Monolithic Thinking (Standardization)

- "One size fits all" thinking
- Leads to unnecessary restrctions on technology and implementation approaches between teams.
- Simplifies management oversight but at a cost.
- Harms engineers ability to use the right tool for the job and reduces their motivation.
- Reduces learning and experimentation, leading to poorer solution choices.

#### Monolithic Workplace (Open-Plan Office)

- Single office-layout pattern for all teams and individuals in the same geographic location.
- Typically isolated individual work spaces (cubicles) or an open-plan layout without explicit barriers between people's desks.
- Can have a recurring negative effect on individuals and teams.

### Software Boundaries or "Fracture Planes"

- Splitting software can reduce the consistency between different parts of the software and lead to duplication across subsystems
- _Fracture Plane_ -- a natural seam in the software system that allows the system to be split easily into two or more parts.
- There are different types of fracture planes

#### Fracture Plane: Business Domain Bounded Context

- Most of our fracture planes should map to business-domain bounded  contexts.
- _Bounded context_ -- a unit for partitioning a larger domain model into smaller parts, each of which represents an internally consistent business domain area.
> DDD [domain-driven design] is about designing software based on models of the underlying domain. A model acts as a ubiquitous language to help communication between software developers and domain experts. It also acts as the conceptual foundation for the design of the software itself--how it's broken down into objects or functions. To be effective, a model needs to be unified--that is, to be internally consistent so that there are no contradictions within it.
- Identifying bounded contexts  requires a fair amount of business knowledge and technical experise.
- Other advantages of applying DDD
  - focusing on core complexity and opportunities within a bounded context for a given business domain
  - exploring models via collaboration between business experts
  - building software that expresses these models explicitly
  - having both business owners and technologists speaking in ubiquitous language within a bounded contex†

#### Fracture Plane: Regulatory Compliance

- In highly regulared industries, like finance or healthcare, regulatory requirements can provide hard borders for software.
- On one hand, good idea to minimouze the amount of variation in the auditing processes across different systems.
- On other hand, following strict requirements should not be forced on areas of the system that are not as critical.

#### Fracture Plane: Change Cadence

- Where different parts of the system need to change at different frequencies.
- Splitting off parts of the system that change at different speeds allows them to change more quickly or slowly, depending on the parts.
- Business needs now drive the speed of change, rather than the monolith imposing a fixed speed for all.

#### Fracture Plane: Team Location

- Working across different time zones aggravates communication delays and introduces bottlenecks when manual approvals or code reviews are needed from people in different time zones with little working-time overlap.
> If you must have remote workers, you will need to do extra work to foster the collaboration within the team and between the teams in order to build the community. You should try to have the same time zone versus different time zones; otherwise, people won't want to meet with each other because it cuts into their personal time at home.
- For a team to communicate efficiently, the options are between full colocation or a true remote-first approach.
- When neither options is feasible then it's better to split off the monolith into separate subsystems for teams in different locations.

#### Fracture Plane: Risk

- Different risk profiles might coexist in a large monolith.
- Having true continuous-delivery capabilities in place with a loosely coupled system architecture (not a monolith) decreases risk of deploying small changes very frequently.
- Multiple types of risks that can suggest fracture planes.
- Regulatory compliance is a specific type of risk.
- Other examples include
  - marketing-driven changes with a higher risk profile (focusing on customer acquisition)
  - marketing-driven changes with a lower risk profile changes to revenue-generating transactional features (focusing on customer retention)
  - Number of users might drive acceptable risk
    - Millions of free users vs hundreds of paying users

#### Fracture Plane: Performance Isolation

- Splitting off subsystems based on particular performance demands helps to ensure it can scale autonomously, increasing performance and reducing cost.

#### Fracture Plane: Technology

- Common kinds of technology-driven splits typically introduce more constraints and reduce flow of work rather than improve it.
- Certain situations where splitting off based on technology can be effective, particularly for systems integrating older or less automatable technology.

#### Fracture Plane: User Personas

- As a system grows, so does their customer base (internal or external)
- Tiered pricing systems, the subset is built in by design (higher paying customers have access to more features).
- Thus, it may make sense to split off subsystems for user personas in these types of situations.
- Sharper focus on customers' needs and experience using the system, which should result in higher customer satisfaction and improve the organization's bottom line.
- Can improve speed and quality of customer support
  - Becomes easier to map issues to a given subsystem and team.

#### Natural "Fracture Planes" for Your Specific Organization or Technologies

- Litmus test for applicability of a fracture plane: Does the resulting architecture support more autonomous teams with reduced cognitive load?
- Simple heuristic that can help assess system and team boundaries: Could we, as a team, effectively consume or provide the subsystem as a service?

### Summary: Choose Software Boundaries to Match Team Cognitive Load

- When optimizing for flow, stream-aligned teams should be responsible for a single domain.
- Need to look for natural ways to break down the system (fracture planes) that allow the resulting parts to evolve as independently as possible.
- Often a combination of fracture planes will be required.
> If you have microservices but you wait and do end-to-end testing of a combination of them before a release, what you have is a distributed monolith.
- Fracture plans can be chosen around specific challenges to help avoid hand-offs between teams and promote flow.
- In all cases, it is essential to make software segments team sized so that teams can effectively own and evolve their software in a sustainable way.

