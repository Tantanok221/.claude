# Continue Command

Execute the current task from todo list with focused context.

## Usage
```
/continue <ticket-id> [--lite]
/continue <feature-description> [--lite]
```

## Instructions
Load the current task from an existing todo list and implement it using referenced steering documents. This command focuses on executing one task at a time with minimal context.

**ULTRATHINK MODE ACTIVATED**: Focus deeply on current task implementation with highest quality. Use maximum tokens for precise execution and maintain architectural consistency. Leverage Gemini CLI MCP (Model Context Protocol) capabilities for deep codebase analysis and understanding when needed for task execution.

**--lite Mode Instructions**: When `--lite` is used, streamline the interactive coding session. The AI will proceed with logical micro-tasks, automatically applying changes and committing upon task completion, based on the pre-approved plan and steering documents, with minimal intermediate confirmations. The AI will still present changes, but the default action will be to proceed unless explicitly interrupted or directed otherwise by the user. This mode prioritizes speed and automation based on trust in the AI's execution of the planned task.

## Process

### 1. **Git Branch Management**
   **Check current git status and manage branches:**
   
   **Determine Master Branch:**
   - Check git origin URL: `git remote get-url origin`
   - If origin contains "gitlab" → master branch is "master"
   - If origin contains "github" → master branch is "main"
   - Default to "main" if unable to determine
   
   **Determine Expected Branch Name:**
   - For ticket-id provided (from `work-progress/{ticket-id}.md` filename): `pg/tk-{ticket-id}` (e.g., `pg/tk-fix-user-login`)
   - For feature-description provided (if a matching todo list is found and its ID is derived from feature): `feat/{kebab-case-feature}` (e.g., `feat/add-dark-mode`)
   - **Note**: The exact expected branch name will match the one created by `/plan`.
   
   **Branch Validation Process:**
   ```bash
   # Check current branch
   current_branch=$(git branch --show-current)
   
   # Determine the expected branch name based on the loaded todo list's ID
   # (This logic will be executed after step 2, but conceptually the check happens here)
   # Placeholder for logic: expected_branch = derive_branch_from_todo_list_id(ticket_id_or_feature_description)
   
   # Check if on correct branch
   if current_branch == expected_branch:
     echo "✓ On correct branch: ${current_branch}"
     proceed_to_todo_list_load() # Proceed to the next actual step
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
   
   # Create and checkout the expected branch (assuming it might not exist if planning was just done)
   git checkout -b {expected_branch} || git checkout {expected_branch} # Try create, if exists, just checkout
   echo "✓ Switched to and updated: ${master_branch}, then switched to: ${expected_branch}"
   ```

### 2. **Load Todo List**
   **If ticket-id provided:**
   - Look for `work-progress/{ticket-id}.md`
   - If not found, search for similar ticket IDs in work-progress directory
   
   **If feature-description provided:**
   - Search all todo lists in `work-progress/` directory for relevant content
   - Present options to user if multiple matches found
   - If no matches, suggest running `/create-issue` then `/plan`
   
   **Internal Note**: Once the todo list is loaded, the `ticket-id` from its filename (or the derived `feature-id`) will be used by the "Git Branch Management" step (Step 1) to determine the `expected_branch` name for validation.

### 3. **Find Current Task**
   - Read todo list and locate current task from status section
   - Extract task details: files, steering docs, complexity, dependencies
   - Verify task is ready (dependencies completed)

