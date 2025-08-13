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
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the current branch is the project's master/main branch and is up-to-date, handling all branch checking and switching operations.
   
   **Context for Agent**: The agent will be configured to ensure the master/main branch is active and up-to-date for issue creation work.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Load and Analyze Steering Context**
   **Before creating the issue, understand the architectural landscape:**
   
   **Check for Steering Documents:**
   - Look for `steering/` directory
   - If steering documents don't exist, inform user and suggest running `/steering-setup`
   - If steering exists, proceed with steering-aware issue creation
   
   **Use Steering Context Reader Agent**: Use the steering-context-reader agent to gather relevant steering documentation for this feature.
   
   **Context for Agent**: "I'm creating an issue for '{feature_description}'. This feature appears to involve {identified-feature-areas}. I need relevant steering context to create a comprehensive issue that aligns with project standards, architectural patterns, and coding guidelines."
   
   **Agent Execution**: Wait for the steering-context-reader agent to return relevant steering context for this feature.

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
   - **Detect Library/Framework Keywords**: Analyze feature description for specific technologies, frameworks, libraries, integrations, or migration requirements that would benefit from current documentation research

### 4.5. **Library/Framework Documentation Research**
   **Use tech-doc-agent when feature involves specific technologies:**
   
   **Detection Keywords in Feature Description:**
   - Library/framework names: "React", "Express", "OAuth", "JWT", "Stripe", "GraphQL", "MongoDB", etc.
   - Integration patterns: "integrate with", "connect to", "add authentication", "payment processing"
   - Migration tasks: "migrate from", "upgrade to", "replace with", "modernize"
   - Technology-specific features: "API integration", "database migration", "authentication system"
   
   **Tech Doc Agent Integration Process:**
   ```
   IF feature description contains library/framework keywords:
   1. Use tech-doc-agent with comprehensive feature context
   2. Pass detailed context about the feature requirements
   3. Receive focused documentation analysis for issue planning
   4. Integrate findings into issue document structure
   ```
   
   **Context to Provide to Tech Doc Agent:**
   ```
   "I'm creating an issue for: {feature_description}
   
   **Problem Context**: {what business problem this feature solves}
   **Technical Context**: {existing system architecture from steering docs}
   **Feature Requirements**: {functional and technical requirements}
   **Scope**: Issue creation phase - need current best practices and implementation guidance
   **Integration Needs**: {detected libraries, services, or frameworks involved}
   
   I need current documentation insights for: {detected technologies}
   Focus on: {implementation patterns, architectural considerations, integration approaches, security patterns}"
   ```
   
   **Tech Doc Agent Execution:**
   - Wait for tech-doc-agent to return focused documentation analysis
   - Integrate findings into issue requirements and implementation guidance
   - Include current best practices and architectural patterns
   - Reference version-specific considerations and integration approaches
   - Identify security and performance considerations for the technologies

