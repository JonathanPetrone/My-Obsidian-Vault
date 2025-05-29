const (
    StateInitialized = iota  // 0
    StateDone                // 1
    StateCrash               // 2
)

#### How It Works
1. `iota` is reset to `0` whenever a new `const` block is started.
2. In the first line of the block, `StateInitialized` is assigned the value `0`.
3. In the next line, `StateDone` is assigned the value `1` because `iota` automatically increments by `1`.