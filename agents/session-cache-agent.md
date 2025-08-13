---
name: session-cache-agent
description: Specialized agent for managing session cache operations to debloat custom slash commands
tools: Glob, Grep, LS, Read, Edit, MultiEdit, Write, NotebookEdit, WebFetch, TodoWrite, WebSearch, BashOutput, KillBash
model: haiku
color: yellow
---

# Session Cache Agent

## Purpose
Handle all session cache (`.claude-session-cache.json`) operations for custom slash commands. This agent centralizes session cache logic to debloat the main command workflows.

## Scope
**IMPORTANT**: This agent handles ONLY session cache operations. Context cache operations (`work-progress/{ticket-id}-context.json`) are handled separately by the main commands.

## Session Cache Template
```json
{
  "lastTicketId": "AUTH-123",
  "lastFeatureDescription": "add OAuth login support", 
  "timestamp": "2025-01-15T10:30:00Z",
  "lastCommand": "continue"
}
```

## Core Operations

### 1. Read Session Cache
**Input**: Request to read current session cache
**Process**:
- Look for `.claude-session-cache.json` in project root
- If found: Parse and validate JSON structure
- If not found: Return "no cache found"
- If invalid: Return "invalid cache"

**Output**: Session cache data or status message

### 2. Write Session Cache
**Input**: 
- `ticketId` (string, optional)
- `featureDescription` (string, optional) 
- `command` (string: "continue", "plan", "design")

**Process**:
- Create session cache object with provided data
- Add current timestamp
- Write to `.claude-session-cache.json` in project root
- Ensure file is added to `.git/info/exclude`

**Output**: Confirmation of cache update

### 3. Validate Session Cache
**Input**: Session cache data
**Process**:
- Check JSON structure matches template
- Validate timestamp format
- Verify command is valid ("continue", "plan", "design")
- Check that either ticketId or featureDescription exists

**Output**: Validation result (valid/invalid with details)

### 4. Session Cache Logic (Combined Operation)
**Input**:
- `providedTicketId` (optional)
- `providedFeatureDescription` (optional)
- `currentCommand` (string)

**Process**:
```
IF ticketId or featureDescription provided:
  Use provided parameters
ELSE:
  Check for .claude-session-cache.json in project root
  IF cache exists and valid:
    Use cached values
    Show "✓ Using recent work: {value}"
  ELSE:
    Return prompt for user input
```

**Output**: Work target (ticketId/featureDescription) or prompt for user input

## Git Exclusion Management
Ensure `.claude-session-cache.json` is added to `.git/info/exclude` to keep it local and out of version control.

## Error Handling
- **Missing cache file**: Return clear status, don't error
- **Corrupted JSON**: Return "invalid cache" with error details
- **Missing git exclude**: Add file to exclusion automatically
- **File permission issues**: Return clear error message

## Interface for Main Commands
Commands should call this agent with context like:
```
"I need to handle session cache for the {command} command. 
Parameters provided: ticketId='{value}' featureDescription='{value}'. 
Please check existing cache and determine the work target, or update the cache with new values."
```

## Agent Response Format
```markdown
## Session Cache Operation Result

**Operation**: {read/write/validate}
**Status**: {success/no_cache/invalid_cache/error}
**Work Target**: {ticketId or featureDescription if determined}
**Message**: {user-facing message like "✓ Using recent work: AUTH-123"}
**Action Required**: {user_input_needed/none}

## Details
{Additional context or error details if applicable}
```