### 5.5. **Issue Clarification and Requirements Brainstorm**
   **Interactive brainstorming process to clarify and enhance the user's feature description:**
   
   **Issue Clarification Initialization:**
   - Analyze the initial feature description for gaps, ambiguities, and missing context
   - Review gathered steering context and tech-doc-agent insights to identify clarification needs
   - Generate requirements-focused questions based on business and technical scope gaps
   
   **Use Gemini CLI Brainstorm MCP**: Use the mcp__gemini-cli__brainstorm tool to enhance issue clarity through iterative requirements gathering.
   
   **Context for Issue Brainstorm Tool:**
   ```
   "I'm creating a comprehensive issue document for the feature: {feature_description}
   
   **Initial Context**: {user-provided-feature-description}
   **Steering Insights**: {relevant-architectural-patterns-and-constraints}
   **Technology Context**: {tech-doc-agent-findings-if-applicable}
   **Project Context**: {existing-system-architecture-and-patterns}
   
   I need to brainstorm clarifying questions to create a comprehensive, well-defined issue document. The original description may be missing important details about:
   - Business context and value proposition
   - Technical requirements and constraints
   - User experience and workflow details
   - Integration points and dependencies
   - Success criteria and acceptance criteria
   - Edge cases and error scenarios
   
   Generate thoughtful clarifying questions that will help me gather the missing context and create a more comprehensive issue document that enables accurate design and implementation planning."
   ```
   
   **Iterative Issue Clarification Process:**
   
   **Initial Clarification Round:**
   - Call mcp__gemini-cli__brainstorm with comprehensive feature context
   - Present 3-5 most relevant clarifying questions to user based on brainstorm results
   - Focus on critical gaps in business context, technical scope, and user requirements
   
   **Issue Clarification Question Categories:**
   ```
   Business Context & Value:
   - "What specific business problem does this feature solve?"
   - "Who are the primary users/stakeholders that will benefit from this?"
   - "How will success be measured for this feature?"
   - "What's the business priority and expected timeline?"
   - "What's the potential impact if this feature is not implemented?"
   
   Technical Scope & Requirements:
   - "What specific functionality needs to be implemented?"
   - "Are there existing systems this needs to integrate with?"
   - "What are the performance, scalability, and availability requirements?"
   - "Are there security, compliance, or accessibility considerations?"
   - "What data will be created, modified, or accessed by this feature?"
   
   User Experience & Workflows:
   - "What's the expected user journey/workflow for this feature?"
   - "Are there different user roles with different needs or permissions?"
   - "What edge cases or error scenarios should be considered?"
   - "How should users be notified of changes, errors, or completion?"
   - "Are there mobile, desktop, or cross-platform considerations?"
   
   Implementation Context:
   - "Are there existing patterns in the codebase this should follow?"
   - "What systems, services, or databases will be affected?"
   - "Are there migration, backward compatibility, or versioning concerns?"
   - "What testing strategy should be considered (unit, integration, e2e)?"
   - "Are there deployment or infrastructure implications?"
   
   Dependencies & Constraints:
   - "Are there external dependencies (APIs, services, libraries) required?"
   - "What are the technical constraints or limitations to consider?"
   - "Are there team dependencies or coordination requirements?"
   - "What documentation or training materials will need to be updated?"
   ```
   
   **User Response Collection:**
   ```markdown
   Based on the feature description, I have these clarifying questions to create a comprehensive issue:
   
   📋 **Business & Requirements Questions:**
   1. {business-context-question-1}
   2. {user-value-question-2}
   3. {success-criteria-question-3}
   
   🔧 **Technical & Implementation Questions:**
   4. {technical-scope-question}
   5. {integration-or-constraint-question}
   
   Please provide additional context to help me create a more detailed and actionable issue document.
   You can also respond with:
   - "issue clear" - Sufficient detail provided for issue creation
   - "sufficient detail" - Proceed with current information
   - "more questions" - Ask additional clarifying questions
   ```
   
   **Follow-up Clarification Rounds (if needed):**
   - **Continue if user provides substantial additional context**: Generate follow-up questions based on new information
   - **Stop if user indicates sufficient clarity**: "issue clear", "sufficient detail", "proceed with creation"
   - **Stop if minimal new context**: Agent determines enough information gathered for comprehensive issue
   - **Maximum 3 clarification rounds**: Prevent user fatigue and infinite loops
   
   **Follow-up Question Generation:**
   - Use mcp__gemini-cli__brainstorm again with updated context including user responses
   - Focus on areas that still need clarification based on previous answers
   - Generate 2-3 targeted follow-up questions maximum per round
   
   **Issue Clarification Results Synthesis:**
   - Capture all user responses and requirements insights from brainstorm sessions
   - Identify business context and value proposition clarifications
   - Note technical requirements and constraints discovered
   - Document user experience and workflow details provided
   - Prepare enhanced feature context for issue document generation
   
   **Enhanced Issue Context Integration:**
   ```json
   {
     "issue_clarification_insights": {
       "business_context": "Clarified business problem and value proposition",
       "user_requirements": ["Requirement 1", "Requirement 2", "Requirement 3"],
       "technical_constraints": ["Constraint 1", "Constraint 2"],
       "success_criteria": ["Criteria 1", "Criteria 2"],
       "integration_needs": ["Integration 1", "Integration 2"],
       "user_workflows": ["Workflow 1", "Workflow 2"],
       "edge_cases": ["Edge case 1", "Edge case 2"],
       "user_clarifications": {
         "question": "user response",
         "follow_up": "additional context"
       }
     }
   }
   ```

