# Table-Driven Tests and Mc/DC

Tests for Go services use table-driven cases. For every changed non-trivial
decision, design Modified Condition/Decision Coverage (Mc/DC): each atomic
condition must be shown to independently affect the decision outcome.

## Table shape

Each row has a descriptive name, the relevant input conditions, and the expected
observable result. Run rows as subtests so that a failing case identifies the
scenario.

```go
func TestCanPublish(t *testing.T) {
	t.Parallel()

	tests := []struct {
		name                       string
		authenticated, editor, live bool
		want                       bool
	}{
		{"all conditions allow publish", true, true, true, true},
		{"authentication independently denies", false, true, true, false},
		{"editor role independently denies", true, false, true, false},
		{"live account independently denies", true, true, false, false},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := canPublish(tt.authenticated, tt.editor, tt.live); got != tt.want {
				t.Fatalf("canPublish() = %v, want %v", got, tt.want)
			}
		})
	}
}
```

For the decision `authenticated && editor && live`, the first row paired with
each following row proves that changing exactly one condition changes the
outcome. That is Mc/DC, not merely line or branch coverage.

## Designing Mc/DC cases

1. Identify the decision and every atomic boolean condition it evaluates.
2. For each condition, choose two valid cases where all other relevant
   conditions are fixed and the decision result changes only because that
   condition changed.
3. Put the pairs in the table or document the pair relationship in a short
   adjacent comment when it is not obvious.
4. Assert the public result of the decision: returned value, domain error,
   state change, emitted command, or response contract. Do not assert a private
   implementation detail just to raise coverage.

For compound expressions with `||`, nested decisions, or short-circuit behavior,
choose pairs that demonstrate each condition's independent contribution. Include
the relevant false and true paths; do not assume an aggregate coverage number
proves the pairs exist.

## Domain invariants

Sometimes a condition cannot vary independently in valid input because a domain
invariant makes a combination impossible. Do not manufacture invalid production
states merely to satisfy a coverage metric. Instead, document the invariant in a
test comment, cover every valid combination, and test the validation boundary
that rejects invalid input.

## Test scope by layer

- `domain` tests are pure, deterministic unit tests for invariants and decisions.
- `app` tests use fakes for ports and verify use-case behavior, error mapping,
  cancellation, and retry/transaction decisions.
- `adapter` tests are contract or integration tests for HTTP, gRPC, SQL, queues,
  and other external boundaries.
- End-to-end tests cover critical integrations, not every branch already proven
  by lower layers.

Keep tests independent of network access, wall-clock timing, and execution
order. For a bug fix, add a regression case that fails without the source
change. For concurrency changes, run race detection and test cancellation,
shutdown, and error propagation.
