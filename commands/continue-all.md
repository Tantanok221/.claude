# Continue All Command

Execute ALL remaining tasks from todo list in one comprehensive session with focused context.

## Usage
```
/continue-all <ticket-id>
/continue-all <feature-description>
/continue-all
```

**Note**: When no parameters are provided, the command will use cached session data from previous work in this project, or prompt you to specify what to work on.

## Instructions
Load the current todo list and implement ALL remaining tasks using referenced steering documents. This command focuses on executing all tasks at once with comprehensive implementation.

**ULTRATHINK MODE ACTIVATED**: Focus deeply on complete implementation with highest quality. Use maximum tokens for precise execution and maintain architectural consistency across all tasks. Leverage Gemini CLI MCP (Model Context Protocol) capabilities for deep codebase analysis and understanding when needed for comprehensive task execution.

## Process

### 1. **Git Branch Management**
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the correct Git branch is active for the tasks, handling all branch checking and switching operations.
   
   **Context for Agent**: The expected branch pattern will be derived from the `ticket-id` or `feature-description` obtained in Step 2 (Session Cache & Todo List Loading). The agent will determine the appropriate branch name and handle switching if needed.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Session Cache & Todo List Loading**
   **Handle session caching and todo list loading:**
   
   **Session Cache Logic:**
   - If ticket-id or feature-description provided: Use the provided parameters
   - If no parameters provided: **Use Session Cache Agent** to check for cached session data
     - **Context for Agent**: "I need to check session cache for the continue-all command. No parameters were provided. Please check for existing `.claude-session-cache.json` and return the cached work target or indicate if user input is needed."
     - **Agent Execution**: Wait for the session-cache-agent to return cached values or prompt for user input
   - If user provides nothing: Exit with "No work specified"
   
   **Todo List Loading:**
   
   **If ticket-id obtained:**
   - Look for `work-progress/{ticket-id}.md`
   - If not found, search for similar ticket IDs in work-progress directory
   
   **If feature-description obtained:**
   - Search all todo lists in `work-progress/` directory for relevant content
   - Present options to user if multiple matches found
   - If no matches, suggest running `/create-issue` then `/plan`
   
   **Internal Note**: Once the todo list is loaded, the `ticket-id` from its filename (or the derived `feature-id`) will be used by the "Git Branch Management" step (Step 1) to determine the `expected_branch` name for validation.

### 3. **Analyze All Remaining Tasks**
   - Read todo list and identify ALL incomplete tasks
   - Extract comprehensive task details: files, steering docs, complexity, dependencies
   - Verify dependency chains and execution order
   - **CRITICAL**: Ensure all dependencies are satisfied before execution

### 4. **Load Comprehensive Context**
   **Load steering configuration, task-specific context, and session learnings for ALL tasks:**
   
   **Session Context Cache Template:**
   ```json
   {
     "issueId": "AUTH-123",
     "sessionLearnings": {
       "codebaseInsights": "Uses Express.js with custom auth middleware in middleware/auth.js",
       "keyFiles": ["middleware/auth.js", "services/AuthService.js", "routes/auth.js"],
       "dependencies": ["passport", "jsonwebtoken", "bcrypt"],
       "businessContext": "OAuth integration must work with existing JWT system",
       "technicalConstraints": "Rate limiting applied at middleware level",
       "architecturalPatterns": "Repository pattern in services/, middleware for auth",
       "libraryDocumentationInsights": "JWT best practices: use RS256 for production, short expiry for access tokens. Passport OAuth2 requires callback URL configuration.",
       "integrationPatterns": "OAuth2 flow: authorization code grant with PKCE, store refresh tokens securely"
     },
     "lastUpdated": "2025-01-15T10:30:00Z"
   }
   ```
   
   **Context Loading Process:**
   - Check for session context cache: `work-progress/{ticket-id}-context.json`
   - If context cache exists: Load session learnings and show "✓ Loaded session context - resuming with previous discoveries"
   - If no context cache: Continue with standard context loading
   
   **Use Steering Context Reader Agent**: Use the steering-context-reader agent to gather relevant steering documentation for ALL tasks.
   
   **Context for Agent**: "I'm working on ALL remaining tasks for {ticket-id}. The tasks involve files: {comprehensive-file-list} and reference steering docs: {comprehensive-steering-doc-list}. I need comprehensive context from the steering directory to implement all tasks according to project standards and patterns. Focus on {all-task-focus-areas}."
   
   **Agent Execution**: Wait for the steering-context-reader agent to return comprehensive steering context for all tasks. The agent will read steering/config.md, identify relevant documents, and return filtered context to prevent context bloat.
   
   **Library Documentation Context via Tech Doc Agent:**
   **Use tech-doc-agent when tasks involve libraries, frameworks, or integrations:**
   
   **Detection Keywords in All Task Descriptions:**
   - Library/framework names: "React", "Express", "Node.js", "JWT", "OAuth", "Stripe", etc.
   - Migration tasks: "migrate", "upgrade", "update version", "replace with"
   - Integration tasks: "integrate with", "connect to", "API integration", "SDK"
   - Implementation patterns: "authentication", "payment", "database", "caching"
   
   **Tech Doc Agent Integration Process:**
   ```
   IF any task contains library/framework keywords:
   1. Use tech-doc-agent with comprehensive task context
   2. Pass comprehensive context about all implementation tasks
   3. Receive focused documentation analysis for all tasks
   4. Integrate findings into implementation approach
   ```
   
   **Context to Provide to Tech Doc Agent:**
   ```
   "I'm implementing ALL remaining tasks for {ticket-id}: {comprehensive-task-descriptions}.
   
   **Problem Context**: {brief description of what needs to be implemented across all tasks}
   **Technical Context**: {current tech stack from session cache, file context}
   **Specific Requirements**: {all task details, files to modify, implementation scope}
   **Scope**: Complete implementation phase - need current best practices and patterns
   **Dependencies**: {other systems or libraries involved based on all task context}
   
   I need current documentation insights for: {detected libraries/frameworks across all tasks}
   Focus on: {implementation patterns, API usage, integration examples relevant to all tasks}"
   ```
   
   **Tech Doc Agent Execution:**
   - Wait for tech-doc-agent to return comprehensive documentation analysis
   - Integrate findings into implementation guidance for all tasks
   - Reference specific API patterns and current best practices
   - Include version-specific considerations and implementation examples
   
   **Load Task Context:**
   - Load original issue document (`issues/{ticket-id}-issue.md`)
   - Combine steering context from agent with session learnings from context cache if available
   - Integrate tech-doc-agent insights when libraries/frameworks detected
   - Comprehensive task analysis for all remaining tasks