### 6. **Handle Ticket ID and Linear Integration**
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

### 7. **Present Steering-Informed Plan**
   - Based on your research and steering analysis, outline a plan for creating the issue document
   - Include the proposed structure of the issue, the ticket ID, filename format, and how you'll incorporate project-specific conventions
   - **Show which steering documents influenced the plan**
   - Present this plan in `<plan>` tags
   - Include any reference links to feature base or other sources that have the source of the user request
   - Please ensure any references to local files with line numbers or repositories are included at the bottom of the issue document for future reference

### 8. **Create Comprehensive Issue Document**
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
   
   ## Library/Framework Context
   {include-if-tech-doc-agent-was-used}
   - **Technologies Involved**: {detected-libraries-and-frameworks}
   - **Current Best Practices**: {library-specific-recommendations-from-docs}
   - **Implementation Patterns**: {proven-integration-approaches-and-examples}
   - **Version Considerations**: {compatibility-requirements-and-breaking-changes}
   - **Integration Guidelines**: {sdk-usage-patterns-and-api-approaches}
   - **Security Considerations**: {library-specific-security-patterns-and-requirements}
   - **Performance Considerations**: {optimization-patterns-and-scalability-guidelines}
   
   ## Issue Clarification Insights
   {include-if-issue-brainstorm-was-conducted}
   - **Business Context**: {clarified-business-problem-and-value-proposition}
   - **User Requirements**: {detailed-user-needs-and-workflows-from-brainstorm}
   - **Technical Constraints**: {constraints-and-limitations-identified}
   - **Success Criteria**: {success-metrics-and-acceptance-criteria-clarified}
   - **Integration Needs**: {integration-points-and-dependencies-identified}
   - **Edge Cases**: {edge-cases-and-error-scenarios-discussed}
   - **User Clarifications**: {important-user-responses-and-additional-context}
   
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
   - **Tech Doc Agent Analysis**: {library-documentation-and-best-practices-if-used}
   - **Issue Clarification**: {brainstorm-insights-and-user-clarifications-if-conducted}
   - **External References**: APIs, libraries, documentation
   - **Related Issues**: Connected features or dependencies
   ```

### 9. **Save the File**
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

### 10. **Final Output**
   - Present the complete issue document content in `<issue_document>` tags
   - Include the full file path where it will be saved
   - **Include steering document consultation summary**
   - Do not include any explanations or notes outside of these tags in your final output

## Important Notes

- **STEERING INTEGRATION**: Always read relevant steering documents to inform issue creation
- **LINEAR INTEGRATION**: When ticket IDs are provided, fetch Linear context to enhance issue quality
- **TECH DOC AGENT INTEGRATION**: Use tech-doc-agent when features involve specific libraries, frameworks, or technologies to include current best practices and implementation patterns
- **ISSUE CLARIFICATION BRAINSTORM**: Use interactive brainstorming to clarify ambiguous feature descriptions and gather comprehensive requirements
- **ARCHITECTURE AWARENESS**: Issues should reflect existing architectural patterns and constraints
- **PLAN OPTIMIZATION**: Create issues with sufficient detail for accurate `/plan` task breakdown
- **TECHNICAL DEPTH**: Include technical requirements that enable direct implementation planning (enhanced by tech-doc-agent insights and issue clarification when applicable)
- **BUSINESS CONTEXT**: Maintain clear connection between business value and technical implementation (enhanced by Linear context and user clarifications when available)
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

**If tech-doc-agent unavailable or library documentation insufficient:**
- Continue with standard issue creation process using web research
- Note in the issue document that library-specific context was limited
- Include general best practices from web research instead of specific library documentation

**If feature description unclear:**
- Ask clarifying questions about business context
- Request technical requirements if missing
- Ensure sufficient detail for implementation planning

## Example Usage

```bash
/create-issue "add OAuth login support" --ticket-id PG-123
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/other-work → Switch to main and update
# 3. Loads steering: security-architecture.md, api-design-guidelines.md, integration-patterns.md
# 4. Research best practices for issue documentation
# 4.5. Detect keywords: "OAuth", "login" → tech-doc-agent triggered
#      Call tech-doc-agent: "creating issue for OAuth login support in Express.js app"
#      Tech-doc-agent returns: OAuth 2.0 patterns, Passport.js integration, JWT best practices
# 5.5. Issue clarification brainstorm: Ask questions about authentication scope, user flows, integration
#      User provides context: "Support Google/GitHub OAuth, single sign-on, mobile app integration"
#      Follow-up questions about security requirements and user management
#      User clarifies: "Need role-based access, session management, audit logging for compliance"
# 6. Fetches Linear ticket PG-123 details via MCP + Linear context
# 7. Analyzes OAuth requirements: steering + tech-doc-agent insights + Linear context + brainstorm clarifications
# 8. Creates comprehensive issue with library documentation context + Linear ticket reference + user clarifications
# 9. Includes current OAuth implementation patterns + clarified requirements for /plan command

