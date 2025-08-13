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
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the correct Git branch is active for planning, handling all branch checking and switching operations.
   
   **Context for Agent**: The expected branch pattern will be derived from the `ticket-id` or `feature-description` provided to this command. The agent will determine the appropriate branch name and handle switching if needed.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Session Cache & Work Target Identification**
   **Handle session caching and work target identification:**
   
   **Session Cache Logic:**
   - If ticket-id or feature-description provided: Use the provided parameters
   - If no parameters provided: **Use Session Cache Agent** to check for cached session data
     - **Context for Agent**: "I need to check session cache for the plan command. No parameters were provided. Please check for existing `.claude-session-cache.json` and return the cached work target or indicate if user input is needed."
     - **Agent Execution**: Wait for the session-cache-agent to return cached values or prompt for user input
   - If user provides nothing: Exit with "No work specified"
   
   **Work Target Identification:**
   
   **If ticket-id obtained:**
   - **First, look for the design file:** `design/{ticket-id}-design.md`
   - If found: `work_target_file = design/{ticket-id}-design.md`, `work_target_type = "design"`
   - Else (design file not found):
     - Look for the issue file: `issues/{ticket-id}-issue.md`
     - If found:
       - Prompt user: "A design document for this issue was not found. Would you like to create one first using `/design {ticket-id}`? (yes/no)"
       - If user says "yes": Exit command with "Please run `/design {ticket-id}` first."
       - If user says "no" (or declines): `work_target_file = issues/{ticket-id}-issue.md`, `work_target_type = "issue"`
     - Else (neither design nor issue found):
       - Inform user: "Neither a design document nor an issue document was found for {ticket-id}."
       - Suggest: "Please ensure the ticket ID is correct, or create an issue first using `/create-issue`."
       - Exit command.
   
   **If feature-description obtained:**
   - Search all design documents in `design/` directory for relevant content.
   - If no relevant design documents, search all issue documents in `issues/` directory for relevant content.
   - Match against titles, descriptions, and tags.
   - Present top 3 most relevant documents to user.
   - Ask user to select which document to work on, or offer to create new issue/design.
   - If no relevant documents found, suggest creating new issue with `/create-issue` or new design with `/design`.

### 3. **Load and Analyze Work Target Document**
   - Load the `work_target_file` identified in the previous step.
   - If file doesn't exist (after all search attempts), inform user and suggest:
     - Check if ticket ID is correct
     - List available documents in `design/` and `issues/` directories
     - Offer to search for similar ticket IDs
   - **Deep Document Analysis**:
     - Parse document type (design/issue) and content (bug/enhancement/feature)
     - Identify affected components and systems
     - Understand business context and requirements
     - Note technical constraints and dependencies
     - Extract acceptance criteria and success metrics
     - Identify potential complexity areas and edge cases
     - **Extract keywords for steering document mapping** (authentication, database, ui, api, testing, deployment, performance, security, etc.)
     - **If design document**: Extract detailed technical design, API specs, data model changes, etc., to inform task breakdown.
     - **If issue document (fallback)**: Extract high-level requirements and functional details for initial task breakdown.

### 4. **Load Steering Context for Planning**
   **Use Steering Context Reader Agent**: Use the steering-context-reader agent to gather relevant steering documentation for planning this specific issue.
   
   **Context for Agent**: "I'm planning implementation for {ticket-id}: {issue-title-and-description}. This is a {issue-type} that involves {identified-keywords-from-issue-analysis}. I need relevant steering context to create an accurate implementation plan that follows project standards, architectural patterns, and coding guidelines."
   
   **Agent Execution**: Wait for the steering-context-reader agent to return relevant steering context for this issue.

