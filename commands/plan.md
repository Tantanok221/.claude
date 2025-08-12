# Plan Command

Load issue context, analyze implementation requirements, and create a comprehensive todo list for session-based development workflow.

## Usage
```
/plan <ticket-id> [--lite]
/plan <feature-description> [--lite]
```

## Instructions
You are helping Claude Code understand a specific issue by loading the issue document and relevant steering context. This command provides comprehensive context for issue analysis and implementation planning.

**ULTRATHINK MODE ACTIVATED**: This is critical issue analysis that requires deep understanding. Use maximum tokens to thoroughly analyze the issue, identify all relevant steering documents, and provide comprehensive context. Think deeply about implementation approaches, edge cases, and how this issue fits into the broader system architecture.

**--lite Mode Instructions**: When `--lite` is used, streamline the planning process by making quick, reasonable defaults. The AI will still perform comprehensive analysis and generate a detailed todo list internally, but it will present a more concise summary and automatically proceed with file creation without explicit user confirmation. Focus on generating a usable plan efficiently.

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

### 2. **Identify Work Target**
   **If ticket-id provided:**
   - Look for the issue file: `issues/{ticket-id}-issue.md`
   - If exact match not found, search for similar ticket IDs in issues directory
   - If multiple matches, present options to user
   - **[--lite Mode Adjustment]**: If multiple matches, automatically select the top 1 most relevant issue without user interaction.
   
   **If feature-description provided:**
   - Search all issue documents in `issues/` directory for relevant content
   - Match against issue titles, descriptions, and tags
   - Present top 3 most relevant issues to user
   - Ask user to select which issue to work on, or offer to create new issue
   - If no relevant issues found, suggest creating new issue with `/create-issue`
   - **[--lite Mode Adjustment]**: If multiple relevant issues are found, automatically select the top 1 most relevant issue without asking the user to select. If no relevant issues are found, still suggest `/create-issue`.

### 2. **Load and Analyze Issue Document**
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

### 2. **Determine Relevant Steering Documents**
   Based on issue analysis, intelligently select relevant steering documents:
   
   **Architecture Documents (select relevant):**
   - `steering/architecture/system-overview.md` - For system-wide changes
   - `steering/architecture/data-flow.md` - For data-related issues
   - `steering/architecture/integration-patterns.md` - For external integration work
   - `steering/architecture/security-architecture.md` - For security-related issues
   
   **Standards Documents (select relevant):**
   - `steering/standards/coding-standards.md` - For all development work
   - `steering/standards/api-design-guidelines.md` - For API changes
   - `steering/standards/database-conventions.md` - For database work
   - `steering/standards/testing-standards.md` - For testing requirements
   
   **Pattern Documents (select relevant):**
   - `steering/patterns/common-patterns.md` - For implementation guidance
   - `steering/patterns/anti-patterns.md` - For avoiding known issues
   - `steering/patterns/decision-trees.md` - For implementation choices
   
   **Context Documents (select relevant):**
   - `steering/context/business-domain.md` - For business logic issues
   - `steering/context/technical-constraints.md` - For constraint awareness
   - `steering/context/stakeholder-map.md` - For stakeholder impact

### 3. **Smart Document Selection Logic**
   **Only load steering documents that are directly relevant:**
   
   ```
   IF issue involves API changes → load api-design-guidelines.md
   IF issue involves database → load database-conventions.md + data-flow.md
   IF issue involves frontend → load coding-standards.md + common-patterns.md
   IF issue involves security → load security-architecture.md + technical-constraints.md
   IF issue involves integrations → load integration-patterns.md + system-overview.md
   IF issue involves business logic → load business-domain.md + decision-trees.md
   ```
   
   **Always load:**
   - `steering/standards/coding-standards.md` (universal)
   - `steering/patterns/anti-patterns.md` (prevent mistakes)

### 4. **Load Selected Steering Documents**
   - Read only the identified relevant steering documents
   - Parse each document for context relevant to the issue
   - Note any conflicts or special considerations
   - Identify patterns and standards that apply to this specific issue

