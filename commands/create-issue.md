# Create Issue Command

Create comprehensive, steering-aware issue documents that provide rich context for implementation planning and development workflow.

## Usage
```
/create-issue <feature_description> [--ticket-id TICKET_ID]
```

## Instructions
You are creating well-structured issue documents that integrate with the steering-aware development workflow. These issues will be consumed by `/plan` command for implementation planning, so they must contain sufficient technical detail and architectural context.

**ULTRATHINK MODE ACTIVATED**: This is critical issue creation that requires deep analysis of both the feature requirements and existing architectural context. Use maximum tokens to thoroughly understand the feature, analyze relevant steering documents, and create the most comprehensive issue possible. Think deeply about implementation complexity, architectural patterns, and how this feature fits into the existing system.


**PURPOSE**: Create steering-aware issues that enable accurate implementation planning and maintain architectural consistency throughout the development workflow.

## Process

### 1. **Git Branch Management**
   **Ensure we're on master branch for issue creation:**
   
   **Determine Master Branch:**
   - Check git origin URL: `git remote get-url origin`
   - If origin contains "gitlab" → master branch is "master"
   - If origin contains "github" → master branch is "main"
   - Default to "main" if unable to determine
   
   **Branch Validation Process:**
   ```bash
   # Check current branch
   current_branch=$(git branch --show-current)
   
   # Determine correct master branch
   master_branch="main" # or "master" based on origin
   
   # Check if on master branch
   if current_branch == master_branch:
     echo "✓ On correct branch: ${current_branch}"
     proceed_to_steering_analysis()
   else:
     echo "✗ Not on master branch. Current: ${current_branch}, Expected: ${master_branch}"
     switch_to_master()
   ```
   
   **Switch to Master Process (if needed):**
   ```bash
   # Switch to master and update
   git checkout {master_branch}  # master or main
   git fetch origin
   git pull origin {master_branch}
   echo "✓ Switched to and updated: ${master_branch}"
   ```

### 2. **Load and Analyze Steering Context**
   **Before creating the issue, understand the architectural landscape:**
   
   **Check for Steering Documents:**
   - Look for `steering/` directory
   - If steering documents don't exist, inform user and suggest running `/steering-setup`
   - If steering exists, proceed with steering-aware issue creation
   
   **Load Steering Configuration:**
   - Read `steering/config.md` for always-include docs and feature mappings
   - If config.md doesn't exist, use default behavior (coding-standards.md, anti-patterns.md)
   
   **Analyze Feature for Steering Relevance:**
   ```
   Feature Analysis → Use Config Mappings:
   
   Load always-include docs from config.md, then:
   IF feature involves authentication/security → load docs from "authentication" mapping
   IF feature involves APIs → load docs from "api" mapping  
   IF feature involves database → load docs from "database" mapping
   IF feature involves UI → load docs from "ui" mapping
   IF feature involves integrations → load docs from "integration" mapping
   IF feature involves business logic → load docs from "business" mapping
   ```
   
   **Load Relevant Steering Documents:**
   - Read always-include documents from config.md
   - Read feature-specific documents based on config mappings
   - Extract architectural constraints and patterns
   - Identify coding standards and conventions
   - Note business rules and technical constraints

### 3. **Research the Project**
   - Examine the current project's structure, existing issues directory (if it exists), and documentation
   - Look for any CONTRIBUTING.md, issue templates, or similar files that might contain guidelines for creating issues
   - Note the project's coding style, naming conventions, and any specific requirements for issue documentation
   - Check if there's an existing issues/ directory structure and naming conventions
   - **Cross-reference with steering documents** for consistency

### 4. **Research Best Practices** 
   - Search for current best practices in writing issue documentation, focusing on clarity, completeness, and actionability
   - Look for examples of well-written issues in popular open-source projects for inspiration
   - Search the web for best practices on the topics handled
   - **Integrate steering document patterns** with industry best practices