/create-issue "optimize database queries for user dashboard"
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Current branch: master → Already on correct branch
# 3. Loads steering: database-conventions.md, performance-standards.md, system-overview.md
# 4. Research best practices for issue documentation
# 4.5. Detect keywords: "database", "queries", "optimize" → tech-doc-agent triggered
#      Call tech-doc-agent: "creating issue for database query optimization"
#      Tech-doc-agent returns: indexing strategies, query optimization patterns, monitoring tools
# 5.5. Issue clarification brainstorm: Ask questions about performance requirements, data volumes, user impact
#      User provides context: "Dashboard loads 50,000+ records, 2-3 second load times, affects all users"
#      Follow-up questions about caching and optimization priorities
#      User clarifies: "Redis caching available, prioritize initial load time, real-time updates needed"
# 6. Analyzes performance requirements: steering + tech-doc-agent insights + brainstorm clarifications
# 7. Asks user for ticket ID or generates descriptive one (e.g., "optimize-user-dashboard-queries")
# 8. Presents comprehensive plan with steering + library documentation + performance requirements
# 9. Creates issue with database optimization best practices + clarified performance context
# 10. Ready for accurate /plan task breakdown

/create-issue "integrate Stripe payment processing" --ticket-id PG-456
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: main → Already on correct branch
# 3. Loads steering: security-architecture.md, api-design-guidelines.md, payment-standards.md
# 4. Research best practices for issue documentation
# 4.5. Detect keywords: "Stripe", "payment processing" → tech-doc-agent triggered
#      Call tech-doc-agent: "creating issue for Stripe payment integration"
#      Tech-doc-agent returns: Stripe SDK patterns, webhook handling, security practices
# 5.5. Issue clarification brainstorm: Ask questions about payment flows, compliance, user experience
#      User provides context: "Support subscriptions and one-time payments, PCI compliance required"
#      Follow-up questions about refunds, failed payments, and international users
#      User clarifies: "Handle partial refunds, retry failed payments, support multiple currencies"
# 6. Fetches Linear ticket PG-456 via MCP + Linear context
# 7. Cross-references Linear requirements with steering docs + Stripe best practices + brainstorm insights
# 8. Creates issue with comprehensive integration guidance + Linear metadata + payment requirements
# 9. Includes current Stripe implementation patterns + security considerations + user clarifications
```

## What Happens After This Command?

**Issue Document Created:**
- **Steering-informed issue** - Contains architectural context and technical requirements
- **Library-enhanced issue** - Includes current best practices and implementation patterns for relevant technologies (via tech-doc-agent)
- **Requirements-clarified issue** - User-provided clarifications and comprehensive requirements captured through brainstorming
- **Implementation ready** - Sufficient detail for accurate `/plan` task breakdown
- **Standards compliant** - Follows existing patterns and coding guidelines
- **Business aligned** - Clear connection between business value and technical work

**Next Steps:**
- **Clear your context** - Use `/clear` to start fresh for planning
- **Create design document** - Use `/design {ticket-id}` to outline the technical solution
- **Review the issue** - Check issues/{ticket-id}-issue.md anytime
- **Refine if needed** - Edit the issue file to add more details
