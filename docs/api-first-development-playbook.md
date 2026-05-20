# API-First Development Playbook

This playbook expands on the existing API-first notes and gives a practical end-to-end flow for OpenAPI-driven projects.

## 1. Kickstart the project from a contract-first baseline

1. Generate the app with API First enabled.
2. Keep `src/main/resources/swagger/api.yml` as the source of truth.
3. Optionally keep a starter JDL only for entities and relationships that must exist before delegate implementation.

## 2. Regenerate API artifacts from the OpenAPI spec

Use the existing generator commands after each `api.yml` change:

```bash
./mvnw generate-sources
```

or

```bash
./gradlew openApiGenerate
```

Generated interfaces and delegate entry points should be treated as generated code.

## 3. Implement delegates and isolate mapping concerns

Implement generated delegates in `@Service` classes and keep transport/domain mapping separate.

Recommended pattern:

- Delegate methods orchestrate use cases.
- MapStruct mappers convert API DTOs to entities and entities back to DTOs.
- Business rules remain in domain/service classes.

## 4. Keep generated and handwritten layers separated

To avoid regeneration conflicts:

- Do not hand-edit generated OpenAPI classes.
- Keep handwritten logic in delegates, services, and mappers.
- Re-run generation when the contract changes and keep manual code outside generated sources.

## 5. Verify API-first behavior with focused tests

At minimum, include tests for:

- Delegate behavior for success/error paths.
- Mapper round-trips for key DTO/entity pairs.
- Contract compatibility for representative request/response payloads.

## 6. Suggested iterative workflow

1. Update `api.yml`.
2. Regenerate sources.
3. Implement/adjust delegate services and mappers.
4. Run tests.
5. Repeat until contract and implementation converge.

---

Context: proposed as a documentation companion for `jhipster/generator-jhipster#27215`.