### 5. **Handle Ticket ID and Linear Integration**
   **Ticket ID Processing:**
   - If a ticket ID is provided via --ticket-id, use that ID
   - If no ticket ID is provided:
     a. Ask the user if they want to provide a custom ticket ID
     b. Wait for user response
     c. If user provides a ticket ID, use it
     d. If user declines or says they don't want to provide one, then create a short, descriptive ticket ID by summarizing the feature description into 2-4 words, using kebab-case format (e.g., "fix-login-button", "add-dark-mode", "mobile-safari-bug")

   **Linear MCP Integration:**
   - If a ticket ID is provided and follows Linear format (e.g., "PG-123"), attempt to fetch ticket details from Linear:
     ```
     Use MCP Linear tools to:
     1. Fetch issue details by ID using mcp__linear-server__get_issue
     2. Extract relevant information: title, description, status, priority, assignee, team, project, labels, estimate, url, gitBranchName
     3. Include Linear URL and comprehensive project context
     4. Use this information to enhance the issue document with Linear context
     ```
   
   **Linear Information Integration:**
   - If Linear ticket found:
     - Use Linear title and description as additional context
     - Include Linear metadata: status, priority, assignee, team, project, labels, estimate, due_date
     - Reference the Linear ticket URL in the issue document
     - Cross-reference Linear requirements with local steering documents
     - Enhance business context with Linear project and team information
   - If Linear ticket not found or MCP unavailable:
     - Continue with standard issue creation process
     - Note the ticket ID for future reference

### 6. **Present Steering-Informed Plan**
   - Based on your research and steering analysis, outline a plan for creating the issue document
   - Include the proposed structure of the issue, the ticket ID, filename format, and how you'll incorporate project-specific conventions
   - **Show which steering documents influenced the plan**
   - Present this plan in `<plan>` tags
   - Include any reference links to feature base or other sources that have the source of the user request
   - Please ensure any references to local files with line numbers or repositories are included at the bottom of the issue document for future reference

### 7. **Create Comprehensive Issue Document**
   **Once the plan is approved, draft the steering-aware issue document:**
   
   **Enhanced Issue Structure:**
   ```markdown
   # {Issue Title}
   
   ## Metadata
   - **Type**: bug/enhancement/feature
   - **Priority**: low/medium/high/critical
   - **Status**: open
   - **Created**: {timestamp}
   - **Ticket ID**: {ticket-id}
   - **Linear Context**: {linear-ticket-info-if-available}
   
   ## Business Context
   - **Problem Statement**: What business problem does this solve?
   - **Business Value**: Impact on users, revenue, or operations
   - **User Stories**: Who benefits and how?
   - **Success Metrics**: How will success be measured?
   
   ## Technical Requirements
   - **Architecture Impact**: Which components/services are affected?
   - **Integration Points**: External services, APIs, databases involved
   - **Performance Requirements**: SLAs, load expectations, response times
   - **Security Considerations**: Authentication, authorization, data protection
   - **Compliance Requirements**: Regulatory or business compliance needs
   
   ## Implementation Guidance (From Steering Context)
   - **Complexity Estimate**: Low/Medium/High (based on architectural analysis)
   - **Estimated Task Count**: Rough breakdown (5-15 tasks)
   - **Dependencies**: Other issues, external changes, team dependencies
   - **Risk Areas**: Potential challenges, technical debt, integration complexity
   
   ## Steering Document Context
   - **Architecture Patterns**: Relevant patterns from steering/architecture/
   - **Coding Standards**: Applicable standards from steering/standards/
   - **Implementation Patterns**: Relevant patterns from steering/patterns/
   - **Business Constraints**: Relevant context from steering/context/
   
   ## Detailed Requirements
   - **Functional Requirements**: What the system must do
   - **Non-Functional Requirements**: Performance, security, usability
   - **Edge Cases**: Unusual scenarios to handle
   - **Error Handling**: Expected error scenarios and responses
   
   ## Acceptance Criteria (Steering-Informed)
   - **Functional Criteria**: Feature works as specified
   - **Technical Criteria**: Follows architectural patterns and coding standards
   - **Performance Criteria**: Meets performance requirements
   - **Security Criteria**: Implements required security measures
   - **Testing Criteria**: Has appropriate test coverage
   
   ## Implementation Hints for /plan Command
   - **Likely File Changes**: Which files/directories will be affected
   - **Testing Strategy**: Unit, integration, end-to-end testing needs
   - **Documentation Updates**: What documentation needs updating
   - **Migration Requirements**: Database or configuration changes needed
   
   ## References
   - **Steering Documents**: List of steering docs consulted
   - **Linear Ticket**: {linear-ticket-url-if-available}
   - **External References**: APIs, libraries, documentation
   - **Related Issues**: Connected features or dependencies
   ```

