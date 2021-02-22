# @typechain/ethers-v5

## 6.0.0

### Major Changes

- cd73777: Improve typings for events. Generate types for `queryFilter` for events.

  Note: This is a breaking change as it requires using TypeScript >= 4.0.0 (previously 3.9 was fine.)

  Example:

  ```typescript
  const filter = contract.filters.Transfer() // TypedEventFilter<>
  const result = await contract.queryFilter(filter) // TypedEvent<>

  result[0].args.from // type support for named event parameters
  result[0].args[0] // type support by index

  contract.on(filter, (from, to, value, event) => {
    from // string
    to // string
    value // BigNumber
    event // TypedEvent<>
  })
  ```

## 5.0.0

### Major Changes

- 0d4b293: Changed return type of functions from a object with number indexes, to an array merged with object containing
  named outputs.

  Before, solidity function like this:

  ```
  function x() public pure returns (uint256)
  ```

  Generated such method signature:

  ```typescript
  x(overrides?: CallOverrides): Promise<{0: BigNumber}>;
  ```

  New output is:

  ```typescript
  x(overrides?: CallOverrides): Promise<[BigNumber]>;
  ```

  The difference is that now you can use standard array destructuring while working with output types.

### Patch Changes

- db5baa5: Do not generate typings in contract type itself for reserved keywords that would collide with ethers
  internals