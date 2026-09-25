# llm-parse

[![CI](https://github.com/Mattbusel/llm-parse/actions/workflows/ci.yml/badge.svg)](https://github.com/Mattbusel/llm-parse/actions/workflows/ci.yml)
![C++17](https://img.shields.io/badge/C%2B%2B-17-blue.svg)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

**Turn HTML, markdown and messy text into clean chunks for LLM pipelines.** One header, `llm_parse.hpp`. Needs nothing beyond the C++17 standard library.

Before a document can be embedded, summarised or stuffed into a prompt it has to be cleaned up: tags stripped, code blocks pulled out, whitespace normalised and the text split into overlapping chunks. llm-parse does that offline in one header, with no HTML or markdown library to install.

## Features

- `parse_html()`: visible text plus title, links and headings; `strip_html()` for plain text only
- `parse_markdown()`: plain text plus code blocks, headings and links; `strip_markdown()`
- `clean_text()`: whitespace normalisation, optional URL and email redaction, length cap
- `chunk()`: size and overlap control, prefers paragraph and sentence boundaries
- `analyze()`: character, word, sentence and paragraph counts, and guesses whether text is code, HTML or markdown
- Fully offline, standard library only

## Quick start

Copy the header into your project:

```bash
curl -fsSLO https://raw.githubusercontent.com/Mattbusel/llm-parse/main/include/llm_parse.hpp
```

Define `LLM_PARSE_IMPLEMENTATION` in exactly one `.cpp` file before including it; every other file just includes the header. Save this as `main.cpp` next to the header:

```cpp
#define LLM_PARSE_IMPLEMENTATION
#include "llm_parse.hpp"
#include <iostream>

int main() {
    auto page = llm::parse_html(
        "<html><head><title>Docs</title></head><body>"
        "<h1>Install</h1><p>Run <b>make</b>. See <a href=\"https://example.com\">the site</a>.</p>"
        "</body></html>");
    std::cout << "title: " << page.title << "\ntext: " << page.text << "\n";
    std::cout << "links: " << page.links.size() << ", headings: " << page.headings.size() << "\n";

    auto md = llm::parse_markdown("# Setup\n\n```bash\nmake\n```\nThen run it.");
    std::cout << "code blocks: " << md.code_blocks.size() << "\n";

    llm::ChunkConfig cc;
    cc.chunk_size = 200;
    cc.overlap    = 20;
    for (const auto& c : llm::chunk(page.text + "\n\n" + md.plain_text, cc))
        std::cout << "[chunk] " << c << "\n";

    auto stats = llm::analyze(md.plain_text);
    std::cout << stats.word_count << " words, markdown? " << stats.likely_markdown << "\n";
}
```

```bash
g++ -std=c++17 -O2 main.cpp -o demo
```

## API at a glance

| Call | Purpose |
|---|---|
| `parse_html(html, cfg)` | `HTMLParseResult{text, title, links, headings}` |
| `parse_markdown(md, cfg)` | `MarkdownParseResult{plain_text, code_blocks, headings, links}` |
| `clean_text(text, cfg)` | Normalise and redact per `ParseConfig` |
| `chunk(text, ChunkConfig)` | Overlapping chunks, sizes in characters |
| `analyze(text)` | `TextStats` |

## Notes and limitations

- This is a pragmatic tag stripper, not a full HTML5 parser: it is meant for extracting readable text, not for rendering or DOM work.
- Chunk sizes are measured in characters, not tokens.

## Build the examples

The repo builds `examples/html_parse.cpp`, `examples/markdown_parse.cpp`, `examples/chunker.cpp`, `examples/text_stats.cpp` with CMake:

```bash
cmake -B build
cmake --build build
```

## Part of llm-cpp

llm-parse is one of 26 single-header C++ libraries in [llm-cpp](https://github.com/Mattbusel/llm-cpp), a toolkit for building LLM features into native code. Each library stands alone; combine them by giving each `*_IMPLEMENTATION` define its own `.cpp` file. See the [llm-cpp README](https://github.com/Mattbusel/llm-cpp#using-several-together) for the full list and examples of using several together.

## License

MIT. See [LICENSE](LICENSE).