### 4. **Load Context**
   **Load steering configuration and task-specific context:**
   
   **Load Steering Configuration:**
   - Read `steering/config.md` for always-include docs and feature mappings
   - If config.md doesn't exist, use default (coding-standards.md, anti-patterns.md)
   
   **Load Task Context:**
   - Load original issue document (`issues/{ticket-id}-issue.md`)
   - Load always-include steering documents from config.md
   - Load task-specific steering documents referenced in current task
   - Current task example:
     ```markdown
     - [ ] #3 🟠 Implement OAuth authorization flow
       - Files: routes/auth.js, controllers/AuthController.js
       - Steering: security-architecture.md, api-design-guidelines.md
     
     # Loads from config.md:
     - Always-include docs (e.g., coding-standards.md, anti-patterns.md)
     - Plus task-specific: security-architecture.md, api-design-guidelines.md
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
   
   **[--lite Mode Adjustment]**: In `--lite` mode, skip this explicit overall plan confirmation. Instead, present a very brief summary like "Starting implementation of task #{number} in --lite mode. Automatic progression unless interrupted." and immediately proceed to Step 6.

### 6. **Implement Task (Interactive Coding Session)**
   After user confirmation of the overall task plan (or automatic progression in `--lite` mode):
   - **Enter Interactive Loop**: The AI will now guide the user through the current task's implementation in an iterative manner, breaking it into smaller, logical micro-tasks until the current task from the todo list is resolved.
   - **Execute Micro-Task**: Perform a small, logical part of the current task. This might involve:
     - Creating a new file.
     - Adding a function signature or class definition.
     - Implementing a small, self-contained piece of logic.
     - Modifying an existing section of code.
     - Adding a test case.
     - Utilize Gemini CLI MCP (if available) for advanced codebase understanding, dependency mapping, and architectural context when modifying existing files, integrating new components, or responding to user prompts for changes.
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
     - **[--lite Mode Adjustment]**: In `--lite` mode, after presenting proposed changes, the AI will wait for a very short duration for explicit interruption. If no interruption occurs, it will automatically assume "Apply and Continue" and proceed to the next micro-task. It will still *show* the changes but won't *explicitly ask for confirmation* unless a specific instruction from the user breaks the auto-progression.
       Example in `--lite` mode:
       "Proposed changes for `components/LoginPage.js`: [shows snippet]. Automatically applying and proceeding to next step (type 'stop' or 'modify' to intervene)."
   - **Apply Changes (on User Approval)**: Only write to files or make persistent changes to the codebase *after* explicit user approval for that specific micro-task (or automatic approval in `--lite` mode).
   - **Loop or Conclude**:
     - If more sub-steps remain for the current task (as inferred by AI or guided by user), continue the interactive loop.
     - If the user explicitly says "Done Task" or the AI infers the current task's completion and all micro-tasks are approved: Proceed to "Update Progress & Commit Task."

   **Key Principles of Interactive Session:**
   - **Granularity**: Break down the task into the smallest actionable units to allow precise user control.
   - **Transparency**: Always show proposed changes (code snippets, diffs) clearly before applying them to files.
   - **Control**: User maintains full control over the process, guiding the AI through each incremental step (more explicit in default, more implicit in `--lite`).
   - **Contextual Adaptation**: The AI re-evaluates and adapts its approach based on user feedback during the session.
   - **Progressive Feedback**: Provide continuous updates on current micro-task status and readiness for next steps.

### 7. **Update Progress & Commit Task**
   - This step is triggered *only when the entire current task from the todo list* (as identified in step 3) is considered fully implemented and explicitly confirmed by the user via "Done Task" or implied in `--lite` mode.
   - **Determine Commit Ticket ID**:
     - Extract the primary ticket ID from the `work-progress/{ticket-id}.md` filename (e.g., if filename is `AUTH-123.md`, then `ticket-id` is `AUTH-123`). This will be the initial proposed ID.
     - If the filename does not contain a clear ticket ID (e.g., `feature-dashboard.md`):
       - Prompt user: "A clear ticket ID was not found in the `work-progress` filename for this task. Would you like to provide one for the commit message (e.g., `PROJ-456`)? Type 'none' or press Enter to skip."
       - Wait for user response.
       - Store the provided ID. If the user types 'none' or just presses Enter, the `ticket-id` for the commit will be `null`.
       - **[--lite Mode Adjustment]**: The prompt will be concise: "Ticket ID for commit (e.g., PROJ-456)? Type 'none' or Enter to skip." The AI will still wait for input before proceeding.
   - **Mark Task Complete**: Mark current task as completed in todo list.
   - **Update Status**: Update status section with next task.
   - **Add Session Notes**: Add session notes with completion summary for the *current task* to the todo list.
   - **Update Next Session Context**: Update next session context within the todo list.
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
     - **[--lite Mode Adjustment]**: The presentation will be more concise, and after a brief display, the commit will proceed automatically unless explicitly interrupted.
       Example:
       ```markdown
       Committing Task #{number}: {task-description}
       Message: `{OPTIONAL_TICKET_ID_PREFIX}[TYPE]: {task-description}`
       Files: {file1.js}, {file2.css}
       
       (Auto-committing in 3s... Type 'stop' or 'modify' to intervene)
       ```
   - **Perform Git Commit (Micro-Commit Fashion)**:
     - *Only after explicit user confirmation (or auto-approval in --lite mode):*
     - Execute `git add {identified_files_for_this_task}`
     - Execute `git commit -m "{resolved_commit_message}"`
     - Notify the user of the successful commit.
     - Example: "Changes for task #{number} committed successfully."

### 8. **Show Results**
   ```markdown
   # Task Complete: {ticket-id} - Task #{number}
   
   ## Completed
   - {accomplishments} (Summary of the entire task's completion)
   - **Git Commit:** Created a micro-commit for this task.
   
   ## Files Modified
   - {file-list} (List of all files changed during the task's interactive session)
   
   ## Next Steps
   - Continue: `/clear` then `/continue {ticket-id}`
   - Review: Check work-progress/{ticket-id}.md
   - Progress: {completed}/{total} tasks done
   ```

## Important Notes
- **ONE TASK AT A TIME**: Focus on current task, ignore completed/future tasks during the interactive session.
- **USER CONFIRMATION**: Never modify files without explicit approval for each incremental change or the overall task plan (unless `--lite` mode provides implicit approval). **Never commit without explicit or implied user approval.**
- **PROGRESS TRACKING**: Update todo list automatically after the *entire task* is completed.
- **STEERING COMPLIANCE**: Always follow referenced patterns and standards, re-evaluating if user feedback necessitates a different approach.
- **MICRO-COMMITS**: Each completed task from the todo list results in a dedicated, small Git commit, promoting clear history and easy reverts.

## Example Usage
```bash
/continue AUTH-123
# ... (steps 1-6 as before) ...
# 7. Task #3 completed. Uses AUTH-123 from filename for commit.
#    AI: "Committing for Task #3: Implement OAuth flow"
#    "Proposed Commit Message: [AUTH-123] feat: Implement OAuth authorization flow"
#    "Files to be Committed:"
#    "- routes/auth.js"
#    "- controllers/AuthController.js"
#    "Do you want to commit these changes?"
#    User: "Yes, commit"
#    AI: "Changes for task #3 committed successfully."
#    Task #3 complete. Progress: 3/7 tasks done. Ready for task #4."
# 8. Shows summary of completed task #3, lists all modified files, ready for task #4.

/continue feat-add-dashboard
# ... (steps 1-6 as before) ...
# 7. Task #1 completed.
#    AI: "A clear ticket ID was not found in the `work-progress` filename for this task. Would you like to provide one for the commit message (e.g., `PROJ-456`)? Type 'none' or press Enter to skip."
#    User: "DASH-001"
#    AI: "Committing for Task #1: Setup dashboard routes"
#    "Proposed Commit Message: [DASH-001] feat: Setup dashboard routes"
#    "Files to be Committed:"
#    "- routes/dashboard.js"
#    "- controllers/dashboard.js"
#    "Do you want to commit these changes?"
#    User: "Looks good"
#    AI: "Changes for task #1 committed successfully."
#    Task #1 complete. Progress: 1/5 tasks done. Ready for task #2."
# 8. Shows summary for task #1, lists files, ready for task #2.

/continue another-feature --lite
# ... (steps 1-6 as before) ...
# 7. Task #1 completed.
#    AI: "Ticket ID for commit (e.g., PROJ-456)? Type 'none' or Enter to skip."
#    User: "none"
#    AI: "Committing Task #1: Implement logging"
#    "Message: feat: Implement logging"
#    "Files: services/logger.js, tests/logger.test.js"
#    "(Auto-committing in 3s... Type 'stop' or 'modify' to intervene)"
#    (After 3s, AI commits)
#    AI: "Task #1 completed. Changes committed to branch: `feat: Implement logging`."
# 8. Shows summary for task #1, lists files, ready for task #2.
```