### 4.5. **Library/Framework Documentation Analysis**
   **Use tech-doc-agent when issue involves specific technologies:**
   
   **Detection Sources:**
   - **Issue Document Analysis**: Scan issue document for library/framework keywords and integration requirements
   - **Design Document Analysis**: If design document exists, analyze for technology choices and implementation approaches
   - **Architecture Keywords**: Look for authentication, payment processing, database integration, API development, migration tasks
   
   **Detection Keywords:**
   - Library/framework names: "React", "Express", "OAuth", "JWT", "Stripe", "GraphQL", "MongoDB", "Next.js", etc.
   - Integration patterns: "integrate with", "connect to", "API integration", "third-party service", "SDK"
   - Migration tasks: "migrate from", "upgrade to", "replace with", "modernize", "update version"
   - Technology-specific features: "authentication system", "payment processing", "database migration"
   
   **Tech Doc Agent Integration Process:**
   ```
   IF issue/design documents contain library/framework keywords:
   1. Use tech-doc-agent with comprehensive planning context
   2. Pass detailed context about the planning requirements
   3. Receive focused documentation analysis for task breakdown
   4. Integrate findings into task planning and complexity assessment
   ```
   
   **Context to Provide to Tech Doc Agent:**
   ```
   "I'm creating an implementation plan for {ticket-id}: {issue-title-and-description}.
   
   **Problem Context**: {what needs to be implemented based on issue analysis}
   **Technical Context**: {current architecture from steering docs and design document}
   **Planning Requirements**: {need task breakdown, complexity assessment, implementation guidance}
   **Scope**: Planning phase - need current implementation patterns and best practices for accurate task breakdown
   **Technologies Detected**: {detected libraries, frameworks, or integration requirements}
   
   I need current documentation insights for: {list of detected technologies}
   Focus on: {implementation patterns, API usage, integration approaches, complexity considerations, testing strategies}"
   ```
   
   **Tech Doc Agent Execution:**
   - Wait for tech-doc-agent to return focused documentation analysis
   - Integrate findings into task breakdown and complexity assessment
   - Include current implementation patterns and best practices
   - Reference version-specific considerations and integration approaches
   - Identify testing strategies and implementation complexity factors

### 5. **Comprehensive Context Analysis**
   **Synthesize issue + steering context + tech-doc-agent insights (enhanced by brainstorm in step 6.5):**
   
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
   
   **Library Documentation Insights:**
   {include-if-tech-doc-agent-was-used}
   - Current best practices and implementation patterns for detected technologies
   - Version compatibility requirements and breaking changes to consider
   - Proven integration approaches and architectural patterns
   - Library-specific testing strategies and complexity factors
   - Security and performance considerations for specific technologies
   
   **Risk Assessment:**
   - Potential implementation challenges (including library-specific risks)
   - Areas that need special attention
   - Dependencies that could cause issues (including version compatibility)
   - Testing strategies needed (including library-specific testing approaches)

### 6.5. **Brainstorm and Context Enhancement**
   **Interactive brainstorming process to gather additional context for accurate planning:**
   
   **Brainstorm Initialization:**
   - Analyze the gathered context: issue requirements, steering guidance, and tech-doc-agent insights
   - Identify knowledge gaps and areas needing clarification for optimal task breakdown
   - Generate domain-specific questions based on issue type and complexity
   
   **Use Gemini CLI Brainstorm MCP**: Use the mcp__gemini-cli__brainstorm tool to enhance planning context through iterative questioning.
   
   **Context for Brainstorm Tool:**
   ```
   "I'm planning implementation for {ticket-id}: {issue-title-and-description}.
   
   **Current Context**: {summary-of-issue-steering-and-tech-doc-context}
   **Issue Type**: {bug/feature/enhancement} involving {detected-domains-eg-auth-database-ui}
   **Steering Patterns**: {key-architectural-patterns-from-steering-docs}
   **Technology Insights**: {tech-doc-agent-findings-if-applicable}
   **Complexity Indicators**: {preliminary-complexity-assessment}
   
   I need to brainstorm implementation approaches, potential challenges, and planning considerations to create the most accurate and comprehensive todo list. Focus on identifying:
   - Implementation approach trade-offs and decision points
   - Potential technical challenges and edge cases
   - Business requirements clarification needs
   - Architecture and integration considerations
   - Resource and timeline implications
   
   Generate thoughtful questions that will help me plan this implementation more effectively."
   ```
   
   **Iterative Brainstorm Process:**
   
   **Initial Brainstorm Round:**
   - Call mcp__gemini-cli__brainstorm with comprehensive context
   - Present 3-5 most relevant questions to user based on brainstorm results
   - Focus on critical decisions that affect task breakdown and complexity
   
   **Question Categories by Domain:**
   ```
   Authentication/Security:
   - "What authentication flows need to be supported (SSO, 2FA, social login)?"
   - "Are there specific security compliance requirements (GDPR, SOX, etc.)?"
   - "How should user sessions and token refresh be handled?"
   
   Database/API:
   - "What are the expected data volumes and performance requirements?"
   - "Are there existing APIs that need to remain compatible?"
   - "How should data migration and rollback be handled?"
   
   UI/Frontend:
   - "What browsers and devices need to be supported?"
   - "Are there accessibility requirements (WCAG compliance level)?"
   - "How should the UI handle loading states and errors?"
   
   Performance/Scalability:
   - "What are the expected user loads and peak usage patterns?"
   - "Are there specific response time or throughput requirements?"
   - "How should caching and optimization be prioritized?"
   
   Business Logic:
   - "What edge cases and error scenarios must be handled?"
   - "Are there workflow approval or notification requirements?"
   - "How should business rule validation be implemented?"
   
   Integration/Migration:
   - "What systems need to integrate with this feature?"
   - "Are there data migration or import/export requirements?"
   - "How should backwards compatibility be maintained?"
   ```
   
   **User Response Collection:**
   ```markdown
   Based on the analysis, I have these key questions for better planning:
   
   🧠 **Implementation Approach Questions:**
   1. {domain-specific-question-1}
   2. {domain-specific-question-2}
   3. {domain-specific-question-3}
   
   🛠️ **Technical Considerations:**
   4. {technical-complexity-question}
   5. {integration-or-architecture-question}
   
   Please provide answers to help me create a more accurate implementation plan.
   You can also respond with:
   - "skip brainstorm" - Skip to todo list creation
   - "done" - Sufficient context provided
   - "next questions" - Ask different or follow-up questions
   ```
   
   **Follow-up Brainstorm Rounds (if needed):**
   - **Continue if user provides substantial new context**: Generate follow-up questions based on responses
   - **Stop if user indicates sufficient context**: "skip brainstorm", "done", "enough context"
   - **Stop if minimal new context**: Agent determines enough information gathered
   - **Maximum 3 brainstorm rounds**: Prevent user fatigue and infinite loops
   
   **Follow-up Question Generation:**
   - Use mcp__gemini-cli__brainstorm again with updated context including user responses
   - Focus on areas that need clarification based on previous answers
   - Generate 2-3 targeted follow-up questions maximum per round
   
   **Brainstorm Results Synthesis:**
   - Capture all user responses and insights from brainstorm sessions
   - Identify implementation approaches and technical decisions revealed
   - Note complexity factors and risk areas discovered
   - Document business constraints and requirements clarified
   - Prepare enhanced context for todo list generation
   
   **Enhanced Context Integration:**
   ```json
   {
     "brainstorm_insights": {
       "implementation_approach": "Clarified approach based on user input",
       "technical_decisions": ["Decision 1", "Decision 2", "Decision 3"],
       "business_constraints": ["Constraint 1", "Constraint 2"],
       "complexity_factors": ["Factor 1", "Factor 2"],
       "risk_areas": ["Risk 1", "Risk 2"],
       "integration_requirements": ["Requirement 1", "Requirement 2"],
       "user_clarifications": {
         "question": "user response",
         "follow_up": "clarification"
       }
     }
   }
   ```

