

- **Basic Concepts**:
    
    - Effect type is central to the library, generic over three parameters: requirements, error, and value.
    - An effect is a function-like construct that describes a program without executing it immediately.

- **Effect Types**:
    
    - **Unit Effect**: No dependencies, no errors, returns void.
    - **Basic Effect Creation**: Use `succeed` for success and `fail` for failure.
    - **Sync Constructor**: Defers side effects until the effect is run.
    - **Try Constructor**: Catches errors from synchronous functions.
    - **Promise Constructor**: Models asynchronous computations.


- **Running Effects**:
    - Use `runSync` for synchronous effects and `runPromise` for asynchronous effects.
    - Effects should be run at the edges of the program.

- **Transformation Functions**:
    
    - **Pipe**: Allows chaining of transformation functions.
    - **Map**: Applies a function to the value inside an effect.
    - **FlatMap**: Merges effects, allowing for chaining while preserving error handling.
    - **Tap**: Executes side effects without altering the original value.
    - **All**: Combines multiple effects into a single effect.


- **Example Program**:
    
    - Generates random numbers, fetches Pokémon data from an API, validates JSON, and calculates the heaviest Pokémon.
    - Uses `tryPromise` for API calls and schema validation for data structure.

- **Error Handling**:
    
    - Define custom error types for better error management.
    - Use `catchAll` for handling all errors and `catchTag` for specific error types.

- **Dependency Injection**:
    
    - Define dependencies using a tag and provide them before running effects.
    - Allows for flexible data fetching implementations.

- **Logging and Benchmarking**:
    
    - Use logging functions to observe program behavior.
    - Adjust log levels and use `withLogSpan` for timing sections of the program.
    - Effects can be run concurrently to improve performance.