# Common Memory Issues in C Programming

## Incorrect memory accesses

### Using uninitialized variables

### Out-of-bounds memory accesses (read/write underflow/overflow bugs)

#### Write overflow

A bug where a write is attempted into a memory buffer after its last legally accessible location

#### Write underflow

A write is attempted into a memory buffer before its first legally accessible location

#### Read underflow

A read is attempted on a memory buffer before its first legally accessible location

#### Read overflow

A read is attempted on a memory buffer after its last legally accessible location

### Use-after-free/use-after-return (out-of-scope) bugs

### Double-free

## Leakage

## Undefined Behavior

## Data Races
