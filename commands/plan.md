# Plan Command

Load issue context, analyze implementation requirements, and create a comprehensive todo list for session-based development workflow.

## Usage
```
/plan <ticket-id>
/plan <feature-description>
/plan
```

**Note**: When no parameters are provided, the command will use cached session data from previous work in this project, or prompt you to specify what to work on.

## Instructions
You are helping Claude Code understand a specific issue by loading the issue document and relevant steering context. This command provides comprehensive context for issue analysis and implementation planning.

**ULTRATHINK MODE ACTIVATED**: This is critical issue analysis that requires deep understanding. Use maximum tokens to thoroughly analyze the issue, identify all relevant steering documents, and provide comprehensive context. Think deeply about implementation approaches, edge cases, and how this issue fits into the broader system architecture.


## Process

### 1. **Git Branch Management**
   **Check current git status and manage branches:**
   
   **Determine Master Branch:**
   - Check git origin URL: `git remote get-url origin`
   - If origin contains "gitlab" → master branch is "master"
   - If origin contains "github" → master branch is "main"
   - Default to "main" if unable to determine
   
   **Expected Branch Naming:**
   - For ticket-id: `pg/tk-{ticket-id}` (e.g., `pg/tk-fix-user-login`)
   - For feature: `feat/{kebab-case-feature}` (e.g., `feat/add-dark-mode`)
   
   **Branch Validation Process:**
   ```bash
   # Check current branch
   current_branch=$(git branch --show-current)
   
   # Determine expected branch name
   if [ticket-id provided]:
     expected_branch="pg/tk-{ticket-id}"
   else:
     expected_branch="feat/{kebab-case-feature-name}"
   
   # Check if on correct branch
   if current_branch == expected_branch:
     echo "✓ On correct branch: ${current_branch}"
     proceed_to_planning()
   else:
     echo "✗ Wrong branch. Current: ${current_branch}, Expected: ${expected_branch}"
     switch_to_correct_branch()
   ```
   
   **Branch Switching Process (if needed):**
   ```bash
   # Switch to master and update
   git checkout {master_branch}  # master or main
   git fetch origin
   git pull origin {master_branch}
   
   # Create and checkout new branch
   git checkout -b {expected_branch}
   echo "✓ Created and switched to: ${expected_branch}"
   ```

### 2. **Session Cache & Work Target Identification**
   **Handle session caching and work target identification:**
   
   **Session Cache Template:**
   ```json
   {
     "lastTicketId": "AUTH-123",
     "lastFeatureDescription": "add OAuth login support", 
     "timestamp": "2025-01-15T10:30:00Z",
     "lastCommand": "plan"
   }
   ```
   
   **Session Cache Logic:**
   - If ticket-id or feature-description provided: Use the provided parameters
   - If no parameters provided: Check for `.claude-session-cache.json` in project root
   - If cache exists and valid: Use cached ticket ID or feature description, show "✓ Using recent work: {value}"
   - If no cache or cache invalid: Prompt user with: "No recent work found. Please provide either:
     - Ticket ID (e.g., AUTH-123)  
     - Feature description (e.g., add OAuth support)"
   - If user provides nothing: Exit with "No work specified"
   
   **Work Target Identification:**
   
   **If ticket-id obtained:**
   - Look for the issue file: `issues/{ticket-id}-issue.md`
   - If exact match not found, search for similar ticket IDs in issues directory
   - If multiple matches, present options to user
   
   **If feature-description obtained:**
   - Search all issue documents in `issues/` directory for relevant content
   - Match against issue titles, descriptions, and tags
   - Present top 3 most relevant issues to user
   - Ask user to select which issue to work on, or offer to create new issue
   - If no relevant issues found, suggest creating new issue with `/create-issue`

### 3. **Load and Analyze Issue Document**
   - Look for the issue file: `issues/{ticket-id}-issue.md`
   - If file doesn't exist, inform user and suggest:
     - Check if ticket ID is correct
     - List available issues in the directory
     - Offer to search for similar ticket IDs
   - **Deep Issue Analysis**:
     - Parse issue type (bug/enhancement/feature)
     - Identify affected components and systems
     - Understand business context and requirements
     - Note technical constraints and dependencies
     - Extract acceptance criteria and success metrics
     - Identify potential complexity areas and edge cases
     - **Extract keywords for steering document mapping** (authentication, database, ui, api, testing, deployment, performance, security, etc.)

