# figma-to-rag 🎨→📚 (work-in-progress!!)

Convert Figma designs into RAG-optimized content for AI applications. This tool helps bridge the gap between design systems and AI by making your Figma content queryable through RAG (Retrieval-Augmented Generation).

## Features ✨

- Extract content from Figma files including:
  - Text content and styles
  - Component information
  - Design hierarchies
  - Style tokens
- Convert to RAG-optimized format
- Process content using OpenAI for enhanced documentation
- Comprehensive error handling and logging
- Progress tracking and detailed reports
- Multiple output formats


## Workflow Overview 🔄

```mermaid
flowchart LR
    subgraph Input ["1. Input"]
        F[Figma Design]
    end
    
    subgraph Process ["2. Processing"]
        C[Extract Content]
        R[Convert to RAG]
        O[OpenAI Enhancement]
    end
    
    subgraph Output ["3. Output"]
        D[Documentation]
        J[JSONL Files]
    end
    
    F --> C
    C --> R
    R --> O
    O --> D
    D --> J
    
    style F fill:#ffffff,stroke:#000000,stroke-width:4px
    style C fill:#ffffff,stroke:#000000,stroke-width:2px,stroke-dasharray: 5 5
    style R fill:#ffffff,stroke:#000000,stroke-width:2px,stroke-dasharray: 10 5
    style O fill:#ffffff,stroke:#000000,stroke-width:2px,stroke-dasharray: 15 5
    style D fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:dots
    style J fill:#ffffff,stroke:#000000,stroke-width:4px,pattern:grid
```
## System Architecture 🏗️

```mermaid
graph TB
    subgraph Interface ["User Interface"]
        CLI([Command Line Tool])
    end
    
    subgraph Core ["Core Processing"]
        direction LR
        Conv[["Figma Converter"]]
        Proc[["OpenAI Processor"]]
    end
    
    subgraph External ["External Services"]
        direction LR
        FA[("Figma API")]
        OA[("OpenAI API")]
    end
    
    subgraph Storage ["Data Storage"]
        Raw[("Raw Data")]
        Processed[("Processed Data")]
    end
    
    CLI --> Conv & Proc
    Conv --> FA
    Proc --> OA
    FA --> Raw
    OA --> Processed
    
    style CLI fill:#ffffff,stroke:#000000,stroke-width:4px
    style Conv fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:dots
    style Proc fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:grid
    style FA fill:#ffffff,stroke:#000000,stroke-width:2px,stroke-dasharray: 5 5
    style OA fill:#ffffff,stroke:#000000,stroke-width:2px,stroke-dasharray: 5 5
    style Raw fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:cross
    style Processed fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:cross
```

## Processing Steps 🔄

```mermaid
sequenceDiagram
    participant U as User
    participant C as CLI
    participant F as Figma API
    participant O as OpenAI
    participant S as Storage
    
    Note over U,S: 1️⃣ Content Extraction
    U->>C: Run convert command
    C->>F: Request file content
    F-->>C: Return design data
    C->>S: Save raw content
    
    Note over U,S: 2️⃣ Content Processing
    U->>C: Run process command
    C->>S: Load raw content
    C->>O: Process content
    O-->>C: Return enhanced docs
    
    Note over U,S: 3️⃣ Output Generation
    C->>S: Save RAG documents
```

## Output Structure 📊

```mermaid
graph TD
    subgraph "Output Directory Structure"
        Root(("📁 output/"))
        Data["📂 data/"]
        Logs["📂 logs/"]
        
        Root --> Data
        Root --> Logs
        
        Data --> |Raw Content| RC["📄 raw_content.jsonl"]
        Data --> |Processed| PC["📄 processed_content.jsonl"]
        Data --> |Statistics| ST["📄 stats.json"]
        
        Logs --> |Conversion| CL["📝 convert_logs.log"]
        Logs --> |Processing| PL["📝 process_logs.log"]
        Logs --> |Errors| EL["📝 error_logs.log"]
    end
    
    style Root fill:#ffffff,stroke:#000000,stroke-width:4px
    style Data fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:dots
    style Logs fill:#ffffff,stroke:#000000,stroke-width:2px,pattern:grid
    style RC fill:#ffffff,stroke:#000000,stroke-width:2px
    style PC fill:#ffffff,stroke:#000000,stroke-width:2px
    style ST fill:#ffffff,stroke:#000000,stroke-width:2px
```