### 8. **Present Todo List for Review & Iteration**
   **CRITICAL: DO NOT CREATE ANY FILES - This is an iterative planning phase**
   
   **Show Complete Todo List Content for Review:**
   ```markdown
   📋 Here's the proposed todo list for review:
   
   **Planned File: `work-progress/{ticket-id}.md`**
   
   ---
   
   # Work Progress: {ticket-id}
   
   ## Source Document Reference
   - Source: {work_target_type} ({work_target_file})
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
     - Libraries: {relevant-library-guidance-if-applicable}
     - Complexity: low/medium/high
     - Dependencies: none/#{task-numbers}
   
   ### Phase 2: Core Implementation  
   - [ ] #2 🟠 {task-description}
     - Files: {file-list}
     - Steering: {relevant-steering-docs}
     - Libraries: {relevant-library-guidance-if-applicable}
     - Complexity: low/medium/high
     - Dependencies: #{previous-tasks}
   
   ### Phase 3: Integration & Testing
   - [ ] #3 🔴 {task-description}
     - Files: {file-list}
     - Steering: {relevant-steering-docs}
     - Libraries: {relevant-library-guidance-if-applicable}
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
   
   ### Library/Framework Guidance
   {include-if-tech-doc-agent-was-used}
   - **Implementation Patterns**: {current-patterns-for-detected-libraries}
   - **Best Practices**: {library-specific-recommendations-from-docs}
   - **Version Considerations**: {compatibility-and-breaking-changes}
   - **Integration Approaches**: {proven-integration-strategies}
   - **Testing Patterns**: {library-specific-testing-approaches}
   - **Security Guidelines**: {technology-specific-security-patterns}
   
   ### Brainstorm Insights
   {include-if-brainstorm-was-conducted}
   - **Implementation Approach**: {clarified-approach-from-brainstorm}
   - **Technical Decisions**: {key-decisions-identified-through-brainstorm}
   - **Business Constraints**: {constraints-clarified-with-user}
   - **Complexity Factors**: {complexity-considerations-from-discussion}
   - **Risk Mitigation**: {risks-identified-and-mitigation-strategies}
   - **Integration Requirements**: {integration-needs-clarified}
   - **User Clarifications**: {important-user-responses-and-context}
   
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
   - Library guidance: {tech-doc-agent-insights-if-applicable}
   - Brainstorm insights: {user-clarifications-and-enhanced-context-if-applicable}
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
   

### 9. **Iterative Planning Mode**
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

### 10. **Create Todo List File (Only After Explicit Command)**
   **Only when user says "create file" or "save the todo list":**
   
   **File Creation Process:**
   - Create directory: `work-progress/` if it doesn't exist
   - Create file: `work-progress/{ticket-id}.md`
   - Include all todo list content from step 8
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

### 11. **Planning Output**
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

### 12. **Update Session Cache & Optimization Notes**
   
   **Session Cache Update (After Successful Planning):**
   **Use Session Cache Agent**: Use the session-cache-agent to update the session cache with current work details.
   
   **Context for Agent**: "I need to update the session cache for the plan command. TicketId='{current-ticket-id}' featureDescription='{current-feature-description}' command='plan'. Please update the session cache and ensure git exclusion."
   
   **Agent Execution**: Wait for the session-cache-agent to confirm cache update. Show "✓ Session cache updated for future /plan and /continue commands"
   
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
- **LIBRARY GUIDANCE**: Uses tech-doc-agent to include current best practices and implementation patterns for detected technologies
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
/plan AUTH-123
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/other-work → Wrong branch!
# 3. Switch to main, fetch/pull latest, create pg/tk-AUTH-123
# 4. Load: design/AUTH-123-design.md (OAuth login implementation)
# 5. Load relevant steering docs for analysis
# 5.5. Detect keywords: "OAuth", "JWT", "authentication" → tech-doc-agent triggered
#      Call tech-doc-agent: "planning OAuth login implementation with JWT tokens"
#      Tech-doc-agent returns: OAuth 2.0 patterns, Passport.js guidance, JWT best practices
# 6. Synthesize context: issue + steering + tech-doc-agent insights
# 6.5. Brainstorm session: Ask questions about OAuth flows, token refresh, session management
#      User provides context: "Support social login, need token refresh, session timeout = 24hrs"
#      Follow-up questions about error handling and security requirements
#      User clarifies: "Standard OAuth errors, need audit logging for security events"
# 7. Present todo list with 8 tasks across 3 phases, including library guidance + brainstorm insights
# 8. User: "modify task 5 to include JWT refresh token handling"
# 9. Show updated todo list with enhanced JWT task using tech-doc-agent insights
# 10. User: "add task for OAuth error handling"  
# 11. Show updated todo list with new OAuth-specific error handling task
# 12. User: "create file"
# 13. Create work-progress/AUTH-123.md with library guidance + brainstorm insights
# 14. Ready for: /continue AUTH-123

/plan "add Stripe payment processing"
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Search issues for payment-related work and present top matches for user selection
# 3. Create feat/add-stripe-integration branch
# 4. Load issue + relevant steering documents
# 4.5. Detect keywords: "Stripe", "payment processing" → tech-doc-agent triggered
#      Call tech-doc-agent: "planning Stripe payment integration"
#      Tech-doc-agent returns: Stripe SDK patterns, webhook handling, security practices
# 5. Synthesize context: issue + steering + Stripe best practices  
# 5.5. Brainstorm session: Ask questions about payment flows, webhook handling, compliance
#      User provides context: "Support subscriptions, one-time payments, need PCI compliance"
#      Follow-up questions about error handling and refund processes
#      User clarifies: "Handle failed payments gracefully, support partial refunds"
# 6. Present todo list for review with payment integration tasks + Stripe-specific guidance + brainstorm insights
# 7. User iterates on the plan: "add webhook signature validation", "include PCI compliance tasks"
# 8. User: "save the todo list"  
# 9. Create work-progress/add-stripe-integration.md with Stripe implementation guidance + brainstorm insights  
# 10. Ready for: /continue add-stripe-integration
```

## What Happens After This Command?

**Planning Complete:**
- **Todo list created** - External file with comprehensive task breakdown
- **Branch prepared** - Correct git branch created from latest master/main
- **Steering analyzed** - Relevant patterns and standards identified
- **Library guidance included** - Current best practices and implementation patterns for detected technologies (via tech-doc-agent)
- **Brainstorm insights captured** - User clarifications and enhanced context integrated into planning
- **Dependencies mapped** - Task order and relationships established

**Ready for Development:**
- **Clear your context** - Use `/clear` to start with fresh context
- **Start implementing** - Use `/continue {ticket-id}` to begin work
- **Track progress** - Todo list shows completion status as you work
- **Resume anytime** - Can pause and come back to work later
