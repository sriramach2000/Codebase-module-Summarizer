# Codebase Analyzer

A Python utility for analyzing, augmenting, and managing codebases using AWS S3 storage. This tool provides summarization of modules and functions in a codebase, with optional enhanced summaries generated by Claude API.

## Overview

This tool parses code files from various programming languages, extracts modules and functions, generates meaningful summaries, and stores the augmented files in an S3 bucket. Key features include:

- Support for multiple programming languages (Python, JavaScript, C/C++, Java, Shell, etc.)
- Automatic module extraction and summarization
- Function detection and intelligent summary generation
- Optional enhanced summaries using Claude 3.7 API
- AWS S3 integration for storage and retrieval
- Comprehensive testing framework

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/s3-codebase-analyzer.git
cd s3-codebase-analyzer

# Install requirements
pip install python-dotenv boto3 pandas pydantic matplotlib tqdm anthropic
```

## Environment Setup

Create a `.env` file with your AWS credentials:

```
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=your_region
S3_BUCKET_URI=s3://your-bucket/prefix
```

For Claude API integration, create a separate env file:

```
API_KEY=your_claude_api_key
```

## Usage

### Basic Usage

```python
from codebase_processor import CodebaseProcessor

# Initialize processor
processor = CodebaseProcessor(use_claude_api=True)

# Process a codebase
result = processor.process_codebase("/path/to/codebase")

# Get augmented files
augmented_files = processor.get_augmented_files(result)

# Print summary statistics
print(f"Processed {len(result['augmented_codebase']['files'])} files")
print(f"Found {len(result['all_modules'])} modules")
print(f"Found {result['function_count']} functions")
```

### S3 Integration

```python
from s3_handler import S3Handler, S3Credentials

# Set up credentials
credentials = S3Credentials(
    bucket_uri="s3://your-bucket/prefix",
    aws_access_key_id="your_access_key",
    aws_secret_access_key="your_secret_key",
    region_name="your_region"
)

# Upload to S3
s3_handler = S3Handler(credentials)
s3_handler.upload_json(result, "analyzed_codebase.json")
```

### Enhanced Summaries with Claude API

```python
from claude_integration import ClaudeAPIHandler, initialize_claude_api
from codebase_processor import enhance_summaries_with_api

# Initialize Claude API
claude_api = initialize_claude_api()

# Process codebase with basic summarization
result = processor.process_codebase("/path/to/codebase")

# Enhance summaries with Claude API
enhanced_result = enhance_summaries_with_api(result)
```

## Key Components

### CodeParser

Extracts module dependencies from different programming languages:

- Python: `import os`, `from pathlib import Path`
- JavaScript/TypeScript: `require('module')`, `import x from 'module'`
- C/C++: `#include <header>`, `#include "header"`
- Shell: `source file.sh`, common utility detection

### ModuleSummarizer

Generates concise descriptions of modules based on predefined patterns and language-specific heuristics.

### FunctionParser

Identifies functions and methods across different languages, extracting:
- Function signatures
- Parameters
- Docstrings (when available)
- Context information

### CodebaseProcessor

Orchestrates the entire process:
1. Parses codebase files
2. Extracts modules and functions
3. Generates summaries
4. Optionally enhances summaries with Claude API
5. Produces augmented files with summary comments

### S3Handler

Manages interactions with Amazon S3:
- File uploading/downloading
- JSON handling
- Object listing
- File existence checking

## Testing

The project includes a comprehensive testing framework:

```python
# Run all tests
success, summary = run_all_tests()

# Run specific test groups
test_code_parser()
test_module_summarizer()
test_enhanced_summarizer()
test_codebase_processor()
test_credential_handler()
```

## Advanced Usage

### Customizing Augmentation

```python
# Process only a subset of files
result = processor.process_codebase("/path/to/codebase", sample_limit=50)

# Update existing augmented data with new summaries
updated_result = update_files_with_enhanced_summaries(result)

# Force update all files in S3
upload_to_s3(processor, result, credentials, force_update=True)
```

### Visualizing Results

```python
from visualization import visualize_summary_stats

# Show statistics about generated summaries
visualize_summary_stats(result)
```

