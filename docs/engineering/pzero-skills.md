## What it does

`pzero-skills` gives the agent the framework-specific rules it needs to build production-ready pzero services: REST API structure, RPC definitions and middleware, database access, migrations, and generated models.

It treats generation as the starting point rather than an afterthought. The agent defines the descriptor, runs `pzero gen`, and adds business logic inside the generated Handler → Logic → Model shape instead of inventing a parallel structure.

## When to reach for it

Type `/pzero-skills`, or the agent reaches for it automatically when a task uses pzero.

Reach for it when creating or changing a pzero REST API, RPC service, gateway, SQL model, migration, or framework configuration. For general module boundaries that are not specific to pzero, use [codebase-design](https://aihero.dev/skills-codebase-design); for implementing an already-specified change end to end, use [implement](https://aihero.dev/skills-implement).

## Generate first

The leading idea is **generate first**: update the `.api`, `.proto`, or SQL descriptor before implementing logic, then let pzero establish the files and interfaces the application code should occupy. That keeps handlers thin, dependencies in the service context, and database work on generated field constants and condition chains.

The skill loads only the relevant reference for the task. REST work gets the API file rules; RPC work gets proto structure, validation, and middleware guidance; database work gets connection, CRUD, model-generation, and migration patterns.

## It's working if

- API descriptors include the pzero-required package, group, and compact-handler settings.
- Business logic stays out of handlers and uses generated interfaces and field constants.
- Schema changes include forward and rollback migrations before models are regenerated.
- Database filters use `condition.NewChain()` rather than the obsolete constructor.

## Where it fits

`pzero-skills` is a reach-for-it-anytime framework reference. It complements [codebase-design](https://aihero.dev/skills-codebase-design), which supplies framework-neutral module vocabulary, and [implement](https://aihero.dev/skills-implement), which drives a complete build workflow. For the whole map, see [ask-matt](https://aihero.dev/skills-ask-matt).