### 8. **Save the File**
   - Create the issues directory if it doesn't exist: `mkdir -p issues`
   - Save the issue document as `issues/{ticket_id}-issue.md`
   - Ask the user if they want to add the `issues/` directory to `.git/info/exclude`:
     ```
     Do you want to add the `issues/` directory to `.git/info/exclude` to keep 
     issue documents local and out of version control?
     
     - "yes" - Add issues/ to .git/info/exclude for local-only exclusions
     - "no" - Skip git exclusion (issue documents may be committed)
     ```
     
     **If user says "yes":**
     - Use `.git/info/exclude` for local-only exclusions (not shared with the team)
     - Add "issues/" to `.git/info/exclude` if it's not already there
     - Use commands like: `echo "issues/" >> .git/info/exclude` (but check for duplicates first)
     
     **If user says "no":**
     - Continue without adding git exclusion
     - Inform user that issue documents may be committed to the repository
   - Ensure proper file permissions and formatting

### 9. **Final Output**
   - Present the complete issue document content in `<issue_document>` tags
   - Include the full file path where it will be saved
   - **Include steering document consultation summary**
   - Do not include any explanations or notes outside of these tags in your final output

## Important Notes

- **STEERING INTEGRATION**: Always read relevant steering documents to inform issue creation
- **LINEAR INTEGRATION**: When ticket IDs are provided, fetch Linear context to enhance issue quality
- **ARCHITECTURE AWARENESS**: Issues should reflect existing architectural patterns and constraints
- **PLAN OPTIMIZATION**: Create issues with sufficient detail for accurate `/plan` task breakdown
- **TECHNICAL DEPTH**: Include technical requirements that enable direct implementation planning
- **BUSINESS CONTEXT**: Maintain clear connection between business value and technical implementation (enhanced by Linear context when available)
- **CONSISTENCY**: Ensure issue aligns with established patterns and standards from steering documents

## Error Handling

**If steering documents missing:**
- Inform user that steering documents don't exist
- Suggest running `/steering-setup` first for better issue quality
- Continue with basic issue creation but note limitations

**If Linear MCP unavailable or ticket not found:**
- Continue with standard issue creation process
- Log the ticket ID for future reference
- Note in the issue document that Linear context was not available

**If feature description unclear:**
- Ask clarifying questions about business context
- Request technical requirements if missing
- Ensure sufficient detail for implementation planning

## Example Usage

```bash
/create-issue "add OAuth login support" --ticket-id PG-123
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/other-work → Switch to main and update
# 3. Fetches Linear ticket PG-123 details via MCP
# 4. Loads steering: security-architecture.md, api-design-guidelines.md, integration-patterns.md
# 5. Analyzes OAuth requirements against existing auth patterns + Linear context
# 6. Creates comprehensive issue with architectural context + Linear ticket reference
# 7. Includes implementation hints for /plan command

/create-issue "optimize database queries for user dashboard"
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Current branch: master → Already on correct branch
# 3. Loads steering: database-conventions.md, performance-standards.md, system-overview.md
# 4. Analyzes performance requirements against existing patterns
# 5. Asks user for ticket ID or generates descriptive one (e.g., "optimize-user-dashboard-queries")
# 6. Presents comprehensive plan with steering document influence details
# 7. Waits for user confirmation before creating detailed issue
# 8. Ready for accurate /plan task breakdown

/create-issue "implement user profile page" --ticket-id PG-456
# 1. Fetches Linear ticket PG-456 → Gets title, description, priority, assignee
# 2. Uses Linear context: "User Profile Enhancement - Add avatar upload and bio editing"
# 3. Cross-references Linear requirements with local steering documents
# 4. Creates issue with Linear metadata and URL reference
# 5. Enhanced business context from Linear ticket description
```

## What Happens After This Command?

**Issue Document Created:**
- **Steering-informed issue** - Contains architectural context and technical requirements
- **Implementation ready** - Sufficient detail for accurate `/plan` task breakdown
- **Standards compliant** - Follows existing patterns and coding guidelines
- **Business aligned** - Clear connection between business value and technical work

**Next Steps:**
- **Clear your context** - Use `/clear` to start fresh for planning
- **Create implementation plan** - Use `/plan {ticket-id}` to break down the work
- **Review the issue** - Check issues/{ticket-id}-issue.md anytime
- **Refine if needed** - Edit the issue file to add more details