## Installation 🚀

### Using pip (TODO)

```bash
pip install figma-to-rag
```

### Using Poetry (for development)

```bash
git clone https://github.com/yourusername/figma-to-rag.git
cd figma-to-rag
poetry install
```

## Setup 🔧

1. Get your Figma access token:
   - Log in to Figma
   - Go to Settings → Account Settings
   - Scroll to "Access tokens"
   - Create a new token

2. Get your OpenAI API key:
   - Go to [OpenAI Platform](https://platform.openai.com)
   - Create or use existing API key

3. Set environment variables (optional):
```bash
export FIGMA_ACCESS_TOKEN='your_figma_token'
export OPENAI_API_KEY='your_openai_key'
```

## Usage 💻

### Basic Workflow

1. **Inspect a Figma file**:
```bash
figma-to-rag inspect your_file_key
```

2. **Convert Figma content**:
```bash
figma-to-rag convert your_file_key --output-dir ./output
```

3. **Process with OpenAI**:
```bash
figma-to-rag process ./output/data/your_file_key_raw.jsonl \
    --model gpt-4 \
    --output-dir ./output
```

4. **Validate the processed content**:
```bash
figma-to-rag validate ./output/data/processed_your_file_key_raw.jsonl
```

### Advanced Usage

#### Custom OpenAI Processing
```bash
# Use specific model
figma-to-rag process input.jsonl --model gpt-4o

# Adjust batch size
figma-to-rag process input.jsonl --batch-size 3

# Increase retry attempts
figma-to-rag process input.jsonl --retry 5
```

#### Detailed Analysis
```bash
# Get statistics about processed content
figma-to-rag stats ./output/data/processed_content.jsonl -o ./output/stats.json
```

## Output Format 📄

The tool generates JSONL files with RAG-optimized content:

```json
{
  "content": "Detailed component description and documentation",
  "metadata": {
    "element_type": "component/text/frame",
    "title": "Element title",
    "context": "Design hierarchy location",
    "style_tokens": {
      "font": "Inter",
      "weight": 500,
      "size": "16px"
    },
    "related_elements": [
      "related-component-1",
      "related-component-2"
    ]
  }
}
```

## Error Handling 🚨

The tool includes comprehensive error handling:
- API rate limiting
- Network issues
- Processing failures
- Validation errors

Errors are logged to:
```
./output/logs/[command]_[timestamp].log
```

## Development 🛠️

### Setup

```bash
# Clone repository
git clone https://github.com/yourusername/figma-to-rag.git
cd figma-to-rag

# Install dependencies
poetry install

# Run tests
poetry run pytest

# Run with coverage
poetry run pytest --cov=figma_to_rag
```

### Running Tests

```bash
# All tests
poetry run pytest

# Specific test file
poetry run pytest tests/test_converter.py

# With coverage report
poetry run pytest --cov=figma_to_rag --cov-report=html
```

## Contributing 🤝

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Run tests (`poetry run pytest`)
4. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
5. Push to the branch (`git push origin feature/AmazingFeature`)
6. Open a Pull Request

## Troubleshooting 🔍

### Common Issues

1. **"No access token provided"**
   ```bash
   export FIGMA_ACCESS_TOKEN='your_token_here'
   # or
   figma-to-rag convert file_key --access-token your_token_here
   ```

2. **OpenAI API Issues**
   ```bash
   # Try with smaller batch size
   figma-to-rag process input.jsonl --batch-size 3
   
   # Increase retry attempts
   figma-to-rag process input.jsonl --retry 5
   ```

3. **"File not found"**
   - Check if the file key is correct
   - Ensure you have access to the Figma file
   - Try the inspect command first:
     ```bash
     figma-to-rag inspect your_file_key
     ```

## License 📝

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Support 💬

- 📫 For bugs and feature requests, please [open an issue](https://github.com/yourusername/figma-to-rag/issues)
- 💡 For questions and discussions, please use [GitHub Discussions](https://github.com/yourusername/figma-to-rag/discussions)
- 📖 Check out our [Wiki](https://github.com/yourusername/figma-to-rag/wiki) for additional documentation

## Acknowledgments 🙏

- Figma API Documentation
- OpenAI API
- The open-source community
