# Chapter 7: Team Interaction Modes
____

> Technologies and organizations should be redesigned to intermittently isolate people from each other's work for b est collective performance in solving complex problems.
- Arranging teams into patterns is not enough for high effectiveness.
- Need to change team interactions as well.
- Dynamics between teams and economics can change.
- Three core team interaction modes:
  - Collaboration
  - X-as-a-Service
  - Facilitating

#### Well-Defined Interactions Are Key To Effective Teams

- Poorly defined team interactions and responsibilities are a source of friction and ineffectiveness.
- What must be avoided is the need for all teams to communicate with all the other teams in order to achieve their ends.
- Collaboration--explicitly working together on defined areas
- X-as-a-Service--one team consumes something "as a service" from another team.
- Intermittent collaboration gives the best results.

#### The Three Essential Team Interaction Modes

- Collaboration: working closely together with another team
- X-as-a-Service: consuming or providing something with minimal collaboration
- Facilitating: helping (or being helped by) another team to clear impediments
- Combination of all three team interaction modes is likely needed for most medium-sized and large enterprises.
- Interaction modes should become team habits.

#### Collaboration: Driver of Innovation and Rapid Discovery but Boundary Blurring

- Suitable where a high degree of adaptability or discovery is needed.
- Good for rapid discovery of new things
  - Avoids costly hand-offs between teams.
- Requires good alignment and a high appetite and ability for working together between teams.
- Leads to new insights into how technologies work, with learning brought back into other teams.
- Two useful ways to visualize teams interacting in this mode
  - Two teams with distinct expertise and responsibilities working together on a small set of things.
  - Two teams essentially becoming one, pooling expertise and responsibilities
  - In both cases, the two teams take on joint responsibility for the overall outcomes of their collaborationg.
- Cognitive load of ongoing collaboration can be much higher than working purely inside the team's "natuarl" area.
- Communication overhead is going to be higher, possibly resulting in the apparent reduction of team effectiveness when viewed as a single team.
- Reward from this collaboration must be tangible.
- There should be little to no friction between teams.
- If clear, well-defined interfaces to services or systems between two teams is needed, then using the collaboration mode for extended periods is not the best choice.
- A need for ongoing collaboration suggests incorrect domain boundaries and/or team responsibilities, or the incorrect mix of skills within a team.
- Advantages of Collaboration Mode
  - Rapid innovation and discovery
  - Fewer Hand-offs
- Disadvantages
  - Wide, shared responsibility for each team
  - More detail/context needed between teams, leading to higher cognitive load
  - Possible reduced output during collaboration compared to before.
- Constraint
  - A team should use collaboration mode with, at most, one other team at a time. A team should not use collaboration with more than one team at the same time.
- Typical Uses
  - Stream-aligned teams working with complicated-subsystem teams
  - stream-aligned teams working with platform teams
  - Complicated-subsystem teams working with platform teams

#### X-as-a-Service: Clear Responsibilities with Predictable Delivery but Needs Good Product Management

- Where a component or aspect of the system can be effectively provided "as a service" by a distinct team or group of teams.
- Works best during later phases of systems development and periods where predictable delivery is needed.
- Results in the delivery team having to understand less about non-core aspects of their work.
- Great clarity about who owns what: one team consumers something that the other team provides.
- By design, innovation across the boundary happens more slowly than with collaboration.
- Responsibility boundary needs to make sense in context of business or technical domains.
- Team providing the service will be required to be adept at understanding the needs of the consuming teams.
- Two interacting teams have little need for day-to-day collaboration in order to use or provide the component/API/feature as a service.
- Works well only if the service boundary is well chosen and well implemented, with a good service-management practice from the team providing the service.
- Service should be straightforward to use, test, deploy, and/or debug
- Documentation should be clear, understandable, and up-to-date
- Must be managed in a way that keeps it viable over time.
- Advantages
  - Clarity of ownership with clear responsibility boundaries
  - Reduced detail/context needed between teams, so cognitive load is limited
- Disadvantages
  - Slower innovation of the boundary or API
  - Danger of reduced flow if the boundary or API is not effective
- Constraint
  - A team should expect to use the X-as-a-Service interaction with many other teams simultaneously, whether consuming or providing a service.
- Typical Uses
  - Stream-aligned teams and complicated-subsystem teams consuming Platform-as-a-Service from a platform team
  - Stream-aligned teams and complicated-subsystem teams consuming a component or library as a service from a complicated-subsystem team

#### Facilitating: Sense and Reduce Gaps in Capabilities

- Suited for when one or more teams would benefit from the active help of another team facilitating (or coaching) some aspect of their work.
- Main operating mode of an enabling team
- Can help to discover gaps or inconsistencies in existing components and services used by other teams.
- Does not take part in building the main software systems, supporting components, or platform but, instead, focuses on the quality of interactions between other teams building and running the software.
- Advantages
  - Unblocking of stream-aligned teams to increase flow
  - Detetion of gaps and misaligned capabilities or features in components and platforms
- Disadvantages
  - Requires experienced staff to not work on "building" or "running" things.
  - The interaction may be unfamiliar or strange to one or both teams involved in facilitation
- Constraint
  - A team should expect to use the facilitating interaction mode with a small number of other teams simultaneously, whether consuming or providing the facilitation.