### 5. **Present Complete Implementation Plan**
   Show comprehensive plan for ALL remaining tasks:
   ```markdown
   # Complete Implementation Plan: {ticket-id}
   
   ## All Remaining Tasks
   {list-all-incomplete-tasks-with-details}
   
   ## Implementation Strategy
   - **Execution Order**: {dependency-respecting-order}
   - **Total Files**: {comprehensive-file-list}
   - **Complexity Assessment**: {overall-complexity}
   
   ## Comprehensive Implementation Approach
   {detailed-steps-for-all-tasks}
   
   ## Steering Compliance
   {patterns-and-standards-to-follow-across-all-tasks}
   
   ## Library Documentation Insights
   {include-if-tech-doc-agent-was-used}
   - **Current Best Practices**: {library-specific-recommendations}
   - **Implementation Patterns**: {api-usage-patterns-and-examples}
   - **Version Considerations**: {compatibility-and-breaking-changes}
   - **Integration Guidelines**: {specific-integration-approaches}
   ```
   
   Ask for confirmation:
   ```   🚀 Ready to implement ALL remaining tasks!
   
   Please review and let me know:
   - "Yes, proceed" / "Looks good" (initiates comprehensive implementation session)
   - "Change approach to use {different-method}"
   - "Skip tasks {numbers}"
   - "Cancel"
   ```
   
   **WAIT FOR USER CONFIRMATION** (for the overall complete plan)

### 6. **Implement All Tasks (Comprehensive Implementation Session)**
   After user confirmation of the overall comprehensive plan:
   - **IMPORTANT**: This session focuses on implementing ALL remaining tasks from the todo list in dependency order.
   - **Execute All Tasks in Order**: The AI will implement all tasks systematically, following dependency chains.
   - **For Each Task**:
     - Mark task as in_progress in todo list
     - Execute task implementation following the comprehensive plan
     - This might involve:
       - Creating new files
       - Adding functions, classes, or components
       - Implementing complex business logic
       - Modifying existing sections of code
       - Adding comprehensive test coverage
       - Utilize Gemini CLI MCP (if available) for advanced codebase understanding, dependency mapping, and architectural context when modifying existing files, integrating new components, or responding to user prompts for changes.
       - **Use tech-doc-agent for just-in-time documentation** when implementing library-specific features, encountering API questions, or needing current best practices for specific frameworks during task implementation.
     - **Present Task Completion**:
       - Clearly describe *what* changes were made and *why*, linking back to the overall plan and relevant steering documents.
       - Show actual code snippets, file modifications, or describe specific changes made for this task.
       - Mark task as completed in todo list
     - **Continue to Next Task**: Automatically proceed to the next task in dependency order
   - **Comprehensive Progress Tracking**:
     - Update todo list status continuously
     - Show progress: "Task X of Y completed"
     - Maintain momentum while respecting dependencies
   - **Handle Dependencies**: Ensure all dependencies are satisfied before starting each task
   - **Error Handling**: If any task encounters critical errors, pause and seek user guidance

   **Key Principles of Comprehensive Implementation:**
   - **Complete Coverage**: Work on ALL remaining tasks systematically
   - **Dependency Respect**: Follow task dependency chains strictly
   - **Quality Consistency**: Maintain code quality across all implementations
   - **Progress Transparency**: Continuously update progress status
   - **Architectural Coherence**: Ensure all tasks work together harmoniously
   
   **Tech Doc Agent Integration During Implementation:**
   - **Just-in-Time Documentation**: Use tech-doc-agent when encountering specific library/framework implementation questions during tasks
   - **API Reference Support**: Call tech-doc-agent for current API patterns, method signatures, or integration examples
   - **Best Practices Guidance**: Get current best practices when implementing library-specific features or patterns
   - **Version-Specific Help**: Access current documentation for version-specific features or breaking changes