### 4. **Smart Document Selection Logic**
   **Read steering configuration and apply mappings dynamically:**
   
   **Load Steering Configuration:**
   - Read `steering/config.md` for always-include docs and feature mappings
   - If config.md doesn't exist, use fallback default behavior
   
   **Apply Config-Based Mappings:**
   ```
   # Read steering/config.md and extract:
   # 1. Always Include Steering Docs (loaded for every plan)
   # 2. Feature Pattern Mappings (loaded based on issue content analysis)
   
   Example config.md structure:
   ## Always Include Steering Docs
   - coding-standards.md
   - anti-patterns.md
   - system-overview.md
   
   ## Feature Pattern Mappings
   - **authentication** → security-architecture.md, api-design-guidelines.md
   - **database** → database-conventions.md, data-flow.md
   - **ui** → ui-library-guidelines.md, coding-standards.md
   - **api** → api-design-guidelines.md, integration-patterns.md
   - **testing** → testing-standards.md, common-patterns.md
   - **deployment** → system-overview.md, technical-constraints.md
   - **performance** → technical-constraints.md, common-patterns.md
   - **security** → security-architecture.md, technical-constraints.md
   ```
   
   **Issue Analysis & Mapping Application:**
   - Analyze issue content for keywords (authentication, database, ui, api, etc.)
   - Match keywords against config.md feature mappings
   - Load corresponding steering documents from mappings
   - Always load the "Always Include" documents from config
   
   **Fallback Behavior (if config.md missing):**
   - Always load: coding-standards.md, anti-patterns.md
   - Use basic keyword matching for common patterns

### 5. **Load Selected Steering Documents**
   **Based on config.md mappings and issue analysis:**
   
   - Read the identified steering documents from config.md mappings
   - Always load the "Always Include Steering Docs" from config.md
   - Load feature-specific documents based on keyword matches
   - Parse each document for context relevant to the issue
   - Note any conflicts or special considerations
   - Identify patterns and standards that apply to this specific issue
   
   **Example Document Loading Process:**
   ```
   Issue analysis finds: "authentication", "api", "database" keywords
   
   From config.md mappings:
   - Always include: coding-standards.md, anti-patterns.md, system-overview.md
   - authentication → security-architecture.md, api-design-guidelines.md
   - api → api-design-guidelines.md, integration-patterns.md  
   - database → database-conventions.md, data-flow.md
   
   Final document list:
   ✓ coding-standards.md (always include)
   ✓ anti-patterns.md (always include)
   ✓ system-overview.md (always include)
   ✓ security-architecture.md (from authentication mapping)
   ✓ api-design-guidelines.md (from authentication + api mappings)
   ✓ integration-patterns.md (from api mapping)
   ✓ database-conventions.md (from database mapping)
   ✓ data-flow.md (from database mapping)
   ```

### 6. **Comprehensive Context Analysis**
   **Synthesize issue + steering context:**
   
   **Issue Understanding:**
   - What needs to be implemented/fixed?
   - Why is this important to the business?
   - What are the technical requirements?
   - What are the acceptance criteria?
   
   **Implementation Guidance:**
   - Which architectural patterns apply?
   - What coding standards must be followed?
   - Are there specific patterns to use or avoid?
   - What testing approaches are required?
   
   **Constraints and Considerations:**
   - Business constraints that affect implementation
   - Technical limitations to consider
   - Performance or security requirements
   - Integration points that may be affected
   
   **Risk Assessment:**
   - Potential implementation challenges
   - Areas that need special attention
   - Dependencies that could cause issues
   - Testing strategies needed