- Typical Uses
  - An enabling team helping a stream-aligned, complicated-subsystem, or platform team
  - A stream-aligned, complicated-subsystem, or platform team helping a stream-aligned team

#### Team Behaviors for Each Interaction Mode

- A team should adopt different "styles" of team behavior depending on which other team or teams it is interacting with.
- Need to have a communication backchannel that avoids much of the politics, bandwidth constraints, and simple inefficiency of human-to-human communication.
- Humans work best with others when we can predict their behavior.
- We can build trust by providing consistent experiences for others in the organization.
- Promise theory--explains how and why it is preferable to construct inter-team relationships in terms of promises rather than in terms of commands and enforceable contracts.
  - e.g. Adhering to semantic versioning, the team promises not to break software that depends on their code.

#### Team Behaviors for Collaboration Mode: "High Interaction and Mutal Respect"

- Team members should expect activities to take much longer than they might expect as the "boundary spanning" aspects of collaboration discover and solve previously unknown problems.
- How to train for collaboration mode
  - Basic collaboration skills such as pair programming, mob programming, and whiteboarding can be valuable for teams interacting using collaboration mode.

#### Team Behaviors for X-as-a-Service Mode: "Emphasize the User Experience"

- Expect to emphasize the user experience
- Functionality is important, but in order to drive the best interaction between teams, a focus on the experience of using the platform is essential.
- How to train for X-as-a-Service mode
  - Training in core UX and DevEx practices can be valuable

#### Team Behaviors for Facilitating Mode: "Help and Be Helped"

- Expect to help and be helped.

### Choosing Suitable Team Interaction Modes

- Each team topology is likely to encounter different interaction modes.
  - Stream-aligned teams use X-as-a-Service or collaboration
  - Enabling teams use facilitation
  - Complicated-subsystem teams use X-as-a-Service
  - Platform teams use X-as-a-Service for teams that consume the platform
- These interactions gives hints for the kinds of interpersonal skills likely needed for each team type.
  - Platform teams will need strong product and service-management expertise
  - Enabling teams will need people with strong mentoring and facilitating experience.

### Choosing Basic Team Organization

#### Use the Reverse Conway Maneuver with Collaboration and Facilitating Interactions

- Should be used with temporary but explicit collaboration modes between the teams building the software with one or more enabling teams acting in a facilitating mode.

#### Discover Effective APIs between Teams by Deliberate Evolution of Team Topologies

- Dedicated architecture team is usually an anti-pattern
- Small group of software and systems architects can be effective when the remit of architecture is to discover, adjust, and reshape the interactions between teams, and therefore, the architecture of the system.
> Someone who claims to be an Architect needs both technical and social skills ... they need to have a say in business s trategies, organizational structures, and personnel issues, i.e., they need to be a manager too.

### Choose Team Interaction Modes to Reduce Uncertainty and Enhance Flow

#### Use the collaboration Mode to Discover Viable X-as-a-Service Interactions

- X-as-a-Service interaction mode can be effective in helping software-delivery teams achieve fast flow.
- Collaboration team interaction mode can be used to help redraw service boundaries in order to make the service more suitable for consuming teams.
- We want "just enough" collaboration at the service boundaries to adjust the scope of the service to meet the needs of consuming and providing teams.
- When trying to establish a new X-as-a-Service interaction, the same pattern applies:
  - Collaborate closely to establish viable "as a service" boundaries
  - Continue with light collaboration to validate effective boundaries
- Collaborate on potentially ambiguous interfaces until they are proven stable and functional

#### Change the Team Interaction Mode Temporarily to Help a Team Grow Experience and Empathy

- Changing interaction mode temporarily can help team members refresh and grow their experience, and increase empathy for the other team.
> When you deliberately plan out the [team changes] in your organization, you provide new learning opportunities for people.
- Vitally important that changes are deliberate, temporary, with full consent, understand, and enthusiasm of the people involved.

#### Use Awkwardness in Team Interactions to Sense Missing Capabilities and Misplaced Boundaries

- If a stream-aligned team is spending many hours trying to use a component, this is a signal that something is amiss.
  - Is the component boundary in the right place?
  - Is the component API well specified?
  - Is the component easy enough to use?
  - Does the complicated-subsystem team have a missing capability within the team, such as UX or DevEx?
- The absence of inter-team communication can be a sign that something is wrong in collaboration mode:
  - Do they understand the value of adopting the collaboration mode at this point?
  - Do they have enough skills to undertake this collaboration, or is another team better suited?
  - Is the boundary that the teams are trying to bridge too ambitious?

### Summary: Three Well-Defined Team Interaction Modes

- Simply defining a set of teams with resonsibility boundaries is not enough to produce an effective sociotechnical system; it is also necessary to define sensible and effective interactions between teams.
- Collaboration
  - Two teams work closely together for a defined period to discover new patterns, approaches, and limitations.
  - Responsibility is shared and boundaries blurred, but problems are solved rapidly and the organization learns quickly.
- X-as-a-Service
  - One team consumes something provided "as a service" from another team.
  - Responsibilities are clearly delineated and the consuming team can deliver rapidly.
  - The team providing the service seeks to make their service as easy to consume as possible.
- Facilitating
  - One team helps another team to learn or adopt new approaches for a defined period of time.
  - The team providing the facilitation aims to make the other team self-sufficient as soon as possible, while the team receiving the facilitation has an open-minded attitude to learning.
- 