### 7. **Complete Implementation & Commit All Tasks**
   - This step is triggered when ALL tasks from the todo list are completed.
   - **Mark All Tasks Complete**: Mark all implemented tasks as completed in todo list.
   - **Update Final Status**: Update status section to show completion.
   - **Add Comprehensive Session Notes**: Add session notes with comprehensive completion summary for ALL tasks to the todo list.
   - **Update Final Session Context**: Update final session context within the todo list.
   - **Determine Commit Ticket ID**:
     - Extract the primary ticket ID from the `work-progress/{ticket-id}.md` filename (e.g., if filename is `AUTH-123.md`, then `ticket-id` is `AUTH-123`). This will be the initial proposed ID.
     - If the filename does not contain a clear ticket ID (e.g., `feature-dashboard.md`):
       - Prompt user: "A clear ticket ID was not found in the `work-progress` filename for this comprehensive implementation. Would you like to provide one for the commit message (e.g., `PROJ-456`)? Type 'none' or press Enter to skip."
       - Wait for user response.
       - Store the provided ID. If the user types 'none' or just presses Enter, the `ticket-id` for the commit will be `null`.
   - **Identify Files for Commit**: Determine all files that were created or modified during the execution of ALL tasks.
   - **Present Comprehensive Commit Details & Seek Confirmation**:
     - Present the proposed commit message and the list of all files to be committed.
     ```markdown
     Comprehensive Commit for ALL Tasks: {ticket-id}
     
     Proposed Commit Message:
     `{OPTIONAL_TICKET_ID_PREFIX}[FEATURE]: Complete implementation - {comprehensive-description}`
     
     Files to be Committed:
     - {comprehensive-file-list}
     
     Tasks Completed:
     - {list-of-all-completed-tasks}
     
     Do you want to commit these changes?
     - "Yes, commit" / "Looks good"
     - "Modify commit message to {new message}"
     - "Remove {file.js} from commit"
     - "Create separate commits per task"
     - "Cancel commit" (reverts changes to uncommitted state)
     ```
     - **WAIT FOR USER CONFIRMATION**
   - **Perform Git Commit (Comprehensive Commit)**:
     - *Only after explicit user confirmation:*
     - Execute `git add {all_identified_files}`
     - Execute `git commit -m "{resolved_comprehensive_commit_message}"`
     - Notify the user of the successful comprehensive commit.
     - Example: "All tasks for {ticket-id} committed successfully."

### 8. **Show Complete Implementation Results**
   **Present comprehensive completion summary:**
   ```markdown
   # Complete Implementation: {ticket-id}
   
   ## ✅ All Tasks Completed
   - {comprehensive-accomplishments-summary}
   - **Git Commit:** Created comprehensive commit for all tasks.
   
   ## Files Modified/Created
   - {comprehensive-file-list} (All files changed during complete implementation)
   
   ## Final Progress
   - Status: {total}/{total} tasks completed ✅
   - Implementation: COMPLETE
   
   ## Tasks Accomplished
   {detailed-list-of-all-completed-tasks}
   ```

### 9. **Update Session Caches (Mandatory)**
   **Automatically update both session and context caches for complete implementation workflow:**
   
   **Update Session Cache:**
   **Use Session Cache Agent**: Use the session-cache-agent to update the session cache with completion details.
   
   **Context for Agent**: "I need to update the session cache for the continue-all command. TicketId='{current-ticket-id}' featureDescription='{current-feature-description}' command='continue-all' status='COMPLETED'. Please update the session cache and ensure git exclusion."
   
   **Agent Execution**: Wait for the session-cache-agent to confirm cache update. Show: "✓ Updated session cache - implementation complete"
   
   **Update Context Cache:**
   - Capture comprehensive session learnings and write to `work-progress/{ticket-id}-context.json`
   - Focus on capturing insights from the complete implementation:
     - **Comprehensive Codebase Insights**: All architectural patterns, frameworks, or design decisions discovered across all tasks
     - **All Key Files**: Complete list of files that were crucial for understanding or implementing all tasks
     - **All Dependencies**: Complete list of libraries, services, or components encountered across all tasks
     - **Complete Business Context**: All domain rules, constraints, or requirements learned during comprehensive implementation
     - **All Technical Constraints**: All performance, security, or design limitations discovered
     - **Complete Architectural Patterns**: All coding patterns, conventions, or approaches used across all tasks
     - **Comprehensive Library Documentation Insights**: All key findings from tech-doc-agent about current best practices, API patterns, version-specific features, and implementation approaches for all libraries used
     - **Complete Integration Patterns**: All successful integration strategies and patterns discovered across all tasks
   - Show: "✓ Updated context cache with comprehensive implementation learnings"

