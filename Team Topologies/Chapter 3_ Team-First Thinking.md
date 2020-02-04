# Chapter 3: Team-First Thinking
----

- Teams working as a choesive unit perform far better than collections of individuals for knowledge-rich, problem-solving tasks that require high amounts of information.
> Teams accomplish remarkable feats not simply because of the individual qualifications of their members but because _those members coalesce into a single organism_

- Research by Google found that who is on the team matters less than the team dynamics.
- When it comes to measuring performance, teams matter more than individuals.

#### Use Small, Long-Lived Teams as the Standard

- Team: a stable grouping of five to nine people who work toward a shared goal as a unit.
- The team is the smallest entity of delivery in an organization.
- Work should never be assigned to individuals.
- Effective team has a maximum size of 7-9 people.
- Organizations need to maximize trust between people on a team.
- High trust enables a team to innovate and experiment.

#### Smaller Size Fosters Trust

- Type and depth of relationship we can have with people has clear limits:
  - Around five people--limit of people with whom we can hold close personal relationships and working memory
  - Around fifteen people--limit of people with whom we can experience deep trust
  - Around fifty people--limit of people with whom we can have mutual trust
  - Around 150 people--limit of people whos capabilities we can remember
- There are natural restrictions on the size of effective groupings within any organization.
- Patterns and rules that work at a smaller scale will probably fail to work at a larger scale.
- Important to limit the size of team groupings to help achieve predictable behavior and interactions from those teams:
  - A single team: around five to eight (based on industry experience)
    - In high-trust organizations: no more than fifteen people
  - Families ("tribes"): groupings of teams of no more than fifty people
    - In high-trust organizations: groupings of no more than 150 people
  - Divisions/streams/profit & loss (P&L) lines: groupings of no more than 150 or 500 people.

#### Work Flows to Long-Lived Teams

- Little value in reassigning people to different teams after a six-month project where the team has just begun to perform well.
- Adding new people to a team doesn't immediately increase its capacity. (Brook's Law)
  - This quite possibly reduces capacity during an initial stage.
  - A ramp-up period is necessary to bring people up to speed.
  - Communication lines inside the team increases significantly with each new member.
  - Emotional adaptation required from both new and old team members
- Best approach to team lifespans is to keep the team stable.
- Teams should be stable but not static, changing only occasionally and when necessary.
- High-trust organizations may change teams once a year without major detrimental effects on team performance.

#### The Team Owns the Software

- Team ownership helps to provide the vital "continuity of care" that modern systems need.
- Team ownership also enables a team to think in multiple "horizons"--from exploration stages to exploitation and execution.
  - Horizon 1: Immediate future with products and services that will deliver results the same year.
  - Horizon 2: Covers the next few periods, with an expanding reach of the products and services.
  - Horizon 3: covers many months ahead, where experimentation is needed to assess market fit and suitability of new services, products, and features.
- The danger of allowing multiple teams to change the same system or subsystem is that no one owns either the changes made or the resulting mess.
- Awareness of and ownership over these different time horizons helps a team care for the code more effectively.
- Every part of the software system needs to be owned by exactly one team.
  - There should be no shared ownership of components, libraries, or code.
- Outside teams may submit pull requests or suggestions to the owning team.
- Individual team members should not feel like the code is theirs to the exclusion of others.
- Teams should view themselves as stewards or caretakers.
- Think of code as gardening, not policing.

#### Team Members Need a Team-First Mindset

- For teams to work, team members should put the needs of the team above their own:
  - Arrive for stand-ups and meetings on time.
  - Keep discussions and investigations on track.
  - Encourage a focus on team goals.
  - Help unblock other team members before starting on new work.
  - Mentor new or less experienced team members.
  - Avoid "winning" arguments and, instead, agree to explore options.

#### Embrace Diversity in Teams

- Research suggests that teams with members of diverse backgrounds tend to produce more creative solutions more rapidly and tend to be better at empathizing with other teams' needs.
- Diverse mix of people also appear to foster better results, as team members make fewer assumptions about the context and needs of their software users.
  - Having a variety of viewpoints and experiences helps teams traverse the landscape of solutions much more rapidly.
> People and oganizations benefit from a diverse workforce where differences spark positive energy.

#### Reward the Whole Team, Not Individuals

- Looking to reward individual performance in modern organization tends to drive poor results and damages staff behavior.
- With a team-first approach, the whole team is rewarded for their combined effort.

#### Good Boundaries Minimize Cognitive Load

- Teams with too high of a cognitive load cannot effictively own or safely evolve the software.

#### Restrict Team Responsibilities to Match Team Cognitive Load

