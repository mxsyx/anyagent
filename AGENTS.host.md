## Code Reuse

- Before adding a feature or implementation, search the project for identical or similar implementations and examine how they are used.
- Reuse existing implementations when they meet the requirements. For minor differences, prefer configuration through parameters, composition, or modest extensions.
- Consider a separate implementation only when reuse would blur responsibilities, introduce unreasonable coupling, or significantly increase complexity.
- When extracting shared logic, ensure it serves a clearly defined common responsibility. Avoid over-abstraction merely to eliminate superficial duplication.

## Style

- Name storage keys using the `a.b.c` format.

## Comments

- Prefer clear naming, types, and code structure to express intent rather than relying on comments to explain code.
- Add comments for business rules, complex algorithms, unusual edge cases, and design trade-offs that are difficult to understand from the code alone. Focus on why rather than what.
- Do not add comments that merely restate code behavior or explain the obvious.
- When modifying or removing code, update or delete any related comments that are no longer valid.

## Performance

- Prefer simple implementations with lower computational, memory, and resource overhead, while preserving correctness and maintainability.
- Avoid redundant computations and queries, unnecessary rendering, resource loading, and memory allocations.
- For expensive functionality outside critical paths, prefer lazy loading, on-demand initialization, or deferred execution to avoid unnecessary startup overhead.
- Before introducing complexity through optimizations such as caching, concurrency, or precomputation, identify the performance bottleneck and expected benefit. Measure the results when the benefit cannot be determined directly.
- Do not introduce additional complexity without an actual performance problem.

## Documentation

- Store documents produced by the Agent during implementation under `docs/` at the project root, organized into the following categories:
  - `docs/design/` (Design): Architectural designs, feature designs, and other design decisions.
  - `docs/development/` (Development): Documents used by the Agent for ongoing development, including TODOs and implementation logs.
  - `docs/features/` (Feature Descriptions): Feature descriptions intended to support future product documentation, technical documentation, and website content.
- Each category directory must contain a `README.md` serving as a documentation index, with relative links and brief descriptions. Update the index whenever documents are added, moved, renamed, or deleted.
- Store temporary files such as verification results, benchmark reports, and screenshots in `tmp/` at the project root.
- Files in `tmp/` may be deleted at any time. Preserve conclusions and information needed for ongoing development or future documentation in the appropriate `docs/` category. Do not rely on files in `tmp/` for long-term reference.
