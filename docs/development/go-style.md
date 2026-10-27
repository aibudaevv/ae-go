# Go Style

Read this document before changing any Go source file. Write code that looks
natural to an experienced Go developer and is easy to review as a sequence of
logical operations.

The baseline is the standard Go toolchain and established Go guidance:

- [`gofmt`](https://pkg.go.dev/cmd/gofmt) defines mechanical formatting;
- [Go Code Review Comments](https://go.dev/wiki/CodeReviewComments) defines
  common review conventions;
- [Effective Go](https://go.dev/doc/effective_go) provides foundational idioms,
  but it is not a complete guide to newer language features.

The rules below add repository-wide conventions where the standard formatter
does not make a decision.

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

```go
type (
	UserID string
	Role   string
)

const (
	RoleMember Role = "member"
	RoleAdmin  Role = "admin"
)

var (
	ErrUserNotFound = errors.New("user not found")
	ErrAccessDenied = errors.New("access denied")
)
```

Keep a single declaration standalone when grouping adds no meaning:

```go
type Service struct {
	repository Repository
}
```

Do not combine unrelated declarations into one large block:

```go
type UserID string

type Config struct {
	Address string
}
```

Within a group, keep declarations in a stable logical order. Put a blank line
between meaningful subgroups only when they still belong to the same overall
concept. Do not use a package-level `var` when a constant, function result, or
explicit dependency can avoid mutable global state.

## Function layout

Make each function read from top to bottom as a small number of logical stages,
for example:

1. validate inputs and invariants;
2. acquire required data;
3. apply business rules;
4. persist state or perform side effects;
5. construct and return the result.

Separate logically complete stages with one blank line. Keep statements that
form a single operation together. In particular, a call and its immediate error
check are one logical unit and must not be split by a blank line:

```go
user, err := repository.User(ctx, userID)
if err != nil {
	return User{}, fmt.Errorf("get user: %w", err)
}

result := buildResult(user)
return result, nil
```

Do not add blank lines mechanically after every statement. Whitespace must show
the structure of the function rather than make short related code look
disconnected.

Prefer small functions with one responsibility. Extract a helper when it gives
a block a meaningful name, isolates a decision, or makes testing simpler—not
merely to reduce line count.

## Conditional flow

Prefer guard clauses and early returns to deeply nested control flow. Do not add
an `else` after a branch that always returns, breaks, continues, or panics.

Separate independent adjacent `if` statements with one blank line:

```go
if request.Name == "" {
	return ErrNameRequired
}

if request.Limit > maxLimit {
	return ErrLimitExceeded
}
```

An `if`/`else if`/`else` chain represents one decision and stays together:

```go
if score >= excellentScore {
	grade = GradeExcellent
} else if score >= passingScore {
	grade = GradePassing
} else {
	grade = GradeFailed
}
```

Do not insert a blank line between an operation and the `if` that checks its
result. Do insert a blank line before the next independent condition or stage:

```go
token, err := parser.Parse(rawToken)
if err != nil {
	return fmt.Errorf("parse token: %w", err)
}

if token.Expired() {
	return ErrTokenExpired
}
```

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

## Review checklist

Before handing off a Go change, verify that:

1. `gofmt` produces no further changes.
2. Names read naturally with their package qualifiers.
3. Related declarations are grouped and unrelated declarations remain separate.
4. Blank lines expose logical stages without splitting a call from its error
   check.
5. Independent `if` statements are visually separated and conditional chains
   remain intact.
6. Dependencies, interfaces, errors, contexts, and resource ownership follow
   the rules above.
7. The smallest relevant build, test, static-analysis, and race checks have run.