### 10. **Show Final Completion Status**
   **Present final comprehensive completion summary:**
   ```markdown
   ## 🎉 COMPLETE IMPLEMENTATION FINISHED
   
   **Project**: {ticket-id} - ALL TASKS COMPLETED
   **Status**: ✅ FULLY IMPLEMENTED
   
   ## 📊 Implementation Summary
   
   **Tasks Completed**: {total} of {total} (100%)
   **Files Modified**: {file-count} files
   **Implementation Approach**: Comprehensive single-session execution
   **Quality**: All steering patterns followed, library best practices applied
   
   ## 🎯 Final Deliverables
   
   **Core Implementation**:
   - ✅ All core functionality implemented
   - ✅ All integration points completed
   - ✅ All testing requirements fulfilled
   
   **Code Quality**:
   - ✅ Steering compliance verified
   - ✅ Library best practices applied
   - ✅ Architectural consistency maintained
   
   **Documentation & Context**:
   - ✅ Session learnings captured
   - ✅ Implementation insights documented
   - ✅ Future reference context prepared
   
   ## 🚀 Project Status: READY FOR DEPLOYMENT
   
   The complete implementation is finished and ready for:
   - Code review
   - Testing verification
   - Deployment preparation
   - Stakeholder demonstration
   
   **📋 Session Management:**
   - ✅ Context cache updated with complete implementation learnings
   - ✅ Session cache updated to reflect completion
   - ✅ Todo list marked as fully completed
   - ✅ All changes committed to git
   
   Implementation complete! 🎉
   ```

## Example Usage

```bash
/continue-all AUTH-123
# 1. Git branch management: Verify on feature/auth-oauth branch
# 2. Load todo list: work-progress/AUTH-123.md
# 3. Identify ALL remaining tasks: 5 incomplete tasks found
# 4. Load comprehensive context:
#    - All steering docs needed across all tasks
#    - Detect keywords across all tasks: "OAuth", "JWT", "testing" → tech-doc-agent triggered
#    - Call tech-doc-agent: "implementing complete OAuth system with testing"
#    - Tech-doc-agent returns: comprehensive OAuth patterns, testing strategies, security practices
#    - Load session context cache with previous discoveries
# 5. Present comprehensive implementation plan covering all 5 tasks
# 6. User confirms complete plan
# 7. Comprehensive implementation session:
#    - Task 1: OAuth route handlers → completed
#    - Task 2: JWT token management → completed
#    - Task 3: User authentication middleware → completed
#    - Task 4: Error handling and validation → completed
#    - Task 5: Integration tests → completed
# 8. All tasks completed
# 9. Update todo list, mark all tasks complete
# 10. Comprehensive commit with all OAuth implementation
# 11. Update context cache with complete implementation insights
# 12. Implementation fully complete

/continue-all 
# (no parameters - uses session cache)
# Detects recent work: PAYMENT-456 from session cache
# Loads todo list and completes ALL remaining tasks for payment system
```

## Important Notes
- **COMPLETE IMPLEMENTATION**: This command handles ALL remaining tasks from the todo list in one comprehensive session.
- **DEPENDENCY MANAGEMENT**: Strictly follows task dependency chains to ensure proper implementation order.
- **USER CONFIRMATION**: Never modify files without explicit approval for the overall comprehensive plan. **Never commit without explicit user approval.**
- **COMPREHENSIVE COMMITS**: Creates a single comprehensive commit covering all completed tasks.
- **AUTOMATIC CACHE UPDATES**: Session cache, context cache, and todo list are automatically updated after comprehensive completion.
- **COMPLETE PROGRESS TRACKING**: Updates todo list automatically as each task is completed, providing full visibility.
- **STEERING COMPLIANCE**: Always follows referenced patterns and standards across all tasks.
- **COMPREHENSIVE QUALITY**: Maintains code quality and architectural consistency across all implementations.
- **TECH DOC AGENT INTEGRATION**: Automatically uses tech-doc-agent for comprehensive library/framework documentation across all tasks.
- **COMPLETE CONTEXT**: Comprehensive approach ensures all tasks work together harmoniously.