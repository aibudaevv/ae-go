# Go Style

Read before changing Go source. This page keeps repository conventions compact;
the linked baseline guidance provides background and examples when needed.

The baseline is the standard Go toolchain and established Go guidance:

- [`gofmt`](https://pkg.go.dev/cmd/gofmt) defines mechanical formatting;
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments) defines
  common review conventions;
- [Effective Go](https://go.dev/doc/effective_go) provides foundational idioms,
  but it is not a complete guide to newer language features.

## Formatting and imports

- Run `gofmt` on every changed `.go` file. Do not imitate its output manually
  or align code with spaces.
- Use `goimports` when the repository already requires it. Otherwise organize
  imports as standard library, a blank line, then third-party and local imports.
- Do not impose an arbitrary line-length limit. Break a long expression only
  when the result is easier to read.
- Do not place multiple statements on one line.
- Do not edit generated files manually. Change the source or generator and
  regenerate the artifact.

## Naming

- Use short, descriptive names whose meaning is clear at the point of use.
- Use `MixedCaps` or `mixedCaps`, not underscores, for Go identifiers.
- Keep initialisms consistently capitalized: `ID`, `HTTP`, `URL`, `API`, and
  names such as `userID` and `httpClient`.
- Use short, lower-case, single-word package names. Avoid generic packages such
  as `util`, `common`, `helpers`, `types`, or `interfaces` unless the domain
  gives the name a precise meaning.
- Avoid package-name stutter: prefer `user.Service` to `user.UserService` when
  the shorter name remains unambiguous.
- Use short, consistent receiver names derived from the type. Do not use
  `this`, `self`, or a different receiver name across methods of one type.
- Name getters after the value (`Owner`), not with a `Get` prefix
  (`GetOwner`), unless an external contract requires it.

## Declaration groups

Group related package-level declarations of the same kind using `type (...)`,
`const (...)`, or `var (...)`. A group must express one concept, lifecycle, or
configuration set—not merely collect declarations that share a keyword.

Keep single declarations standalone when grouping adds no meaning, and keep
unrelated declarations separate.

Within a group, keep declarations in a stable logical order. Put a blank line
between meaningful subgroups only when they still belong to the same overall
concept. Do not use a package-level `var` when a constant, function result, or
explicit dependency can avoid mutable global state.

## Function layout

Make functions read as a small sequence of logical stages: validate, acquire
data, apply rules, perform side effects, and return the result.

Separate logically complete stages with one blank line. Keep statements that
form a single operation together. In particular, a call and its immediate error
check are one logical unit and must not be split by a blank line.

Do not add blank lines mechanically after every statement. Whitespace must show
the structure of the function rather than make short related code look
disconnected.

Prefer small functions with one responsibility. Extract a helper when it gives
a block a meaningful name, isolates a decision, or makes testing simpler—not
merely to reduce line count.

## Conditional flow

Prefer guard clauses and early returns to deeply nested control flow. Do not add
an `else` after a branch that always returns, breaks, continues, or panics.

Separate independent adjacent `if` statements with one blank line. Keep an
`if`/`else if`/`else` chain together. Keep an operation and its error-checking
`if` together; add a blank line before the next independent condition or stage.

Use a `switch` when it expresses a multi-way decision more clearly than a long
sequence of mutually exclusive conditions. Avoid boolean parameters that make
call sites unclear; prefer distinct operations or an explicit option type.

## Interfaces and APIs

- Define interfaces at the point of consumption unless a shared contract has a
  clear owner.
- Keep interfaces small and cohesive. Do not create an interface solely because
  a mocking tool expects one.
- Accept interfaces when callers may reasonably supply different behavior;
  return concrete types by default.
- Preserve useful zero values. Require constructors only when a value would
  otherwise be invalid or needs dependencies.
- Avoid exported identifiers without a real external consumer.
- Use typed structures instead of `map[string]any` when the data has a known
  schema.

## Context, errors, and cleanup

- Pass `context.Context` as the first parameter of request-scoped or cancellable
  functions. Do not store it in a struct or replace it with a custom context
  type.
- Check every meaningful error. Discard an error only when the API guarantees
  it is irrelevant, and document the reason when that is not obvious.
- Wrap errors with operation context and `%w` when callers may inspect the
  cause. Error text starts with lower case and has no trailing punctuation.
- Use `errors.Is` and `errors.As`; do not compare wrapped errors by their text.
- Do not use `panic` for ordinary failures or untrusted input.
- Acquire and release resources in the same visible ownership scope where
  practical. Place `defer` immediately after successful acquisition.

## Comments

- Comments explain intent, invariants, constraints, or non-obvious tradeoffs.
  Do not narrate code that already states what it does.
- Documentation comments for exported declarations start with the declaration's
  name and form a complete sentence.
- Keep TODOs actionable and attach a task identifier when one exists.
- Remove stale commented-out code; version control already preserves history.

Review these conventions with the final diff; `gofmt` must produce no further
changes. Follow the [verification schedule](../../AGENTS.md#verification).
