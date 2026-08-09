## What it does

`pzero-skills` gives the agent the framework-specific rules it needs to build production-ready pzero services: REST API structure, RPC definitions and middleware, in-process scheduled jobs, PostgreSQL database access, migrations, and generated models.

It treats generation as the starting point rather than an afterthought. The agent defines the descriptor, runs `pzero gen`, and adds business logic inside the generated Handler → Logic → Model shape instead of inventing a parallel structure. Schema changes run through explicit service migrate commands — never on server startup.

## When to reach for it

Type `/pzero-skills`, or the agent reaches for it automatically when a task uses pzero.

Reach for it when creating or changing a pzero REST API, RPC service, gateway, SQL model, migration, or framework configuration. For general module boundaries that are not specific to pzero, use [codebase-design](https://aihero.dev/skills-codebase-design); for implementing an already-specified change end to end, use [implement](https://aihero.dev/skills-implement).

## Generate first

The leading idea is **generate first**: update the `.api`, `.proto`, or SQL descriptor before implementing logic, then let pzero establish the files and interfaces the application code should occupy. That keeps handlers thin, dependencies in the service context, and database work on generated field constants and condition chains.

The skill loads only the relevant reference for the task. REST work gets the API file rules; RPC work gets proto structure, validation, middleware, and job-pattern guidance; database work gets connection, CRUD, model-generation, and migration patterns (PostgreSQL/pgx through the generated service `migrate` command).

## It's working if

- API descriptors include the pzero-required package, group, and compact-handler settings.
- Business logic stays out of handlers and uses generated interfaces and field constants.
- Schema changes use `<service> migrate create` / `migrate up` as an explicit release step, not during server startup.
- Migrations are PostgreSQL (`pgx`) only; `desc/sql` stays a schema snapshot, not a `pzero gen --desc` input.
- Database filters use `condition.NewChain()` rather than the obsolete constructor.
- Scheduled jobs use config-driven schedules with `overlap: skip` for singleton tasks, and registry keys match `job.jobs` entries one-to-one.

## Where it fits

`pzero-skills` is a reach-for-it-anytime framework reference. It complements [codebase-design](https://aihero.dev/skills-codebase-design), which supplies framework-neutral module vocabulary, and [implement](https://aihero.dev/skills-implement), which drives a complete build workflow. For the whole map, see [ask-matt](https://aihero.dev/skills-ask-matt).
