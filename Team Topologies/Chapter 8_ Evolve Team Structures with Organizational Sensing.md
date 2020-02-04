# Chapter 8: Evolve Team Structures with Organizational Sensing
____

> The design ... is almost never the best possible, [so] the prevailing system concept may need to change. Therefore, flexibility of organization is important to effective design.
- When designing modern organizations for building and running software systems, the most important thing is not the shape of the organization itself but the decision rules and heuristics used to adapt and change the organization as new challenges arise.

#### How Much Collaboration Is Right for Each Team Interaction?

- Neither model, collaborating or X-as-a-Service, is better or worse than the other; they simply are useful for different kinds of work.
> In teams which score highly on architectural capabilities [which directly lead to higher performance], little communication is required between delivery teams to get their work done, and the architecture of the system is designed to enable teams to test, deploy, and change their systems without dependencies on other teams. In other words, architecture and teams are loosely coupled.
- Any ongoing collaboration must be justified as valuable discovery, valuable capability building, or valuable deficiency-filling activity.
- Deliberate use of a change in team interaction to force a beneficial change in delivery capability is the essence of strong, strategic technology leadership.

#### Accelerate Learning and Adoption of New Practices

- "Deliberate collaboration" is particularly useful where two groups have very different prior experience due to the prevailing practices around their respective technologies.
- This collaboration period will also help the organization assess whether the cloud software or the embedded devices can and should be t reated as a platform with the corresponding behaviors from the platform-providing team.

#### Constant Evolution of Team Topologies

- What happens when the requirements or operating context changes?
- Interaction modes of different teams should be expected to change regularly.
- In a larger enterprise, this "discover to establish" pattern is expected to happen all the time with different teams at different stages.
- Close collaboration often doesn't scale across the organization.
- Aim should be to try to establish a well-defined and capable platform that many teams can simply use as a service.

#### Combining Team Topologies for Greater Effectiveness

- Different teams within an organization will have different interaction and collaboration needs.
- Different kinds of team topologies should be expected to be seen simultaneously.

### Triggers for Evolution of Team Topologies

- Can be difficult to have the required organizational self-awareness to detect when it's time to evolve the team structure.

#### Trigger: Software Has Grown Too Large for One Team

- **Symptoms**
  - A startup company grows beyond fifteen people (Dunbar's number).
  - Other teams spend lots of time waiting on a single team to undertake changes.
  - Changes to certain components or workflows in the system routinely get assigned to the same people, even when they're already busy or away.
  - Team members complain about lack of system documentation

- **Context**
  - As software grows, it becomes increasingly difficult understanding the codebase.
  - Can lead to specialization within the team regarding different components of the system.
  - Cycle of specialization is a local optimization that can negatively affect the team's overall flow of work.
    - Can introduce bottlenecks in delivery.
  - Can also negatively affect individual motivation.
  - Team may no longer hold a holistic view of the system, losing self-awareness to realize when the system has become too large.

#### Trigger: Delivery Cadence is Becoming Slower

- **Symptoms**
  - Team members qualitatively feel it takes longer to release changes than it used to.
  - Team velocity or throughput metrics show a clear downward variation compared to one year ago. (There is always some variation, so make sure it's not accidental)
  - Team members complain that the delivery process used to be simpler, with fewer steps.
  - Work in progress keeps increasing, with many changes waiting for another team's action
- **Context**
  - Long-lived, high-performing product team should steadily improve cadence with these prerquisites:
    - Autonomy over entire lify cycle
    - No hard dependencies on external teams
    - Being able to self-serve new infrastructure via an internal platform
  - QA -- functional silo -- all teams delivering software will need to wait on the QA team to be available to test their updates.
  - Delivery slowing can be a result of accrued technical debt.
    - Complexity of codebase may reach a state where even small changes are costly and frequently cause regressions

#### Trigger: Multiple Business Services Rely On a Large Set of Underlying Services

- **Symptoms**
  - Stream-aligned teams have limited visibility of end-to-end flow within their service area.
  - It becomes difficult to achieve a smooth and rapid flow of change due to the number and complexity of subsystem integrations.
  - Attempts to "reuse" an existing set of services and subsystems becomes more and more challenging.
- **Context**
  - Solution to multi-service integration problems is:
    - "Platformize" the lower-level services and APIs with a thin "platform wrapper" that provides a consistent DevEx for stream-aligned teams with things like request-tracking correlation IDs, health-check endpoints, test harnesses, service-level objectives, and diagnostic APIs.
    - Use stream-aligned teams for each high-level business service responsible for operational telemetry and fault diagnosis.

### Self Steer Design and Development

- Operation of software should act as and provide valuable signals to the development activities.

#### Treat Teams and Team Interactions as Senses and Signals

- Organizational sensing uses teams and their internal and external communications as the "senses" of the organization.
- What kinds of things should an organization sense? Ask these questions:
  - Have we misunderstood how users need/want to behave?
  - Do we need to change team-interaction modes to enhance thow the organization is working?
  - Should we still be building thing X in house? Should we be renting it from an external provider?
  - Is the close collaboration between Team A and Team B still effective? Should we move toward an X-as-a-Service model?
  - Is the flow of work for Team C as smooth as it could be? What hampers flow?
  - Does the platform for teams D, E, F, and G provide everything those teams need? Is an enabling team needed for a period of time?
  - Are the promises between these two teams still valid and achievable? What needs to change to make the promises more realistic?

#### IT Operations as High-Value Sensory Input to Development

- IT operations teams can be used as high-fidelity sensory input for development teams, requiring joined-up communications between teams running systems (Ops) and teams building systems (Dev).
- Three Ways of DevOps for modern, high-performing organizations:
  - Systems thinking: optimize for fast flow across the whole organization, not just in small parts
  - Feedback loops: Development informed and guided by Operations
  - Culture of continual experimentation and learning: sensing and feedback for every team interaction
- Vital that communication paths are established and nurtured between Ops and Dev.
> Businesses normally treat operations as an output of design ... In order to empathize, though, one must be able to hear. In order to hear, one needs input from operations. Operations thus becomes an input to design.
- Need to care about the software long after finished coding a feature.
- Avoid "maintenance" or "business as usual" teams whose remit is simply to maintain existing software.
- Having separate teams for new-stuff and BAU also tends to prevent learning between these two groups.

### Summary: Evolving Team Topologies

- Organizations need to expect to adapt and evolve their organization structure on a regular basis.
- Organizations need to ensure that their team interactions optimize for flow, Conway's Law, and a team-first approach.

