# Continue Command

Execute the current task from todo list with focused context.

## Usage
```
/continue <ticket-id>
/continue <feature-description>
/continue
```

**Note**: When no parameters are provided, the command will use cached session data from previous work in this project, or prompt you to specify what to work on.

## Instructions
Load the current task from an existing todo list and implement it using referenced steering documents. This command focuses on executing one task at a time with minimal context.

**ULTRATHINK MODE ACTIVATED**: Focus deeply on current task implementation with highest quality. Use maximum tokens for precise execution and maintain architectural consistency. Leverage Gemini CLI MCP (Model Context Protocol) capabilities for deep codebase analysis and understanding when needed for task execution.

## Process

### 1. **Git Branch Management**
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the correct Git branch is active for the task, handling all branch checking and switching operations.
   
   **Context for Agent**: The expected branch pattern will be derived from the `ticket-id` or `feature-description` obtained in Step 2 (Session Cache & Todo List Loading). The agent will determine the appropriate branch name and handle switching if needed.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Session Cache & Todo List Loading**
   **Handle session caching and todo list loading:**
   
   **Session Cache Logic:**
   - If ticket-id or feature-description provided: Use the provided parameters
   - If no parameters provided: **Use Session Cache Agent** to check for cached session data
     - **Context for Agent**: "I need to check session cache for the continue command. No parameters were provided. Please check for existing `.claude-session-cache.json` and return the cached work target or indicate if user input is needed."
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

### 3. **Find Current Task**
   - Read todo list and locate current task from status section
   - Extract task details: files, steering docs, complexity, dependencies
   - Verify task is ready (dependencies completed)

### 4. **Load Context**
   **Load steering configuration, task-specific context, and session learnings:**
   
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
   
   **Use Steering Context Reader Agent**: Use the steering-context-reader agent to gather relevant steering documentation for the current task.
   
   **Context for Agent**: "I'm working on task #{task-number} for {ticket-id}: {task-description}. The task involves files: {file-list} and references steering docs: {steering-doc-list}. I need relevant context from the steering directory to implement this task according to project standards and patterns. Focus on {task-specific-focus-areas}."
   
   **Agent Execution**: Wait for the steering-context-reader agent to return only the most relevant steering context for the current task. The agent will read steering/config.md, identify relevant documents, and return filtered context to prevent context bloat.
   
   **Library Documentation Context via Tech Doc Agent:**
   **Use tech-doc-agent when current task involves libraries, frameworks, or integrations:**
   
   **Detection Keywords in Task Description:**
   - Library/framework names: "React", "Express", "Node.js", "JWT", "OAuth", "Stripe", etc.
   - Migration tasks: "migrate", "upgrade", "update version", "replace with"
   - Integration tasks: "integrate with", "connect to", "API integration", "SDK"
   - Implementation patterns: "authentication", "payment", "database", "caching"
   
   **Tech Doc Agent Integration Process:**
   ```
   IF current task contains library/framework keywords:
   1. Use tech-doc-agent with task-specific context
   2. Pass comprehensive context about the implementation task
   3. Receive focused documentation analysis for the specific task
   4. Integrate findings into implementation approach
   ```
   
   **Context to Provide to Tech Doc Agent:**
   ```
   "I'm implementing task #{task-number} for {ticket-id}: {task-description}.
   
   **Problem Context**: {brief description of what needs to be implemented}
   **Technical Context**: {current tech stack from session cache, file context}
   **Specific Requirements**: {task details, files to modify, implementation scope}
   **Scope**: Implementation phase - need current best practices and patterns
   **Dependencies**: {other systems or libraries involved based on task context}
   
   I need current documentation insights for: {detected libraries/frameworks}
   Focus on: {implementation patterns, API usage, integration examples relevant to the task}"
   ```
   
   **Tech Doc Agent Execution:**
   - Wait for tech-doc-agent to return focused documentation analysis
   - Integrate findings into implementation guidance
   - Reference specific API patterns and current best practices
   - Include version-specific considerations and implementation examples
   
   **Load Task Context:**
   - Load original issue document (`issues/{ticket-id}-issue.md`)
   - Combine steering context from agent with session learnings from context cache if available
   - Integrate tech-doc-agent insights when libraries/frameworks detected
   - Current task example:
     ```markdown
     - [ ] #3 🟠 Implement OAuth authorization flow
       - Files: routes/auth.js, controllers/AuthController.js
       - Steering: security-architecture.md, api-design-guidelines.md
     
     # Agent returns filtered context from:
     - Always-include docs (e.g., coding-standards.md, anti-patterns.md)
     - Plus task-specific: security-architecture.md, api-design-guidelines.md
     # Plus session learnings from context cache if available
     # Plus tech-doc-agent insights for OAuth libraries and JWT implementation
     ```

