# Chapter 4: Static Team Topologies
----

- Today, the prevailing way to set up or reorganize teams is ad hoc, focused on immediate needs rather than the ability to adapt in the long run.
- Static team topologies--team structures and interactions t hat fit a specific organization's context at a given point in time.

#### Team Anti-Patterns

- Two particular anti-patterns:
  - Ad hoc team design.
    - When teams have grown too large and broken up
    - Teams are broken up due as:
      - Communication overhead starts taking a toll
      - Teams created to take care of all COTS software or all middleware
      - Or a DBA team created after a software crash in production caused by poor DB handling.
  - Shuffling team members
    - Leads to extremely volatile teams assembled on a project basis and disassembled immediately afterward
    - Cost of forming new teams and switching contexts repeatedly gets overlooked.
- Must design teams intentionally.
- Ask these questions:
  - Given our skills, constraints, cultural and engineering maturity, desired software architecture, and business goals, which team topology will help us deliver results faster and safer?
  - How can we reduce or avoid handovers between teams in the main flow of change?
  - Where should the  boundaries be in the software system in order to preserve system viability and encourage rapid flow?
  - How can our teams align to that?

#### Design for Flow of Change

- "Low friction" software delivery--organization designs that emphasize the flow of change from concept to working software.
> An organization has a better chance of success if it is reflectively designed.
- Need to take into account more than a static placement of people when looking at the design of team interactions.

#### Shape Team Intercommunication to Enable Flow and Sensing

- Many organizations assume that software delivery is a one-way process.
  - Dev -> Test -> Transition -> Business as usual (live)
  - Completely incompatible with the speed of change and complexity of modern software systems.
- Teams that are exposed to the live environment tend to address user-visible and operational problems much more rapidly.
- Ensure  delivery teams are cross-functional.
  - Each team need to have the skills necessary to design, develop, test, deploy, and operate the system on the same team.

#### DevOps and the DevOps Topologies

- "Wall of confusion"--when work gets "thrown over the wall"
- DevOps emerged as a result of pressure on dev and ops to deliver efficiently as agile was a growing methodology.
- Problems that led to the birth of DevOps
  - How teams interacted (or not) across the delivery chain causing delays, rework, failures, and a lack of understanding and empathy toward other teams.

#### DevOps Topologies

- Successful patterns are strongly dependent on contextual aspects, like organization and product size, engineering maturity, and shared goals.
- Teams should evolve and morph over time.
- Reflects two key ideas:
  1. There is no one-size-fits-all approach to structuring teams for DevOps success.
  2. There are several topologies known to be anti-patterns. These are bad.

#### Successful Team Patterns

- An inadequate choice of topology doesn't necessarily mean that the desired outcomes aren't good.
- Outcomes are generally not yielded because there's a focus on the new team structure but not enough consideration about the surrounding teams and structures.
- The surrounding environment, teams, and interactions are dependencies on the success of different types of teams.

#### Feature Teams Require High-Engineering Maturity and Trust

- Feature team:
  - Cross functional
  - Cross component
  - Takes a customer facing feature from idea to production
  - Monitors usage and performance
- Cross-functional team
  - Can bring high value to an organization by delivering cross-component, customer-centric features faster than distinct component teams that synchronize into a single release
  - Can only do this if self-sufficient--they don't have to wait for other teams
- If the feature team isn't mature, they may take shortcuts.
  - Leads to a breakdown of trust between teams
  - Increases tech debt
  - Slows down delivery speed overall
- A lack of ownership over shared code may result from the cumulative effects of several teams working on the same codebase unless inter-team discipline is high.
- Roles such as system architects, system owners, or integration leads work across the entire project/organization to support on cross-subsystem concerns.

#### Product Teams Need a Support System

- Product team--identical in purpose and characteristics to a feature team but owning the entire set of features for one or more products
  - Still depends on infrastructure, platform, test environments, build systems, and delivery pipelines (and more)
  - May have full control over some of these dependencies
  - Will need help with others due to the natural cognitive and expertise limits of a team.
- Key for the team to remain autonomous is for external dependencies to be non-blocking.
- Pre-defined schedules can not succeed in coordinating multiple teams on the same stream of work.
  - Teams have different workloads, priorities, and problems
- Non-blocking dependencies
  - Takes form of self service capabilities (e.g. provisioning test environments, creating deployment pipelines, monitoring, etc.) developed and maintained by other teams.
- Creating product teams without a compatible support system creates more bottlenecks.
  - Product teams end up waiting on "hard dependencies" to functional teams (infrastructure, networking, QA).
- As product teams are pressured to deliver faster, the system does not support the necessary levels of autonomy.

#### Cloud Teams Don't Create Application Infrastructure

- Product teams need autonomy to provision their own environments and resources in the cloud
- A cloud team may own the provisioning process but their focus should be in providing high-quality self-services that match both the needs of product teams and the need for adequate risk and compliance management.
- Designing the cloud infrastructure process is responsibility of the cloud team
- The actual provisioning and updates to the application resources is responsibility of the product teams

#### SRE Makes Sense at Scale

- SRE teams are not essential; they are optional
- Building and running software systems is a sociotechnical activity
- SRE model
  - Sets up a healthy and productive interaction between the development and SRE teams by using service-level objectives (SLOs) and error budgets to balance the speed of new features with whatever work is needed to make the software reliable.
  - SRE team can advise or take over operations depending on the SLO and error budgets
  - SRE role can change multiple times over time

### Considerations When Choosing a Topology

- Organization context influences successful setup of certain types of teams.

#### Technical and Cultural Maturity

- Agile--smaller batches of delivery
- DevOps can help an organization adopting agile
  - Bring in battle-tested engineers for their expertise and sync teams by collaborating on shared practices and tools.
  - Needs to have a clear mission and expiration date, otherwise DevOps can end up being another silo

#### Organization Size, Software Scale, and Engineering Maturity

- Organization size (or software scale) and engineering discipline influence the effectiveness of team interaction patterns.
- Low maturity organizations need time to acquire engineering and product development capabilities required for autonomous end-to-end teams.
- Specialized teams (development, operations, security, etc) are an acceptable trade-off only if they collaborate closely to minimize wait times and quickly address issues.
- As size of the organization or software scale increases, focus on providing the underlying infrastructure or platform as a service.
- For high levels of maturity, the SRE model may be effective at scale.

#### Splitting Responsibilities to Break Down Silos

- Sometimes we can remove or lessen dependencies on specific teams by breaking down their set of responsibilities and empowering other teams to take some of them on.
  - e.g. Separate the activities of database development (DB Dev) from database administration (DBA).

#### Dependencies and Wait Tiems between Teams

- Essential to track dependencies and wait times between teams.
> Visualizing important cross-team information helps communicate across teams
- Number of dependencies should not be allowed to increase unchecked.
- Increases should trigger adjustments in the team design and dependencies.

#### Using DevOps Topologies to Evolve the Organization

- Organizations, teams, and strategies change over time.

### Summary: Adopt and Evolve Team Topologies that Match Your Current Context

- Forward-thinking organizations take a multi-stage approach to their team design.
- What works best today might not be the case in the future.