# llm-parse

Offline HTML/markdown parsing, chunking, and text analysis in a single C++ header.

## Features
- HTML state-machine parser: extract text, links, headings; decode entities
- Markdown parser: strip formatting, extract code blocks and links
- Text chunker with paragraph/sentence boundary preference and configurable overlap
- TextStats: word/sentence counts, HTML/markdown/code heuristics
- Zero dependencies, fully offline, C++17, namespace `llm`

## Quick Start
```cpp
#define LLM_PARSE_IMPLEMENTATION
#include "llm_parse.hpp"

auto result = llm::parse_html("<h1>Hello</h1><p>World</p>");
std::cout << result.text;

auto chunks = llm::chunk(long_text, {.max_chars = 500});
```

## API
```cpp
HtmlResult     parse_html(const std::string& html);
MarkdownResult parse_markdown(const std::string& md);
std::string    clean_text(const std::string& text);
std::vector<std::string> chunk(const std::string& text, const ChunkConfig& = {});
TextStats      analyze(const std::string& text);
```

## License
MIT — Mattbusel, 2026

## See Also

| Repo | Purpose |
|------|---------|
| [llm-stream](https://github.com/Mattbusel/llm-stream) | SSE streaming |
| [llm-cache](https://github.com/Mattbusel/llm-cache) | Response caching |
| [llm-cost](https://github.com/Mattbusel/llm-cost) | Token cost estimation |
| [llm-retry](https://github.com/Mattbusel/llm-retry) | Retry + circuit breaker |
| [llm-format](https://github.com/Mattbusel/llm-format) | Markdown/code formatting |
| [llm-embed](https://github.com/Mattbusel/llm-embed) | Embeddings + cosine similarity |
| [llm-pool](https://github.com/Mattbusel/llm-pool) | Connection pooling |
| [llm-log](https://github.com/Mattbusel/llm-log) | Structured logging |
| [llm-template](https://github.com/Mattbusel/llm-template) | Prompt templates |
| [llm-agent](https://github.com/Mattbusel/llm-agent) | Tool-use agent loop |
| [llm-rag](https://github.com/Mattbusel/llm-rag) | Retrieval-augmented generation |
| [llm-eval](https://github.com/Mattbusel/llm-eval) | Output evaluation |
| [llm-chat](https://github.com/Mattbusel/llm-chat) | Multi-turn chat |
| [llm-vision](https://github.com/Mattbusel/llm-vision) | Vision/image inputs |
| [llm-mock](https://github.com/Mattbusel/llm-mock) | Mock LLM for testing |
| [llm-router](https://github.com/Mattbusel/llm-router) | Model routing |
| [llm-guard](https://github.com/Mattbusel/llm-guard) | Content moderation |
| [llm-compress](https://github.com/Mattbusel/llm-compress) | Prompt compression |
| [llm-batch](https://github.com/Mattbusel/llm-batch) | Batch processing |
| [llm-audio](https://github.com/Mattbusel/llm-audio) | Audio transcription/TTS |
| [llm-finetune](https://github.com/Mattbusel/llm-finetune) | Fine-tuning jobs |
| [llm-rank](https://github.com/Mattbusel/llm-rank) | Passage reranking |
| [llm-parse](https://github.com/Mattbusel/llm-parse) | HTML/markdown parsing |
| [llm-trace](https://github.com/Mattbusel/llm-trace) | Distributed tracing |
| [llm-ab](https://github.com/Mattbusel/llm-ab) | A/B testing |
| [llm-json](https://github.com/Mattbusel/llm-json) | JSON parsing/building |
