# Scripts

Utility scripts and tools that are not part of the main application package.

## Current Scripts

Scripts currently in root directory:
- `../setup_project.sh` - Automated project setup
- `../update_kb_chroma.py` - Knowledge base management utility
- `pdf_processor_medgemma.py` - PDF processing helper for MedGemma knowledge base

## Purpose

This directory should contain:
- Setup/installation scripts
- Database management tools
- Data import/export utilities
- Development helper scripts
- Maintenance tasks
- One-off migration scripts

## Usage Examples

```bash
# Run setup script
bash setup_project.sh

# Update knowledge base
python update_kb_chroma.py output/medgemma_kb_*.json

# Process PDFs for MedGemma knowledge base
python pdf_processor_medgemma.py path/to/file.pdf
```


Scripts here are meant to be run directly, not imported as modules.
