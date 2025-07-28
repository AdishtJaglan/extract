## Overview

This project extracts a clean, hierarchical outline from PDF documents, including the **Title**, and headings at levels **H1**, **H2**, and **H3**, along with their corresponding page numbers. The output is a JSON file containing the structured outline for each processed PDF.

## Approach

1. **Built-in Outline Extraction**: Use `pdfjs-dist`'s `getOutline()` API to retrieve bookmarks or document outline if available.
2. **Fallback via Font Metrics**:
   - Extract all text items along with their computed font sizes and vertical positions.
   - Cluster the most frequent font sizes to identify heading levels (top three sizes map to H1, H2, H3).
   - Sort and merge consecutive runs of identical levels for readability.
3. **Title Derivation**: If multiple H1 headings exist, the first H1 is selected as the document title and removed from the outline list.
4. **Filesystem I/O**: Recursively process all PDFs in `/app/input` and write corresponding JSON files to `/app/output`.

## Models & Libraries

- **Node.js**: JavaScript runtime for server-side execution.
- **pdfjs-dist (legacy build)**: Core PDF parsing and rendering API.
- **canvas** (optional): Provides `DOMMatrix` and `Path2D` polyfills for environments lacking browser DOM.
- **fs-extra**: Extended filesystem operations, including JSON read/write and directory management.
- **path**: Node.js utility for path resolution across platforms.

## Prerequisites

- Docker installed on a Linux/amd64 environment.
- A folder structure:
  ```
  ├── input/    # Place your .pdf files here
  └── output/   # Generated .json files will be written here
  ```

## Docker Setup

The solution runs inside a Docker container to ensure consistent environment and dependencies.

### 1. Build the Docker Image

```bash
docker build --platform linux/amd64 -t mysolutionname:somerandomidentifier .
```

### 2. Run the Container

```bash
docker run --rm \
  -v "$(pwd)/input:/app/input" \
  -v "$(pwd)/output:/app/output" \
  --network none \
  mysolutionname:somerandomidentifier
```

- **--rm**: Automatically remove the container after execution.
- **-v**: Bind mount local directories for input and output.
- **--network none**: Isolate the container from external networks.

## Expected Output

For each `filename.pdf` in `/app/input`, a corresponding `filename.json` will be generated in `/app/output`:

```json
{
  "title": "<Document Title>",
  "outline": [
    { "level": "H1", "text": "Introduction", "page": 1 },
    { "level": "H2", "text": "What is AI?", "page": 2 },
    { "level": "H3", "text": "History of AI", "page": 3 }
  ]
}
```

## Notes

- If the PDF contains no built-in outline, the font-metrics method ensures headings are still identified.
- Errors during PDF parsing are logged to the console, and processing continues for remaining files.
- Adjust the Docker commands as needed for different host environments or CI pipelines.

