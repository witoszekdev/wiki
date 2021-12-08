# Domain Driver Design Distilled

by Vaugh Vernon

Business often use Scrum to deliver software in a timely manner, which is not the purpose of Scrum - it's purpose is *knowledge aquisition*.

> [...] design is inevitable. The alternative to good design is bad design, not no design at all

When we make no design about software, we're basically making bad design.

Problems that DDD can solve:
- Business views software as a nessesity, not an advantage (there might not be a fix for that, because of business culture)
- Developers are too wrapped up in technology rather than design (chasing new "shiny objects")
- Database is given too much priority, discussion focus on data model not business processes and operations
- Developers don't give though about naming things - this creates a gap between business and software mental models
- Poor colaboration between business - they write a lot of documentation, that developers don't use
- Persistance in business logic, business logic inside user interface components
- Bad database queries that block users from performing time-sensitive operations
- Wrong abstractions for "future needs" not actuall business needs
- Strongly coupled services, that depend on one another

Fixing bad design is more expensive than building it right in the first place.

## Strategic design

**Bounded Context** - is a semantic sontextual boundary; this means that within the bondary each component of the software model has a specific meaning and does specific things.

When we cross a country border the oficial language changes. (ex. French is used in France, German in Germany). Bounded Context is the same for language - a language barier.

**1 team per Bounded Context** - the team can work on multiple Bounded Contexts but only 1 team should be the owner of that context. This eliminates the change of any surprises that arise when another team makes a change to the source code.

**Ubiquitous Language** - language developed by team working in the *Bounded Context*, it's spoken by every memeber of the team that creates the software within the *Bounded Context*. It's *ubiquitous* because it's spoken both by team members and it's implemented in the software model. Everyone at the team understand what is mean with precision and constraints when words from *ubiquitious language* are used. A form of this language is the source code.

The same words can mean different things based on what *bounded context* they're in - any words outside the context aren't sexpected to adhere to the same definitions.

**Core Domain** - *bounding context* that is developed as a key strategic initiative of the organisation. It's most important - because it is a means to achive greateness ✨. It is used to distinguish organisation from other competitors or at the very least to address a major line of business.

**Big Ball of Mud** - happens when a system has multiple tangled models without explicit boundaries between them. (+ usally multiple teams work on it, which makes things worse). Tests take a lot of time, and are often skipped when devs are in hurry. Product is trying to do too much, with too many people, in the wrong place. Trying to speak *Ubiquitous Language* will result in abandoning it.

Big Ball of Mud happens when devs don't speak with business experts.

Business depertnemtns or work groups can provide a good indication when model boundaries should exist - we should find at least 1 business expert per business function. Even if the business doesn't have well defined departments we should still find domain experts.

Why? Because each business function has likely a different definition for the same term.
Example: flight
  - pilots: single takeoff and landing
  - maintenance: defined by law
  - tickets: can be nonstop, or one-stop (z przesiadką, bez przesiadki, z międzylądowaniem, etc.)
Everyone uses the same word but it means different thing to the team that uses it.

Ask yourself: "Are there 2+ meanings for this word?" Yes - then you have a 2+ *bounded contexts*.
We don't have to name the word with added prefix of *bounded context* (ex. MaintenanceFlight / TicketFlight) - the name is simply *Flight* in all *bounded contexts*.

### Filtering concepts

Come concepts will be inside the *Bounded Context* some will be out - we need to ask ourselves **"what is core?"**

Concepts that survive this core-only filtering are part of the *Ubiquitous Language* of the team that owns the *Bounded Context*.

In order to decide what is and isn't core we need to bring *Domain Experts*.

Who is Domain Expert?
- Focused on business concerns
- Like Scrum Master in Scrum (understands how Scrum is executed on a project)
- Product Owner is not a subsitute for Domain Expert
  - PO is usually more focused on managing and priorizing the product backlog
  - This doesn't mean that they're expersts in the business core competency
- We want to use their *mental model*

Developers
- Cannot be so technically centered that they can't accept the business focus
- Business model is more complex than technical aspects of the project (that's why you're using DDD)

Conversation > documents
The *ubiquitious language* is developed by collaborative feedback loop, not documents.

#### Example

Scrum software (aka JIRA) - Ubiquitous Language is Scrum

We have:
- backlog
- tasks
- release items
- sprint items
- esimations

That's our core

Other stuff like:
- User account management
- Billing
- HR scheduling
- Calendar entries

Are not part of the core Agile bounded context.

### Developing the Ubiquitous Language

We don't have to use only nouns - we can express the *Core Domain* as a set of concrete scenarios about what the domain model is supposed to do.

Those aren't user storier or use cases - we want to describe what each component of the *Core Domain* is supposed to do.

⚠️ Beware time spent on keeping documents, diagrams, drawings up to date
They aren't the domain model. They are just tools. In the end code is the model, and model is the code. We don't want the upkeeping of documents to become a burden.

When we write the scenarios we might as well write *acceptance criteria tests*

### Maintenance

**Maintenance phase != end of innovation**

Ubiquitous Language that was developed early on must thrive as years pass - if a long-term commitment cannot be made, is our model really a *Core Domain*?

### Architecture

Our domain model should be kept free of technology

Types of architecture that work along with DDD:
- Ports and Adapters
- Event-Driven Architecture, Event Sourcing
- Command Query Responsibility Segregation (CQRS)
- Reactive and Actor Model
- Representational State Transfer (REST)
- Service-Oriented Architecture (SOA)
- Microservices (they're essentialy rquivalent to Bounded Context, can be even more narrow)
  - microservice < bounded context (in size)

