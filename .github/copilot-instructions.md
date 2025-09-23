# Automations Repository - n8n Workflow Collection

**ALWAYS follow these instructions first and only fallback to additional search and context gathering if the information in these instructions is incomplete or found to be in error.**

This repository contains n8n workflow automation files stored as JSON configurations. These are ready-to-import workflows for the n8n automation platform that perform AI-powered tasks including newsletter generation and RAG (Retrieval-Augmented Generation) workflows.

## Working Effectively

### Prerequisites and Setup
- Install Node.js (v20+ recommended): `node --version && npm --version` to verify
- **CRITICAL**: n8n installation requires internet connectivity which may be blocked in sandboxed environments
- Attempt to install n8n globally: `npm install -g n8n` -- installation typically takes 3-5 minutes but may fail due to network restrictions
- If n8n installation fails due to network issues (ENOTFOUND errors), document this limitation but continue with JSON validation

### Repository Structure
```
.
├── README.md                           # Basic repository information
├── LICENSE                            # MIT license
├── n8n/                              # n8n workflow directory
│   ├── ai-agent-newsletter.json      # AI newsletter automation workflow
│   └── rag/                          # RAG workflow subdirectory
│       └── crawl4Ai-rag.json        # Web crawling and RAG workflow
```

### Workflow Validation
- **ALWAYS validate workflow JSON structure before making changes**
- Create and run validation script:
```bash
# Simple JSON and structure validation
for file in n8n/*.json n8n/*/*.json; do
  if [ -f "$file" ]; then
    echo "=== Validating $(basename "$file") ==="
    if jq . "$file" > /dev/null 2>&1; then
      echo "✅ Valid JSON structure"
      echo "📋 Workflow name: $(jq -r '.name' "$file")"
      echo "🔗 Node count: $(jq '.nodes | length' "$file")"
      credential_count=$(jq '[.nodes[] | select(.credentials)] | length' "$file")
      if [ "$credential_count" -gt 0 ]; then
        echo "🔐 Nodes requiring credentials: $credential_count"
      fi
    else
      echo "❌ Invalid JSON structure"
    fi
    echo
  fi
done
```
- **NEVER CANCEL**: Workflow validation takes 5-10 seconds. Always wait for completion.

### Build and Test Process
**IMPORTANT**: This repository does NOT have traditional build/compile steps since it contains configuration files.

#### JSON Validation (Required)
- `jq --version` -- verify jq is available for JSON processing
- Validate JSON syntax: `jq . n8n/ai-agent-newsletter.json > /dev/null && echo "✅ Valid JSON" || echo "❌ Invalid JSON"`
- Validate all workflows: `find n8n -name "*.json" -exec jq . {} \; > /dev/null && echo "✅ All JSON files valid"`
- **NEVER CANCEL**: JSON validation typically takes 2-3 seconds per file

#### Workflow Content Analysis
- Extract workflow metadata: `jq -r '.name' n8n/ai-agent-newsletter.json`
- List node types: `jq -r '.nodes[] | .type' n8n/ai-agent-newsletter.json | sort | uniq -c`
- Find credential requirements: `jq -r '.nodes[] | select(.credentials) | {name: .name, credentials: .credentials}' n8n/ai-agent-newsletter.json`

### n8n Platform Integration (When Available)
**WARNING**: These commands require n8n installation and may not work in restricted environments:
- Start n8n locally: `n8n start` -- takes 30-45 seconds to initialize. NEVER CANCEL.
- Import workflow: `n8n import:workflow --file=n8n/ai-agent-newsletter.json`
- **LIMITATION**: n8n requires a web browser interface for full functionality testing

## Validation Requirements

### Manual Validation Steps
Since this repository contains automation workflow configurations, validation focuses on:

1. **JSON Structure Validation** (REQUIRED)
   - Run JSON syntax validation on all workflow files
   - Verify required fields (name, nodes) are present
   - Check for malformed node configurations

2. **Workflow Logic Validation** (RECOMMENDED)
   - Verify node connections are valid
   - Check that credential placeholders are properly formatted
   - Ensure trigger nodes are configured correctly

3. **Dependency Analysis** (REQUIRED)
   - Identify all external service dependencies
   - Document required API credentials
   - Note any special node types that require specific n8n versions

### Complete Validation Scenario
After making any changes to workflow files, ALWAYS run this complete validation:

```bash
echo "🔍 Starting Complete Workflow Validation..."

# Step 1: JSON Structure Validation
echo "=== Step 1: JSON Structure Validation ==="
find n8n -name "*.json" -exec jq . {} \; > /dev/null && echo "✅ All JSON files valid" || echo "❌ JSON validation failed"

# Step 2: Workflow Content Analysis  
echo -e "\n=== Step 2: Workflow Analysis ==="
for file in n8n/*.json n8n/*/*.json; do
  if [ -f "$file" ]; then
    echo "--- $(basename "$file") ---"
    echo "Name: $(jq -r '.name' "$file")"
    echo "Nodes: $(jq '.nodes | length' "$file")"
    echo "Active: $(jq -r '.active // "not set"' "$file")"
    credential_count=$(jq '[.nodes[] | select(.credentials)] | length' "$file")
    if [ "$credential_count" -gt 0 ]; then
      echo "Credentials required: $credential_count"
    fi
    # Check for broken connections
    connection_errors=$(jq '.connections | to_entries[] | select(.value.main == null or (.value.main | length == 0)) | .key' "$file" 2>/dev/null | wc -l)
    if [ "$connection_errors" -gt 0 ]; then
      echo "⚠️  Potential connection issues detected"
    fi
    echo
  fi
done

# Step 3: Credential Requirements Audit
echo "=== Step 3: Credential Requirements ==="
for file in n8n/*.json n8n/*/*.json; do
  if [ -f "$file" ]; then
    cred_nodes=$(jq -r '.nodes[] | select(.credentials) | .name + ": " + ((.credentials | keys) | join(", "))' "$file")
    if [ -n "$cred_nodes" ]; then
      echo "$(basename "$file"):"
      echo "$cred_nodes" | sed 's/^/  /'
    fi
  fi
done

echo -e "\n✅ Validation Complete!"
```

### Timeout Specifications
- JSON validation: 5-10 seconds per file
- n8n installation (if attempted): 3-5 minutes. **NEVER CANCEL** - network timeouts are expected
- Workflow import: 10-15 seconds. **NEVER CANCEL** - wait for completion

### Expected Limitations
- **Network restrictions may prevent n8n installation** - document this as expected behavior
- **Workflow execution requires live credentials** - not possible to test end-to-end functionality
- **UI interactions require browser access** - not available in headless environments

## Repository Contents

### AI Newsletter Workflow (`n8n/ai-agent-newsletter.json`)
- **Purpose**: Automated AI-powered email newsletter generation
- **Nodes**: 8 total (including 2 AI/LangChain nodes)
- **Required Credentials**:
  - Gmail OAuth2 for email sending
  - Azure OpenAI API for content generation
- **Schedule**: Configurable trigger (default daily)
- **Key Features**: 
  - Uses Azure OpenAI Chat Model for content generation
  - Custom JavaScript formatting for HTML emails
  - Automated scheduling via trigger node

### RAG Workflow (`n8n/rag/crawl4Ai-rag.json`)
- **Purpose**: Web crawling with RAG capabilities for property data
- **Nodes**: 30 total (including 11 AI/LangChain nodes)
- **Required Credentials**:
  - Supabase API for vector storage
  - Cohere API for embeddings and reranking
  - PostgreSQL for data storage and chat memory
  - OpenRouter API for language model access
  - HTTP Header Auth for web crawling
- **Key Features**:
  - XML sitemap parsing and URL extraction
  - Vector embeddings with Cohere
  - PostgreSQL integration for structured data
  - Chat interface with memory
  - Batch processing capabilities

## Common Tasks

### Repository Root Exploration
```bash
ls -la
# Expected output:
# .
# ..
# .git/
# .github/
# LICENSE
# README.md
# n8n/
```

### Workflow Inventory
```bash
find n8n -name "*.json" -type f
# Expected output:
# n8n/ai-agent-newsletter.json
# n8n/rag/crawl4Ai-rag.json
```

### Quick Workflow Summary
```bash
# Get workflow names and node counts
for file in n8n/*.json n8n/*/*.json; do
  if [ -f "$file" ]; then
    echo "=== $(basename "$file") ==="
    jq -r '"Name: " + .name + " | Nodes: " + (.nodes | length | tostring)' "$file"
  fi
done
```

### Credential Audit
```bash
# List all required credentials across workflows
for file in n8n/*.json n8n/*/*.json; do
  if [ -f "$file" ]; then
    echo "=== $(basename "$file") Credentials ==="
    jq -r '.nodes[] | select(.credentials) | .name + ": " + ((.credentials | keys) | join(", "))' "$file"
  fi
done
```

## Important Notes

### Workflow Modification Guidelines
- **ALWAYS backup original JSON before modifications**: `cp n8n/ai-agent-newsletter.json n8n/ai-agent-newsletter.json.backup`
- **ALWAYS validate JSON after changes**: Use jq validation commands above
- **Test node connections**: Verify that workflow connections array matches node modifications
- **Preserve credential references**: Do not modify credential IDs unless intentionally updating

### Development Workflow
1. Make changes to workflow JSON files
2. Run JSON validation to ensure syntax correctness
3. Use jq to verify workflow structure integrity
4. Document any new credential requirements
5. Update this instruction file if new dependencies are added

### Debugging Common Issues
- **JSON Syntax Errors**: Use `jq . filename.json` to locate malformed JSON
- **Missing Nodes**: Check that all referenced node IDs exist in the nodes array
- **Broken Connections**: Verify connections array matches actual node IDs
- **Credential Issues**: Ensure credential objects have proper structure and valid IDs

Remember: This repository contains workflow configurations, not executable code. Focus on JSON structure integrity and workflow logic validation rather than traditional software testing approaches.