# FlossWare Memory Java

Java implementation of the language-neutral FlossWare Memory storage contract.

## Included backends

- `RamMemoryStore`: process-local, ephemeral storage.
- `FileSystemMemoryStore`: one UTF-8 payload per file, with encoded IDs and atomic replacement where supported.
- `DecoratingMemoryStore`: base class for composable store decorators.
- `RamCacheMemoryStore`: read-through/write-through RAM cache around any `MemoryStore`.

The filesystem implementation is intended for single-process use. The delegate remains authoritative when wrapped by the RAM cache; changes made directly to the delegate are not observed by an existing cache. The outermost decorator owns and closes its delegate.

## Example

```java
MemoryStore store = new RamCacheMemoryStore(
    new FileSystemMemoryStore(Path.of("./memory-data"))
);
try (store) {
    store.put("session-1", "remember this");
    String value = store.get("session-1").orElseThrow();
}
```

Requires Java 17+. Run tests with `mvn test`.

The shared architecture and REST contract belong in [FlossWare/memory](https://github.com/FlossWare/memory).