- A team-first approach to cognitive load means limiting the size of the software system that a team is expected to work with.
- Cognitive load: The total amount of mental effort being used in the working memory.
- Three different kinds of cognitive load:
  - Intrinsic--relates to aspects of the task fundamental to the problem space (e.g. "What is the structure of a Java class?" "How do I create a new method?")
    - e.g. Knowledge of the computer language being used.
  - Extraneous--relates to the environment in which the task is done (e.g. "How do I deploy this component again?" "How do I configure this service?")
    - e.g. Details of the commands needed to instantiate a dynamic testing environment.
  - Germane--relates to aspects of the task that need special attention for learning or high performance (e.g. "How should this service interact with the ABC service?")
    - e.g. Specific aspects of the business domain that the application developer is programming.
- Need to minimize intrinsic cognitive load through:
  - Training
  - Good choice of technologies
  - Hiring
  - Pair programming
  - etc.
- Need to eliminate extraneous cognitive load altogether through:
  - Automation
  - etc.
- "Value add" thinking lies with the germane cognitive load.
- Limiting cognitive load for a team means limiting the size of the subsystem the team works on.

#### Measure the Cognitive Load Using Relative Domain Complexity

> Do you feel like you're effective and able to respond in a timely fashion to the work you are asked to do?

- When measuring cognitive load, what we really care about is the domain complexity.

#### Limit the Number and Type of Domains per Team

- Domain complexity is not static.
- Downplaying complexity will lead to failure.
- To get started:
  1. Identify distinct domains that each team has to deal with
  2. Classify these domains into:
      - simple (most of the work has a clear path of action).
      - complicated (changes need to be analyzed and might require a few iterations on the solution to get right).
      - complex (solutions require a lot of experimentation and discovery).
- Domain complexity should be compared relative to each other.
- Heuristics:
  1. Assign each domain to a single team. If domain is too large, split into subdomains--each subdomain gets assigned to a single team.
  2. Single team should accomodate two to three "simple" domains.
  3. A team responsible for a complex domain should not have any more domains assigned to them--not even a simple one.
      - This is due to the cost of disrupting the flow of work (solving complex problems takes time and focus) and prioritization (there will be a tendency to resolve the simple, predictable problems first--delaying the complex problem)
  4. Avoid a single team responsible for two complicated domains.

#### Match Software Boundary Size to Team Cognitive Load

- Instead of choosing between a monolithic or a microservices architecture, design the software to fit the maximum team cognitive load.
- To increase the size of a software subsystem or domain, maximize the cognitive capacity of the team (by reducing the intrinsic and extraneous types of load):
  - Provide a team-first working environment.
  - Minimize team distractions during the workweek by limiting meetings, reducing emails, assigning a dedicated team or person to support queries, etc.
  - Change the management style by communicating goals and outcomes rather than obsessing over the "how"
  - Increase the quality of developer experience for other teams using your team's code and APIs through good documentation, consistency, good UX, and other DevEx practices.
  - Use a platform that is explicitly designed to reduce cognitive load for teams building software on top of it.
- Further benefit of taking a team-first approach is that the team tends to easily develop a shared mental model.

#### Design "Team APIs" and Facilitate Team Interactions

- The team API and well-defined team interactions are ways to produce a coherent, dynamic network of cleanly communicating teams.

#### Define "Team APIs" that Include Code, Documentation, and User Experience

- An API is a description and specification for how to interact programmatically with software, so we extend this idea to entire interactions with the team.
- The Team API includes:
  - Code: runtime endpoints, libraries, clients, UI, etc. produced by the team
  - Versioning: how the team communicates changes
  - Documentation
  - Practices and principles: the team's preferred ways of working
  - Communication: team's approach to remote communication tools
  - Work information: what the team is working on right now, what's next, and overall priorities for short and medium term
  - Other: anything else other teams need to use to interact with the team

- Should explicitly consider usability by other teams.

#### Facilitate Team Interactions for Trust, Awareness, and Learning

- Provide time, space, and money to enable and encourage people from different teams to learn from each other.
- Two critical ways this helps build trust and awareness are:
  - a consciously designed physical and virtual environment
  - time away from desks at guilds, communities of practice

#### Explicitly Design the Physical and Virtual Environments to Help Team Interactions

- Office design should accommodate the following modes of work:
  - focused individual work
  - collaborative intra-team work
  - collaborative inter-team work

#### Warning: Engineering Practices Are Foundational

#### Summary: Limit Teams' Cognitive Load and Facilitate Team Interactions to Go Faster

- Due to limits on team size, there is an upper limit to a team's cognitive load.
- Therefore, there should be a limit on the size of domain complexity that a team is responsible for.