### 5. **Present Implementation Plan**
   Show focused plan for current task:
   ```markdown
   # Current Task: {ticket-id} - Task #{number}
   
   ## Task Details
   - Task: {description}
   - Files: {file-list}
   - Complexity: {low/medium/high}
   
   ## Implementation Approach
   {specific-steps}
   
   ## Steering Compliance
   {patterns-and-standards-to-follow}
   
   ## Library Documentation Insights
   {include-if-tech-doc-agent-was-used}
   - **Current Best Practices**: {library-specific-recommendations}
   - **Implementation Patterns**: {api-usage-patterns-and-examples}
   - **Version Considerations**: {compatibility-and-breaking-changes}
   - **Integration Guidelines**: {specific-integration-approaches}
   ```
   
   Ask for confirmation:
   ```   🚀 Ready to implement current task!
   
   Please review and let me know:
   - "Yes, proceed" / "Looks good" (initiates interactive coding session)
   - "Change approach to use {different-method}"
   - "Skip to next task"
   - "Cancel"
   ```
   
   **WAIT FOR USER CONFIRMATION** (for the overall plan)

### 6. **Implement Task (Interactive Coding Session)**
   After user confirmation of the overall task plan:
   - **Enter Interactive Loop**: The AI will now guide the user through the current task's implementation in an iterative manner, breaking it into smaller, logical micro-tasks until the current task from the todo list is resolved.
   - **Execute Micro-Task**: Perform a small, logical part of the current task. This might involve:
     - Creating a new file.
     - Adding a function signature or class definition.
     - Implementing a small, self-contained piece of logic.
     - Modifying an existing section of code.
     - Adding a test case.
     - Utilize Gemini CLI MCP (if available) for advanced codebase understanding, dependency mapping, and architectural context when modifying existing files, integrating new components, or responding to user prompts for changes.
     - **Use tech-doc-agent for just-in-time documentation** when implementing library-specific features, encountering API questions, or needing current best practices for specific frameworks during micro-task implementation.
   - **Present Proposed Changes**:
     - Clearly describe *what* changes were made and *why*, linking back to the overall plan and relevant steering documents.
     - Show actual code snippets, simulated file diffs, or describe specific modifications to files in the chat.
     - Example: "I propose creating `routes/auth.js` and adding the basic OAuth route structure as per `api-design-guidelines.md` and the task requirements.
       ```javascript
       // routes/auth.js
       const express = require('router');
       const router = express.Router();
       // ... other imports for controllers

       router.get('/oauth/callback', AuthController.handleOAuthCallback);

       module.exports = router;
       ```
       Please review these proposed changes."
   - **Request User Feedback/Approval**:
     - Prompt the user for review and next actions for *this specific micro-task*.
     - Wait for explicit user instruction.
     - Options provided to the user:
       - **"Apply and Continue"** / **"Looks good"**: Apply the proposed changes to the actual files and proceed to the next micro-task or the next logical step within the current task.
       - **"Modify X to Y"** / **"Change this line to..."**: Request specific changes to the proposed code or approach for the current micro-task.
       - **"Show me changes in file Z before applying"**: Request a detailed diff or full file content before applying.
       - **"Undo last change"**: Revert the last proposed (and not yet applied) change or applied micro-task.
       - **"Change approach for this step to use {different-method}"**: Request a different strategy for the current micro-task.
       - **"Skip this step"**: Move past the current micro-task without applying changes (with a warning if it's a dependency for future steps).
       - **"Done Task" / "Finish this task"**: Indicate that the *current task from the todo list* is complete and exit the interactive session to proceed to the update and commit step.
       - **"Cancel"**: Exit the interactive session, abandoning the current task without marking it complete.
   - **Apply Changes (on User Approval)**: Only write to files or make persistent changes to the codebase *after* explicit user approval for that specific micro-task.
   - **Loop or Conclude**:
     - If more sub-steps remain for the current task (as inferred by AI or guided by user), continue the interactive loop.
     - If the user explicitly says "Done Task" or the AI infers the current task's completion and all micro-tasks are approved: Proceed to "Update Progress & Commit Task."

   **Key Principles of Interactive Session:**
   - **Granularity**: Break down the task into the smallest actionable units to allow precise user control.
   - **Transparency**: Always show proposed changes (code snippets, diffs) clearly before applying them to files.
   - **Control**: User maintains full control over the process, guiding the AI through each incremental step.
   - **Contextual Adaptation**: The AI re-evaluates and adapts its approach based on user feedback during the session.
   - **Progressive Feedback**: Provide continuous updates on current micro-task status and readiness for next steps.
   
   **Tech Doc Agent Integration During Implementation:**
   - **Just-in-Time Documentation**: Use tech-doc-agent when encountering specific library/framework implementation questions during micro-tasks
   - **API Reference Support**: Call tech-doc-agent for current API patterns, method signatures, or integration examples
   - **Best Practices Guidance**: Get current best practices when implementing library-specific features or patterns
   - **Version-Specific Help**: Access current documentation for version-specific features or breaking changes
   - **User-Requested Documentation**: Allow user to request "get docs for [library]" or "check current [framework] patterns" during implementation
   
   **Tech Doc Agent Usage Scenarios in Interactive Session:**
   - When implementing authentication flows (OAuth, JWT, etc.)
   - During API integrations with third-party services
   - When working with specific framework patterns (React hooks, Express middleware, etc.)
   - For database migration or ORM usage patterns
   - During testing implementation with specific testing libraries
   - When encountering version compatibility or deprecation issues

### 7. **Update Progress & Commit Task**
   - This step is triggered *only when the entire current task from the todo list* (as identified in step 3) is considered fully implemented and explicitly confirmed by the user via "Done Task".
   - **Mark Task Complete**: Mark current task as completed in todo list.
   - **Update Status**: Update status section with next task.
   - **Add Session Notes**: Add session notes with completion summary for the *current task* to the todo list.
   - **Update Next Session Context**: Update next session context within the todo list.
   - **Determine Commit Ticket ID**:
     - Extract the primary ticket ID from the `work-progress/{ticket-id}.md` filename (e.g., if filename is `AUTH-123.md`, then `ticket-id` is `AUTH-123`). This will be the initial proposed ID.
     - If the filename does not contain a clear ticket ID (e.g., `feature-dashboard.md`):
       - Prompt user: "A clear ticket ID was not found in the `work-progress` filename for this task. Would you like to provide one for the commit message (e.g., `PROJ-456`)? Type 'none' or press Enter to skip."
       - Wait for user response.
       - Store the provided ID. If the user types 'none' or just presses Enter, the `ticket-id` for the commit will be `null`.
   - **Identify Files for Commit**: Determine all files that were created or modified during the execution of this specific task.
   - **Present Commit Details & Seek Confirmation**:
     - Present the proposed commit message and the list of files to be committed.
     ```markdown
     Commit for Task #{number}: {task-description}
     
     Proposed Commit Message:
     `{OPTIONAL_TICKET_ID_PREFIX}[TYPE]: {task-description}`
     
     Files to be Committed:
     - {file1.js}
     - {file2.css}
     - {file3.md}
     
     Do you want to commit these changes?
     - "Yes, commit" / "Looks good"
     - "Modify commit message to {new message}"
     - "Remove {file.js} from commit"
     - "Cancel commit" (reverts changes to uncommitted state)
     ```
     - **WAIT FOR USER CONFIRMATION**
   - **Perform Git Commit (Micro-Commit Fashion)**:
     - *Only after explicit user confirmation:*
     - Execute `git add {identified_files_for_this_task}`
     - Execute `git commit -m "{resolved_commit_message}"`
     - Notify the user of the successful commit.
     - Example: "Changes for task #{number} committed successfully."

### 8. **Show Task Results**
   **Present task completion summary:**
   ```markdown
   # Task Complete: {ticket-id} - Task #{number}
   
   ## Completed
   - {accomplishments} (Summary of the entire task's completion)
   - **Git Commit:** Created a micro-commit for this task.
   
   ## Files Modified
   - {file-list} (List of all files changed during the task's interactive session)
   
   ## Progress
   - Current: {completed}/{total} tasks done
   - Next: Task #{next-number} ready for implementation
   ```

### 9. **Cache Update Confirmation**
   **Ask user about updating cache information:**
   ```markdown
   Would you like to update the session cache with learnings from this task?
   This will help future /continue sessions resume with context from this work.
   
   Options:
   - "Yes, update cache" - Update both session and context caches
   - "No" or "Skip" - Continue without updating caches
   ```
   - **WAIT FOR USER RESPONSE**
   - If user declines, skip to **Show Next Steps** (Step 11)
   - If user accepts, proceed to cache updates (Step 10)

### 10. **Update Session Caches**
   **Update both session and context caches (only if user confirmed in Step 9):**
   
   **Update Session Cache:**
   **Use Session Cache Agent**: Use the session-cache-agent to update the session cache with current work details.
   
   **Context for Agent**: "I need to update the session cache for the continue command. TicketId='{current-ticket-id}' featureDescription='{current-feature-description}' command='continue'. Please update the session cache and ensure git exclusion."
   
   **Agent Execution**: Wait for the session-cache-agent to confirm cache update. Show: "✓ Updated session cache for future commands"
   
   **Update Context Cache:**
   - Capture session learnings and write to `work-progress/{ticket-id}-context.json`
   - Focus on capturing insights that would help future sessions:
     - **Codebase Insights**: Important architectural patterns, frameworks, or design decisions discovered
     - **Key Files**: Files that were crucial for understanding or implementing this task
     - **Dependencies**: Important libraries, services, or components encountered
     - **Business Context**: Domain rules, constraints, or requirements learned during implementation
     - **Technical Constraints**: Performance, security, or design limitations discovered
     - **Architectural Patterns**: Coding patterns, conventions, or approaches used in this codebase
     - **Library Documentation Insights**: Key findings from tech-doc-agent about current best practices, API patterns, version-specific features, and implementation approaches for libraries used
     - **Integration Patterns**: Successful integration strategies and patterns discovered for third-party services or libraries
   - Show: "✓ Updated context cache with session learnings"

### 11. **Show Next Steps**
   **Present final completion summary and next actions:**
   ```markdown
   ## Next Steps
   - Continue work: `/clear` then `/continue {ticket-id}`
   - Review progress: Check work-progress/{ticket-id}.md
   - Overall progress: {completed}/{total} tasks completed
   
   Ready for next development session!
   ```

## Example Usage

```bash
/continue AUTH-123
# 1. Git branch management: Verify on feature/auth-oauth branch
# 2. Load todo list: work-progress/AUTH-123.md
# 3. Find current task: "Implement OAuth authorization flow"
# 4. Load context:
#    - Steering docs: security-architecture.md, api-design-guidelines.md
#    - Detect keywords: "OAuth", "authorization" → tech-doc-agent triggered
#    - Call tech-doc-agent: "implementing OAuth authorization flow for Express.js app"
#    - Tech-doc-agent returns: OAuth 2.0 patterns, Passport.js examples, JWT best practices
#    - Load session context cache with previous discoveries
# 5. Present implementation plan with tech-doc-agent insights:
#    - OAuth 2.0 authorization code flow with PKCE
#    - Passport OAuth2 strategy configuration
#    - JWT token handling patterns
# 6. User confirms plan
# 7. Interactive session:
#    - Micro-task 1: Create OAuth route handler
#    - During implementation: "How should I handle OAuth errors?"
#    - Use tech-doc-agent for current error handling patterns
#    - Apply OAuth-specific error handling based on docs
#    - Continue with remaining micro-tasks
# 8. User says "Done Task"
# 9. Update todo list, mark task complete
# 10. Commit changes with OAuth implementation
# 11. Update context cache with OAuth implementation insights
# 12. Ready for next task

/continue 
# (no parameters - uses session cache)
# Detects recent work: AUTH-123 from session cache
# Continues with next task in sequence
```

## Important Notes
- **ONE TASK AT A TIME**: Focus on current task, ignore completed/future tasks during the interactive session.
- **USER CONFIRMATION**: Never modify files without explicit approval for each incremental change or the overall task plan. **Never commit without explicit user approval.**
- **PROGRESS TRACKING**: Update todo list automatically after the *entire task* is completed.
- **STEERING COMPLIANCE**: Always follow referenced patterns and standards, re-evaluating if user feedback necessitates a different approach.
- **MICRO-COMMITS**: Each completed task from the todo list results in a dedicated, small Git commit, promoting clear history and easy reverts.
- **TECH DOC AGENT INTEGRATION**: Automatically use tech-doc-agent for library/framework documentation during task implementation.
