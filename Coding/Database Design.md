# Database Design

Source: [Database Design Process - Neso Academy](https://www.youtube.com/watch?v=13d7ApTnJ3Y)

## Summary

Database design is the process of moving from user requirements to an implementable database schema. The important path is:

1. Collect and analyze requirements.
2. Build a conceptual schema.
3. Map the conceptual schema into a logical schema.
4. Decide the physical design.

The video spends time on examples, but the reusable lesson is the design flow: understand what data users need, model that data with entities and relationships, convert the model into the database model used by the DBMS, then tune storage and access details.

## Database Design Process

### 1. Requirements Collection and Analysis

Interview or ask users what data they need and what operations they expect to perform.

Capture two kinds of requirements:

- **Data requirements**: what information must be stored.
- **Functional requirements**: what operations users need, such as creating, updating, searching, reporting, or deleting data.

The output should be a concise requirements document that is detailed enough to guide design decisions.

Guidelines:

- Write requirements in plain business language first.
- Identify nouns as candidate entities.
- Identify facts about those nouns as candidate attributes.
- Identify verbs or associations as candidate relationships.
- Record constraints early, such as uniqueness, required fields, allowed values, and ownership rules.

### 2. Conceptual Design

Create a high-level conceptual schema using an ER model or another conceptual data model.

This schema should describe:

- **Entity types**: the main objects the database stores data about.
- **Attributes**: the properties of each entity.
- **Relationships**: how entities are connected.
- **Constraints**: rules that must always be true.

This stage should avoid implementation details. The goal is a model that both technical and non-technical people can understand and validate.

Guidelines:

- Choose clear entity names from the problem domain.
- Mark candidate keys and primary identifiers.
- Separate independent entities from dependent entities.
- Decide relationship cardinality, such as one-to-one, one-to-many, or many-to-many.
- Check whether each requirement is represented in the model.

### 3. Logical Design / Data Model Mapping

Transform the conceptual schema into the implementation data model used by the DBMS, usually the relational model.

The output is a logical database schema: tables, columns, primary keys, foreign keys, and relationship tables.

Guidelines:

- Convert each strong entity into a table.
- Convert attributes into columns.
- Convert relationships into foreign keys or junction tables.
- Use primary keys for stable row identity.
- Use foreign keys to preserve referential integrity.
- Normalize the schema enough to avoid unnecessary duplication and update anomalies.

### 4. Physical Design

Decide how the logical schema will be stored and accessed in the actual DBMS.

This may include:

- File organization or storage structures.
- Indexes.
- Access paths.
- Transaction design.
- Performance-related implementation decisions.

Guidelines:

- Add indexes for frequent lookup, join, and filtering paths.
- Avoid indexing every column by default because indexes slow writes and consume storage.
- Design transactions around real user operations.
- Keep physical optimization separate from the conceptual model; do not distort the business model too early for performance.

## Weak Entity Types

A **weak entity** does not have a complete key of its own. It depends on an owner entity for identification.

Guidelines:

- Use a weak entity when a record cannot be uniquely identified without its parent.
- Represent the dependency with an identifying relationship.
- Store the owner key as part of the weak entity's identity.
- Do not pretend a weak entity has a natural unique key if it is only partially unique.

Example pattern:

- `Employee` can be a strong entity.
- `Dependent` may be weak if a dependent name is only unique for a specific employee, not globally unique.

## ER Diagram Symbols

Common ER notation from the video:

| Concept | Symbol |
| --- | --- |
| Regular entity | Rectangle |
| Weak entity | Double rectangle |
| Attribute | Oval |
| Key attribute | Underlined oval |
| Multi-valued attribute | Double oval |
| Derived attribute | Dashed oval |
| Identifying relationship | Double relationship line |

## Practical Checklist

Before creating tables, confirm:

- The user requirements are written down.
- Functional operations are listed.
- Main entities are identified.
- Attributes are assigned to the correct entities.
- Primary keys or candidate keys are known.
- Weak entities are recognized.
- Relationships and cardinalities are defined.
- Constraints are documented.
- The conceptual model is understandable without DBMS-specific details.
- The logical schema has primary keys, foreign keys, and normalized tables.
- Physical choices like indexes are based on expected queries and transactions.

## Practical Takeaway

Do not start database design by immediately creating tables. Start with requirements, build a conceptual model, validate entities and relationships, map that model into tables, and only then decide physical details like indexes and storage.
