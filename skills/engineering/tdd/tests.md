# Good and Bad Tests

## Good Tests

**Integration-style**: Test through real interfaces, not mocks of internal parts.

```cpp
// GOOD: Tests observable behavior through an owned interface
TEST(DoorLockController, LocksAfterValidCommand) {
  DoorLockController controller;
  controller.handle(Command::Lock);
  EXPECT_EQ(controller.state(), LockState::Locked);
}
```

Characteristics:

- Tests behavior users/callers care about
- Uses public API only
- Survives internal refactors
- Describes WHAT, not HOW
- One logical assertion per test

Test through an owned header or callable seam, use an independently derived expected result, and run with the build flags and sanitizers relevant to the host configuration. Keep target-only timing, memory-map, interrupt, peripheral, and integration claims for the environment that can observe them.

```cpp
// GOOD: observable C++ contract with an independent expected value
TEST(Crc32, MatchesPublishedVector) {
  const std::array<std::uint8_t, 9> input{'1','2','3','4','5','6','7','8','9'};
  EXPECT_EQ(crc32(input), 0xCBF43926u);
}
```

## Bad Tests

**Implementation-detail tests**: Coupled to internal structure.

```cpp
// BAD: Couples the test to an internal collaborator and call count
TEST(DoorLockController, CallsTransitionHelperOnce) {
  MockTransitionHelper helper;
  DoorLockController controller{helper};
  controller.handle(Command::Lock);
  EXPECT_CALL(helper, transitionTo(LockState::Locked)).Times(1);
}
```

Red flags:

- Mocking internal collaborators
- Testing private methods
- Asserting on call counts/order
- Test breaks when refactoring without behavior change
- Test name describes HOW not WHAT
- Verifying through external means instead of interface
- Claiming target behaviour from a host-only double

```cpp
// BAD: Reads private backing storage to verify behavior
controller.handle(Command::Lock);
EXPECT_EQ(controller.internal_state_, 3);

// GOOD: Verifies the owned observable contract
controller.handle(Command::Lock);
EXPECT_EQ(controller.state(), LockState::Locked);
```

**Tautological tests**: Expected value restates the implementation, so the test passes by construction.

```cpp
// BAD: Expected value repeats the implementation formula
const auto expected = samples[0] + samples[1] + samples[2];
EXPECT_EQ(filter.sum(samples), expected);

// GOOD: Expected value comes from a reviewed worked example
EXPECT_EQ(filter.sum({2, 3, 5}), 10);
```
