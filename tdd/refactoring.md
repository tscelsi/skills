# Refactor Candidates

After TDD cycle, look for:

- **Duplication** → Extract function/class
- **Long methods** → Break into private helpers (keep tests on public interface)
- **Shallow modules** → Combine or deepen
- **Feature envy** → Move logic to where data lives
- **Primitive obsession** → Introduce value objects
- **Existing code** the new code reveals as problematic

Python-specific checks:

- Replace boolean/flag parameter clusters with value objects or small strategy types.
- Move implicit dict shapes to typed models (`dataclass`, `TypedDict`, or Pydantic model as appropriate).
- Keep side effects in thin adapter layers; keep domain modules mostly pure.