### 7. **Present Todo List for Review & Iteration**
   **CRITICAL: DO NOT CREATE ANY FILES - This is an iterative planning phase**
   
   **Show Complete Todo List Content for Review:**
   ```markdown
   📋 Here's the proposed todo list for review:
   
   **Planned File: `work-progress/{ticket-id}.md`**
   
   ---
   
   # Work Progress: {ticket-id}
   
   ## Issue Reference
   - Issue: issues/{ticket-id}-issue.md
   - Type: {bug/feature/enhancement}
   - Priority: {priority-level}
   - Created: {timestamp}
   
   ## Current Status
   - Status: planned
   - Current Task: #1 (ready to start)
   - Last Updated: {timestamp}
   - Sessions: 0
   
   ## Todo List
   ### Phase 1: Setup & Preparation
   - [ ] #1 🟡 {task-description}
     - Files: {file-list}
     - Steering: {relevant-steering-docs}
     - Complexity: low/medium/high
     - Dependencies: none/#{task-numbers}
   
   ### Phase 2: Core Implementation  
   - [ ] #2 🟠 {task-description}
     - Files: {file-list}
     - Steering: {relevant-steering-docs}
     - Complexity: low/medium/high
     - Dependencies: #{previous-tasks}
   
   ### Phase 3: Integration & Testing
   - [ ] #3 🔴 {task-description}
     - Files: {file-list}
     - Steering: {relevant-steering-docs}
     - Complexity: low/medium/high
     - Dependencies: #{previous-tasks}
   
   ## Implementation Guidelines
   ### Architecture Considerations
   - {specific-patterns-from-steering-docs}
   - {integration-points-and-constraints}
   - {performance-requirements}
   
   ### Coding Standards
   - {style-guide-requirements}
   - {naming-conventions}
   - {testing-approaches}
   
   ### Business Context
   - {domain-rules-from-steering}
   - {stakeholder-requirements}
   - {success-metrics}
   
   ## Session Notes
   *This section will be updated by /continue sessions*
   
   ## Next Session Context
   **For /continue command:**
   - Current task: #1 {first-task-description}
   - Load steering: {first-task-steering-docs}
   - Focus: {first-task-focus-area}
   - Files to modify: {first-task-files}
   
   ---
   ```
   
   **Plan Summary & Review Options:**
   ```markdown
   📋 Todo list ready for review:
   
   **Plan Summary:**
   - {X} implementation tasks across {Y} phases
   - Files to create/modify: {Z} files total
   - Follows {steering-patterns} patterns and guidelines
   - Estimated complexity: {overall-complexity}
   
   **Review & Modify Options:**
   - "modify task X to [description]" - Change specific tasks
   - "add task [description] to phase Y" - Add new tasks
   - "remove task X" - Remove specific tasks
   - "add phase [name]" - Add new implementation phase
   - "change complexity of task X to [low/medium/high]" - Adjust estimates
   - "move task X to phase Y" - Reorganize task phases
   - "change dependencies of task X to [deps]" - Adjust task dependencies
   
   **When satisfied with the todo list:**
   - "create file" or "save the todo list" - Create the work-progress/{ticket-id}.md file
   - "abort" - Cancel planning session without creating file
   
   ⚠️ **Note: I will NOT create any files until you explicitly tell me to "create file" or "save the todo list"**
   ```
   
   **ENTER ITERATIVE PLANNING MODE - WAIT FOR USER MODIFICATIONS OR CREATION COMMAND**
   

### 8. **Iterative Planning Mode**
   **Handle user modifications to the todo list:**
   
   **For each user request to modify the todo list:**
   - Apply the requested changes to the todo list structure
   - Update task numbers, dependencies, and phases as needed
   - Re-present the complete updated todo list for review
   - Continue iterating until user is satisfied
   - **DO NOT CREATE FILES** during this iterative process
   
   **Common modification patterns:**
   ```markdown
   User: "modify task 3 to include database migration setup"
   → Update task 3 description and add migration-related files/steering docs
   → Show updated complete todo list
   → Wait for more modifications or creation command
   
   User: "add task 'write integration tests' to phase 3"
   → Add new task to Phase 3 with appropriate complexity and dependencies
   → Renumber subsequent tasks if needed
   → Show updated complete todo list
   → Wait for more modifications or creation command
   
   User: "remove task 2"
   → Remove task 2 and update dependencies of other tasks
   → Renumber tasks appropriately
   → Show updated complete todo list
   → Wait for more modifications or creation command
   ```

### 9. **Create Todo List File (Only After Explicit Command)**
   **Only when user says "create file" or "save the todo list":**
   
   **File Creation Process:**
   - Create directory: `work-progress/` if it doesn't exist
   - Create file: `work-progress/{ticket-id}.md`
   - Include all todo list content from step 6
   - Add current timestamp and session information
   - Set status to "planned" and current task to #1
   
   **Todo List File Features:**
   - **Task Dependencies** - Clear dependency chain between tasks
   - **Steering References** - Which steering docs each task needs
   - **Complexity Indicators** - Visual complexity markers (🟡🟠🔴)
   - **Session State** - Tracks progress across development sessions
   - **Context Hints** - Enough info for `/continue` to load relevant context
   
   **Git Exclusion:**
   Ask the user if they want to add the `work-progress/` directory to `.git/info/exclude`:
   ```
   Do you want to add the `work-progress/` directory to `.git/info/exclude` to keep 
   todo lists local and out of version control?
   
   - "yes" - Add work-progress/ to .git/info/exclude for local-only exclusions
   - "no" - Skip git exclusion (todo lists may be committed)
   ```
   
   **If user says "yes":**
   - Add `work-progress/` to `.git/info/exclude`
   - Ensure steering/ is also in `.git/info/exclude` (from steering setup)
   - Keep all planning documents local and out of version control
   - Ensure clean git history
   
   **If user says "no":**
   - Continue without adding git exclusion
   - Inform user that todo lists may be committed to the repository