### 5. **Comprehensive Context Analysis**
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

### 6. **Present Complete Todo List Content & Get Confirmation**
   **CRITICAL: DO NOT CREATE ANY FILES until user confirms the plan**
   
   **Show Complete Todo List File Content:**
   ```markdown
   I'm ready to create the following todo list file:
   
   **File: `work-progress/{ticket-id}.md`**
   
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
   
   **Present Plan Summary:**
   ```markdown
   📋 Implementation plan and todo list ready!
   
   **Plan Summary:**
   - {X} implementation tasks across {Y} phases
   - Files to create/modify: {Z} files total
   - Follows {steering-patterns} patterns and guidelines
   - Estimated complexity: {overall-complexity}
   
   ❓ Do you want me to create this todo list file at `work-progress/{ticket-id}.md`?
   
   Please review the complete content above and let me know:
   
   Options:
   - "yes" or "create" - Create the todo list file exactly as shown
   - "modify task X" - Change specific tasks (e.g., "modify task 3 to include error handling")
   - "add phase" - Add additional implementation phases
   - "change complexity" - Adjust complexity estimates
   - "abort" - Cancel planning session
   ```
   
   **WAIT FOR USER RESPONSE - DO NOT CREATE FILES WITHOUT EXPLICIT CONFIRMATION**
   
   **[--lite Mode Adjustment]**: In `--lite` mode, the "Present Plan Summary" will be significantly more concise, for example:
   ```markdown
   📋 Plan for {ticket-id} generated.
   
   **Plan Summary:**
   - {X} tasks across {Y} phases.
   - Estimated complexity: {overall-complexity}.
   
   Automatically creating `work-progress/{ticket-id}.md`.
   ```
   The explicit confirmation "❓ Do you want me to create this todo list file..." will be skipped, and the file creation will proceed automatically.

### 7. **Create Todo List File (Only After Confirmation)**
   **After user confirmation, create the external todo list:**
   
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
   - Add `work-progress/` to `.git/info/exclude`
   - Ensure steering/ is also in `.git/info/exclude` (from steering setup)
   - Keep all planning documents local and out of version control
   - Ensure clean git history

### 8. **Planning Output**
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

### 9. **Optimization Notes**
   - **Context Efficiency**: Only load steering documents directly relevant to the issue
   - **Prevent Context Rot**: Avoid loading unnecessary documentation
   - **Smart Selection**: Use issue content to determine relevance
   - **Comprehensive Coverage**: Ensure all relevant guidance is included

## Important Notes

- **PLANNING ONLY**: This command creates implementation plans and todo lists - NO actual implementation
- **SESSION PREPARATION**: Prepares for session-based development workflow with `/continue` command
- **EXTERNAL TODO LIST**: Creates persistent state file that survives across development sessions
- **STEERING REFERENCES**: Todo list contains steering document references, not duplicate content
- **USER CONFIRMATION REQUIRED**: Never create todo list files without explicit user approval (unless `--lite` mode is active)
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
- Ask user to clarify which issue they mean (unless `--lite` mode is active, then auto-select top 1)

## Example Usage

```bash
/plan fix-user-login
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/other-work → Wrong branch!
# 3. Switch to main, fetch/pull latest, create pg/tk-fix-user-login
# 4. Load: issues/fix-user-login-issue.md
# 5. Load relevant steering docs for analysis
# 6. Create comprehensive todo list with 8 tasks across 3 phases
# 7. Present plan summary to user
# 8. User approves → Create work-progress/fix-user-login.md
# 9. Ready for: /continue fix-user-login

/plan "add payment processing" --lite
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Search issues for payment-related work. Auto-selects top match (e.g., add-stripe-integration-issue.md).
# 3. Create feat/add-payment-processing branch
# 4. Load issue + relevant steering documents (internally)
# 5. Presents concise plan summary.
# 6. Automatically creates work-progress/add-stripe-integration.md
# 7. Ready for: /continue add-stripe-integration
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
