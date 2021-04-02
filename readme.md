# Goals and Requirements

1. Teachers submit assignments
2. Students submit code
3. System grades assignments
4. System reports grades
5. Auditors audit system (what are the auditing requirements?)

# Assumptions and Decision Drivers

1. Auditability
2. Capture market share
3. Quick Sign up

# Context / System Interactions

![System Interactions Dataflow](http://pointillism.io/mckaysalisbury/grades/prod/dataflow.gv)

# Details and Decisions

![DB](http://pointillism.io/mckaysalisbury/grades/prod/db.gv)

## Decisions

### Capture Market Share
Capture market share means **Fast time to market**. Which means **simpler architecture** over flexibility and scalability for the future. Each of the pieces could be separated, but it might be easier to do development in one repo as one system, but the principle of cautious design tells us to plan for the future, so those boundaries might make for good separatable modules for the future. Also, after an upload multiple tasks need to be performed. Multiple task queues might be better, but a single topic-based system is probably sufficient.

#### The "That's a good problem to have" philosophy
In general, it's a good idea to not solve problems that are good problems to have, because you'll be in a better position then.

If we get the customers we need, and need to scale more, or be more flexible, then that means we've been somewhat successful, and we probably will have the time needed to make the architecture better. So making one service might make more sense. Tools for service meshes and service template creation, and optimizations in devops might make a true microservice architecture implementation faster to build in the future, but I don't think we're there yet.

### Auditability
Although it was previously mentioned that a simpler architecture is preferred, auditability must come first, so a more robust queueing system for tasks that need to be accomplished seems like a mandatory tradeoff. (we'll probably also want to have backups or at least alerts for the task queues) We'll also probably want to be saving the logs and test for repeatability.

### Quick Sign up
Because schools will only give us a short time to sign up. We will need to build integrations with the Learning Management System (LMS) providers. We should also be prepared to have that code be isolated for quick growth and unimpeded development. 

While we will want to plan for sending grades to the LMS (otherwise what's the purpose of the LMS?), I'm not sure if LMS providers even store assignment data. An integration with assignments doesn't seem mandatory for a first pass.

# Risks
There could be more bigger risks than this, but these are the biggest that come to mind:

* **Unknowns**. Are we missing something important?
  * Get more eyes on the proposal. See what might be missing.
  * Do we have or can we get a school or professor to help us with standard use cases to see if we're missing something important?
  * Is assignment integration with the LMSs nmandatory? Will that reduce adoption? If so, it's not mandatory for testing, so build that as a fast follow.
* **Technical limitations**
  * This seems feasible to be developed in the alloted time and budget, but does the team agree?
  * I think the systems should be able to handle the load required, but the database might be the bottleneck
* **Financial**
  * *Does it actually make sense to build this?* With this plan, and discussions with the team, costs can be estimated, and pricing models decided (The pricing models might affect system design, which would affect costs, especially in the future.). Along with market analysis performed, it can help us answer that question. (Along with the related question: *Do we have enough money to start building this?*)