### 10. **Planning Output**
   **Provide planning completion summary:**
   
   ```markdown
   # Planning Complete: {ticket-id}
   
   ## Todo List Created
   - File: work-progress/{ticket-id}.md
   - Tasks: {total-tasks} across {phases} phases
   - Status: Ready for development
   
   ## Next Steps
   - Start development: `/continue {ticket-id}`
   - Review plan: Check work-progress/{ticket-id}.md
   - Modify plan: Edit todo list file manually
   
   ## Development Workflow
   1. New session: `/continue {ticket-id}`
   2. Work on current task
   3. Todo list automatically updates
   4. Repeat until all tasks complete
   
   ## Session Benefits
   - **Zero context rot** - Fresh context each session
   - **Perfect resumability** - Pick up exactly where left off
   - **Progress tracking** - Visual progress in todo list
   - **Steering integration** - Relevant docs loaded per task
   ```

### 11. **Update Session Cache & Optimization Notes**
   
   **Session Cache Update (After Successful Planning):**
   - Write the current ticket ID and feature description to `.claude-session-cache.json` using the unified template
   - Include timestamp and command name ("plan") in the cache
   - Add cache file to `.git/info/exclude` to keep it local
   - Show "✓ Session cache updated for future /plan and /continue commands"
   
   **Optimization Notes:**
   - **Context Efficiency**: Only load steering documents directly relevant to the issue
   - **Prevent Context Rot**: Avoid loading unnecessary documentation  
   - **Smart Selection**: Use issue content to determine relevance
   - **Comprehensive Coverage**: Ensure all relevant guidance is included
   - **Session Continuity**: Cache successful planning for seamless workflow resumption

## Important Notes

- **PLANNING ONLY**: This command creates implementation plans and todo lists - NO actual implementation
- **SESSION PREPARATION**: Prepares for session-based development workflow with `/continue` command
- **EXTERNAL TODO LIST**: Creates persistent state file that survives across development sessions
- **STEERING REFERENCES**: Todo list contains steering document references, not duplicate content
- **USER CONFIRMATION REQUIRED**: Never create todo list files without explicit user approval  
- **CONTEXT EFFICIENCY**: Todo list designed for minimal context loading in subsequent sessions
- **RESUMABLE WORKFLOW**: Perfect for enterprise development that spans multiple sessions/days

## Error Handling

**If issue file not found:**
- List all available issues in `issues/` directory
- Suggest closest matches based on ticket ID similarity
- Offer to search issue content for keywords

**If steering documents missing:**
- Identify which steering documents are missing
- Suggest running `/steering-setup` if no steering documents exist
- Continue with available steering context

**If ticket ID ambiguous:**
- Show similar ticket IDs
- Ask user to clarify which issue they mean

## Example Usage

```bash
/plan fix-user-login
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/other-work → Wrong branch!
# 3. Switch to main, fetch/pull latest, create pg/tk-fix-user-login
# 4. Load: issues/fix-user-login-issue.md
# 5. Load relevant steering docs for analysis
# 6. Present todo list with 8 tasks across 3 phases for review
# 7. User: "modify task 5 to include error handling"
# 8. Show updated todo list with modified task 5
# 9. User: "add task for user notification after login success"
# 10. Show updated todo list with new task added
# 11. User: "create file"
# 12. Create work-progress/fix-user-login.md
# 13. Ready for: /continue fix-user-login

/plan "add payment processing"
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Search issues for payment-related work and present top matches for user selection
# 3. Create feat/add-payment-processing branch
# 4. Load issue + relevant steering documents
# 5. Present todo list for review with payment integration tasks
# 6. User iterates on the plan: "add security validation tasks", "modify error handling"
# 7. User: "save the todo list"
# 8. Create work-progress/add-stripe-integration.md
# 9. Ready for: /continue add-stripe-integration
```

## What Happens After This Command?

**Planning Complete:**
- **Todo list created** - External file with comprehensive task breakdown
- **Branch prepared** - Correct git branch created from latest master/main
- **Steering analyzed** - Relevant patterns and standards identified
- **Dependencies mapped** - Task order and relationships established

**Ready for Development:**
- **Clear your context** - Use `/clear` to start with fresh context
- **Start implementing** - Use `/continue {ticket-id}` to begin work
- **Track progress** - Todo list shows completion status as you work
- **Resume anytime** - Can pause and come back to work later
