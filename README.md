# Llama3

## Requirements

- Java 25+ (uses `MemorySegment` mmap).
- Maven 3.x

## Architecture overview

This project exposes a flexible, extensible model API:

- `Model` interface: `com.llama4j.model.Model`
- Model factory: `com.llama4j.model.ModelFactory`
- Tokenizer factory: `com.llama4j.tokenizer.TokenizerFactory`
- Configuration base type: `com.llama4j.config.ModelConfiguration`

Each model implementation supplies its own configuration class, tokenizer provider, and a service provider entry in
`src/main/resources/META-INF/services`.

## Adding a new model

1. Create a new `Model` implementation (for example `com.llama4j.model.MyModel`) and a configuration class that extends
   `com.llama4j.config.ModelConfiguration`.
2. Add a `ModelProvider` implementation that returns your model.
3. Implement a `TokenizerProvider` and register it in
   `src/main/resources/META-INF/services/com.llama4j.tokenizer.TokenizerProvider`.
4. Register your `ModelProvider` in
   `src/main/resources/META-INF/services/com.llama4j.model.ModelProvider`.

No changes are needed in the shared factories once the providers are registered.

## Download a model (GGUF)

Download a `Q4_0` or `Q8_0` GGUF file, for example:

```bash
# Llama 3.2 (3B)
curl -L -O https://huggingface.co/mukel/Llama-3.2-3B-Instruct-GGUF/resolve/main/Llama-3.2-3B-Instruct-Q4_0.gguf

# Llama 3.2 (1B)
curl -L -O https://huggingface.co/mukel/Llama-3.2-1B-Instruct-GGUF/resolve/main/Llama-3.2-1B-Instruct-Q8_0.gguf
```

## Build

```bash
mvn package
```

## Tests

```bash
mvn test
```

## Run

```bash
java --add-modules jdk.incubator.vector \
  -jar target/llama3-1.0.0-SNAPSHOT.jar \
  --model /path/to/model.gguf \
  --chat
```

## Run from source

```bash
mvn -q exec:java \
  -Dexec.mainClass=com.llama4j.cli.LlamaCli \
  -Dexec.args="--model /path/to/model.gguf --chat"
```
