# When to Mock

Use a mock or test double only at a boundary whose relevant contract it can represent:

- Third-party services or vendor stacks
- Persistent storage (sometimes; prefer a faithful test instance when practical)
- Time/randomness
- File system (sometimes)
- OS services, clocks, buses, peripherals, or hardware registers when the test is explicitly host/simulator-scoped

Don't mock:

- Your own classes/modules
- Internal collaborators
- Anything you control
- Timing, concurrency, ABI, memory, electrical, or hardware behaviour and then claim production fidelity

## Designing for Mockability

At system boundaries, design owned interfaces that are easy to substitute without exposing platform details. A mock proves caller logic and interaction contracts; it does not replace simulator, emulator, target, or HIL evidence.

**1. Use dependency injection**

Pass external dependencies in rather than creating them internally:

```cpp
// Easy to substitute on host while preserving the owned clock contract
class Controller {
 public:
  explicit Controller(Clock& clock) : clock_{clock} {}
 private:
  Clock& clock_;
};

// Hard to control and hides the platform dependency
Timestamp Controller::now() {
  return read_hardware_timer_register();
}
```

**2. Prefer domain operations over generic transport access**

Create specific functions for each external operation instead of one generic function with conditional logic:

```cpp
// GOOD: Operations carry owned meaning and typed results
class VehicleBus {
 public:
  virtual Result publish_status(StatusFrame frame) = 0;
  virtual Result request_diagnostic(DiagnosticRequest request) = 0;
};

// BAD: Tests must reproduce address, width, ordering, and transport policy
class GenericTransport {
 public:
  virtual Bytes transact(Address address, Bytes payload) = 0;
};
```

The domain-operation approach means:
- Each double returns one specific result type
- Less conditional logic in test setup
- The exercised contract is visible
- Transport details stay behind the adapter
