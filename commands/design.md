# Design Command

Create comprehensive, steering-aware design documents that outline the technical solution for a given issue, bridging the gap between requirements and implementation.

## Usage
```
/design <ticket-id>
/design <feature-description>
/design
```

**Note**: When no parameters are provided, the command will use cached session data from previous work in this project, or prompt you to specify what to work on.

## Instructions
You are creating well-structured design documents that integrate with the steering-aware development workflow. These designs will be consumed by the `/plan` command for implementation task breakdown, so they must contain sufficient technical detail, architectural context, and solutioning guidance.

**ULTRATHINK MODE ACTIVATED**: This is critical design creation that requires deep analysis of both the feature requirements and existing architectural context. Use maximum tokens to thoroughly understand the feature, analyze relevant steering documents, and create the most comprehensive and robust design possible. Think deeply about implementation approaches, architectural patterns, data models, API specifications, security, performance, and how this feature fits into the existing system.

**PURPOSE**: Create steering-aware designs that enable accurate implementation planning, maintain architectural consistency, and mitigate risks throughout the development workflow.

## Process

### 1. **Git Branch Management**
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the correct Git branch is active for design work, handling all branch checking and switching operations.
   
   **Context for Agent**: The expected branch pattern will be derived from the `ticket-id` or `feature-description` provided to this command. The agent will determine the appropriate branch name and handle switching if needed.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Session Cache & Work Target Identification**
   **Handle session caching and work target identification:**
   
   **Session Cache Logic:**
   - If ticket-id or feature-description provided: Use the provided parameters
   - If no parameters provided: **Use Session Cache Agent** to check for cached session data
     - **Context for Agent**: "I need to check session cache for the design command. No parameters were provided. Please check for existing `.claude-session-cache.json` and return the cached work target or indicate if user input is needed."
     - **Agent Execution**: Wait for the session-cache-agent to return cached values or prompt for user input
   - If user provides nothing: Exit with "No work specified"
   
   **Work Target Identification (Issue Document):**
   
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

### 4. **Load Steering Context for Design**
   **Use Steering Context Reader Agent**: Use the steering-context-reader agent to gather relevant steering documentation for designing this specific issue.
   
   **Context for Agent**: "I'm designing a technical solution for {ticket-id}: {issue-title-and-description}. This is a {issue-type} that involves {identified-keywords-from-issue-analysis}. I need relevant steering context to create a comprehensive design that follows project standards, architectural patterns, and coding guidelines."
   
   **Agent Execution**: Wait for the steering-context-reader agent to return relevant steering context for this issue.

### 5. **Library Documentation Context via Tech Doc Agent**
   **Use tech-doc-agent for Context7 MCP integration when libraries/frameworks are detected:**
   
   **Detection Keywords:**
   - Tech migration: "migrate from", "upgrade to", "switch from", "replace with", "migration", "modernize"
   - Package/library: "add dependency", "new library", "package", "framework", "SDK", "API client"
   - Version changes: "update version", "upgrade", "downgrade", "latest version"
   - Integration: "integrate with", "connect to", "API integration", "third-party service"
   
   **Tech Doc Agent Integration Process:**
   ```
   IF issue contains tech migration OR package-related keywords:
   1. Use tech-doc-agent with rich contextual information
   2. Pass comprehensive context about the design problem
   3. Receive focused documentation analysis and recommendations
   4. Integrate tech-doc-agent findings into design document
   ```
   
   **Context to Provide to Tech Doc Agent:**
   ```
   "I'm designing a technical solution for {ticket-id}: {issue-title-and-description}. 
   
   **Problem Context**: {brief description of what needs to be designed}
   **Technical Context**: {current tech stack, versions, known constraints}
   **Specific Requirements**: {libraries/frameworks mentioned, migration goals, integration needs}
   **Scope**: Design phase - need architectural guidance and best practices
   **Dependencies**: {other systems or libraries that might be affected}
   
   I need current documentation insights for: {list of detected libraries/frameworks}
   Focus on: {specific areas like migration paths, API patterns, architectural considerations}"
   ```
   
   **Tech Doc Agent Execution:**
   - Wait for tech-doc-agent to return focused documentation analysis
   - Integrate findings into design recommendations and technical approach
   - Reference specific library versions and compatibility considerations
   - Include migration strategies and modern implementation patterns
   
   **Design Enhancement with Tech Doc Agent Results:**
   - Include library-specific architectural patterns from current docs
   - Reference official migration guides and up-to-date best practices
   - Identify current version compatibility requirements
   - Suggest modern alternatives and implementation approaches based on latest documentation

### 6. **Comprehensive Context Analysis for Design**
   **Synthesize issue + steering context + tech-doc-agent insights (enhanced by design brainstorm in step 6.5) to inform the technical design:**
   
   **Issue Understanding:**
   - What needs to be designed to implement/fix the issue?
   - What are the core functional and non-functional requirements?
   
   **Architectural Guidance:**
   - Which existing architectural patterns apply?
   - Are new components or services required?
   - How will data flow through the system?
   - What are the integration points and dependencies?
   
   **Technical Solutioning:**
   - What are the proposed technical approaches?
   - What data model changes are needed?
   - How will APIs be designed and exposed?
   - What are the security and performance considerations?
   - What are the trade-offs of different design options?
   
   **Tech Doc Agent-Informed Decisions:**
   - Latest library/framework recommendations from official docs via tech-doc-agent
   - Migration strategies and compatibility considerations from current documentation
   - Modern patterns and best practices for new integrations
   - Version-specific features and breaking changes
   
   **Risk Assessment:**
   - Potential design challenges or technical debt
   - Areas that need special attention or further investigation
   - Dependencies that could cause issues
   - Testing strategies needed to validate the design

### 6.5. **Design Brainstorm and Architecture Enhancement**
   **Interactive brainstorming process to enhance design decisions and architectural approach:**
   
   **Design Brainstorm Initialization:**
   - Analyze the gathered design context: issue requirements, steering guidance, and tech-doc-agent insights
   - Identify architectural decision points and design trade-offs needing clarification
   - Generate design-specific questions based on technical complexity and system impact
   
   **Use Gemini CLI Brainstorm MCP**: Use the mcp__gemini-cli__brainstorm tool to enhance design decisions through iterative architectural questioning.
   
   **Context for Design Brainstorm Tool:**
   ```
   "I'm designing a technical solution for {ticket-id}: {issue-title-and-description}.
   
   **Design Context**: {summary-of-issue-steering-and-tech-doc-context}
   **Issue Type**: {bug/feature/enhancement} requiring {detected-architectural-domains}
   **Steering Patterns**: {key-architectural-patterns-from-steering-docs}
   **Technology Insights**: {tech-doc-agent-findings-if-applicable}
   **Complexity Indicators**: {preliminary-design-complexity-assessment}
   
   I need to brainstorm architectural approaches, design decisions, and technical trade-offs to create the most robust and well-considered technical design. Focus on identifying:
   - Architectural approach alternatives and trade-offs
   - Technology choice justifications and implications
   - Integration patterns and system boundaries
   - Scalability and performance design considerations
   - Security and data protection architectural decisions
   - Risk mitigation strategies and fallback approaches
   
   Generate thoughtful design questions that will help me make better architectural decisions and create a more comprehensive technical design."
   ```
   
   **Iterative Design Brainstorm Process:**
   
   **Initial Design Brainstorm Round:**
   - Call mcp__gemini-cli__brainstorm with comprehensive design context
   - Present 3-5 most relevant architectural questions to user based on brainstorm results
   - Focus on critical design decisions that affect system architecture and technical approach
   
   **Design Question Categories by Domain:**
   ```
   Architecture/Systems Design:
   - "Should this be implemented as a microservice or integrated into existing monolith?"
   - "What are the component boundaries and how should services communicate?"
   - "How should we handle service discovery and load balancing?"
   
   Data Design & Storage:
   - "What consistency model is required (eventual vs strong consistency)?"
   - "Should we use relational, document, or graph database patterns?"
   - "How should data migration and schema evolution be handled?"
   
   API Design & Integration:
   - "Should this expose REST, GraphQL, or RPC interfaces?"
   - "How should API versioning and backward compatibility be managed?"
   - "What authentication and rate limiting strategies are needed?"
   
   Performance & Scalability:
   - "What are the expected throughput and latency requirements?"
   - "How should caching be implemented (Redis, CDN, application-level)?"
   - "What horizontal scaling patterns should be designed for?"
   
   Security & Compliance:
   - "What authentication flows and authorization models are required?"
   - "How should sensitive data be encrypted at rest and in transit?"
   - "Are there compliance requirements (GDPR, HIPAA, SOX) affecting design?"
   
   Technology Choices & Migration:
   - "What frameworks and libraries best fit our architectural patterns?"
   - "How should we handle migration from legacy systems?"
   - "What deployment and infrastructure patterns should be designed for?"
   
   Risk & Resilience:
   - "How should the system handle failure scenarios and graceful degradation?"
   - "What monitoring and observability should be built into the design?"
   - "How should we handle rollback and disaster recovery scenarios?"
   ```
   
   **User Response Collection:**
   ```markdown
   Based on the analysis, I have these key design questions for better architecture:
   
   🏗️ **Architectural Approach Questions:**
   1. {architecture-specific-question-1}
   2. {system-design-question-2}
   3. {integration-pattern-question-3}
   
   ⚙️ **Technical Design Considerations:**
   4. {technology-choice-question}
   5. {performance-or-security-question}
   
   Please provide your design preferences and constraints to help me create a more robust technical design.
   You can also respond with:
   - "skip design brainstorm" - Skip to design document creation
   - "design approach clear" - Sufficient architectural context provided
   - "next design questions" - Ask different or follow-up design questions
   ```
   
   **Follow-up Design Brainstorm Rounds (if needed):**
   - **Continue if user provides substantial design context**: Generate follow-up questions based on architectural preferences
   - **Stop if user indicates sufficient design clarity**: "skip design brainstorm", "design approach clear", "proceed"
   - **Stop if minimal new design context**: Agent determines enough architectural information gathered
   - **Maximum 3 brainstorm rounds**: Prevent user fatigue and infinite loops
   
   **Follow-up Design Question Generation:**
   - Use mcp__gemini-cli__brainstorm again with updated context including user design preferences
   - Focus on architectural areas that need clarification based on previous answers
   - Generate 2-3 targeted design follow-up questions maximum per round
   
   **Design Brainstorm Results Synthesis:**
   - Capture all user responses and architectural insights from brainstorm sessions
   - Identify design approaches and technical architecture decisions revealed
   - Note technology choices and integration patterns clarified
   - Document performance, security, and scalability requirements discovered
   - Prepare enhanced architectural context for design document generation
   
   **Enhanced Design Context Integration:**
   ```json
   {
     "design_brainstorm_insights": {
       "architectural_approach": "Clarified system architecture based on user preferences",
       "technology_choices": ["Technology 1", "Technology 2", "Technology 3"],
       "design_constraints": ["Constraint 1", "Constraint 2"],
       "performance_requirements": ["Requirement 1", "Requirement 2"],
       "security_considerations": ["Security 1", "Security 2"],
       "integration_patterns": ["Pattern 1", "Pattern 2"],
       "risk_mitigation_strategies": ["Strategy 1", "Strategy 2"],
       "user_design_preferences": {
         "question": "user architectural preference",
         "follow_up": "design clarification"
       }
     }
   }
   ```

### 8. **Present Design Plan for Review & Iteration**
   **CRITICAL: DO NOT CREATE ANY FILES - This is an iterative design phase**
   
   **Show Complete Design Document Content for Review:**
   ```markdown
   📐 Here's the proposed design for review:
   
   **Planned File: `design/{ticket-id}-design.md`**
   
   ---
   
   # Design Document: {ticket-id} - {Issue Title}
   
   ## Issue Reference
   - Issue: issues/{ticket-id}-issue.md
   - Type: {bug/feature/enhancement}
   - Priority: {priority-level}
   
   ## High-Level Overview
   - [Brief summary of the proposed solution]
   - [Key components involved]
   
   ## Architectural Impact
   - [New services/components]
   - [Modified services/components]
   - [Diagram (conceptual, text-based)]
   
   ## Technical Design
   ### Data Model Changes
   - [Proposed schema modifications, new tables/collections]
   - [Relationships and constraints]
   
   ### API Specifications
   - [New endpoints, modified endpoints]
   - [Request/Response structures, authentication, error handling]
   
   ### Component Interactions
   - [Detailed flow between components]
   - [Message queues, eventing, direct calls]
   
   ### Technology Choices
   - [Specific libraries, frameworks, tools, versions]
   - [Justification for choices]
   - [Context7 MCP insights for library selection and migration paths]
   
   ## Security Considerations
   - [Authentication, authorization, data protection]
   - [Compliance with security-architecture.md]
   
   ## Performance & Scalability
   - [Expected load, response times]
   - [Caching strategies, concurrency]
   - [Compliance with technical-constraints.md]
   
   ## Testing Strategy
   - [Unit, integration, end-to-end testing approach]
   - [Key test cases, compliance with testing-standards.md]
   
   ## Trade-offs & Alternatives
   - [Discuss alternative designs considered and reasons for chosen approach]
   - [Known limitations or technical debt]
   
   ## Open Questions / Future Considerations
   - [Any unresolved design points]
   - [Potential future enhancements]
   
   ## Steering Document References
   - [List of steering docs that informed this design]
   
   ## Tech Doc Agent Analysis
   - [Libraries/frameworks analyzed via tech-doc-agent using Context7 MCP]
   - [Migration paths and compatibility insights from current documentation]
   - [Official documentation references and best practices]
   - [Context-specific recommendations based on the design requirements]
   
   ## Design Brainstorm Insights
   {include-if-design-brainstorm-was-conducted}
   - **Architectural Approach**: {clarified-system-architecture-from-brainstorm}
   - **Technology Choices**: {justified-technology-decisions-from-discussion}
   - **Design Constraints**: {constraints-and-requirements-clarified-with-user}
   - **Performance Requirements**: {performance-and-scalability-considerations-identified}
   - **Security Considerations**: {security-and-compliance-requirements-clarified}
   - **Integration Patterns**: {integration-and-communication-patterns-chosen}
   - **Risk Mitigation**: {risks-identified-and-mitigation-strategies-discussed}
   - **User Design Preferences**: {important-architectural-decisions-and-rationale}
   
   ---
   ```
   
   **Design Summary & Review Options:**
   ```markdown
   📐 Design document ready for review:
   
   **Design Summary:**
   - Addresses issue: {ticket-id}
   - Proposes: {brief summary of solution}
   - Aligns with: {key steering patterns}
   - Tech doc agent insights: {library/migration recommendations if applicable}
   - Design brainstorm insights: {architectural-decisions-and-user-preferences-if-applicable}
   
   **Review & Modify Options:**
   - "modify section X to [description]" - Change specific sections
   - "add section [name] with [content]" - Add new sections
   - "remove section X" - Remove specific sections
   - "clarify X" - Request more detail on a specific point
   - "show diagram for X" - Request a conceptual text-based diagram
   
   **When satisfied with the design:**
   - "create file" or "save the design" - Create the design/{ticket-id}-design.md file
   - "abort" - Cancel design session without creating file
   
   ⚠️ **Note: I will NOT create any files until you explicitly tell me to "create file" or "save the design"**
   ```
   
   **ENTER ITERATIVE DESIGN MODE - WAIT FOR USER MODIFICATIONS OR CREATION COMMAND**
   

### 9. **Iterative Design Mode**
   **Handle user modifications to the design document:**
   
   **For each user request to modify the design:**
   - Apply the requested changes to the design document structure
   - Re-present the complete updated design document for review
   - Continue iterating until user is satisfied
   - **DO NOT CREATE FILES** during this iterative process
   
   **Common modification patterns:**
   ```markdown
   User: "modify Data Model Changes to add 'user_preferences' table"
   → Update Data Model Changes section with new table details
   → Show updated complete design document
   → Wait for more modifications or creation command
   
   User: "add section 'Error Handling' with details on global error middleware"
   → Add new section with proposed error handling strategy
   → Show updated complete design document
   → Wait for more modifications or creation command
   
   User: "clarify Component Interactions for the authentication flow"
   → Expand on the authentication flow within Component Interactions
   → Show updated complete design document
   → Wait for more modifications or creation command
   ```

### 10. **Create Design Document File (Only After Explicit Command)**
   **Only when user says "create file" or "save the design":**
   
   **File Creation Process:**
   - Create directory: `design/` if it doesn't exist
   - Create file: `design/{ticket-id}-design.md`
   - Include all design document content from step 7
   - Add current timestamp and session information
   
   **Design Document File Features:**
   - **Comprehensive Solution**: Detailed technical approach
   - **Architectural Alignment**: References to steering documents
   - **Risk Mitigation**: Identifies trade-offs and open questions
   - **Input for Planning**: Provides clear guidance for `/plan`
   
   **Git Exclusion:**
   Ask the user if they want to add the `design/` directory to `.git/info/exclude`:
   ```
   Do you want to add the `design/` directory to `.git/info/exclude` to keep 
   design documents local and out of version control?
   
   - "yes" - Add design/ to .git/info/exclude for local-only exclusions
   - "no" - Skip git exclusion (design documents may be committed)
   ```
   
   **If user says "yes":**
   - Add `design/` to `.git/info/exclude`
   - Ensure steering/ and work-progress/ are also in `.git/info/exclude` (from previous setups)
   - Keep all design documents local and out of version control
   - Ensure clean git history
   
   **If user says "no":**
   - Continue without adding git exclusion
   - Inform user that design documents may be committed to the repository

### 11. **Design Output**
   **Provide design completion summary:**
   
   ```markdown
   # Design Complete: {ticket-id}
   
   ## Design Document Created
   - File: design/{ticket-id}-design.md
   - Status: Ready for planning
   
   ## Next Steps
   - Review design: Check design/{ticket-id}-design.md
   - Create implementation plan: `/plan {ticket-id}`
   - Modify design: Edit design document file manually
   
   ## Design Benefits
   - **Clear Solution**: Detailed technical approach for implementation
   - **Architectural Alignment**: Ensures consistency with project standards
   - **Risk Mitigation**: Identifies and addresses potential issues early
   - **Input for Planning**: Provides precise guidance for task breakdown
   - **Tech Doc Agent Integration**: Leverages up-to-date library docs and migration guides via Context7 MCP
   - **Modern Practices**: Incorporates latest best practices for tech migrations and integrations
   ```

### 12. **Update Session Cache & Optimization Notes**
   
   **Session Cache Update (After Successful Design):**
   **Use Session Cache Agent**: Use the session-cache-agent to update the session cache with current work details.
   
   **Context for Agent**: "I need to update the session cache for the design command. TicketId='{current-ticket-id}' featureDescription='{current-feature-description}' command='design'. Please update the session cache and ensure git exclusion."
   
   **Agent Execution**: Wait for the session-cache-agent to confirm cache update. Show "✓ Session cache updated for future /plan and /continue commands"
   
   **Optimization Notes:**
   - **Context Efficiency**: Only load steering documents directly relevant to the issue and design
   - **Prevent Context Rot**: Avoid loading unnecessary documentation  
   - **Smart Selection**: Use issue content to determine relevance
   - **Comprehensive Coverage**: Ensure all relevant guidance is included
   - **Session Continuity**: Cache successful design for seamless workflow resumption

## Important Notes

- **DESIGN ONLY**: This command creates technical design documents - NO actual implementation or task breakdown
- **SESSION PREPARATION**: Prepares for planning and session-based development workflow with `/plan` and `/continue` commands
- **EXTERNAL DESIGN DOCUMENT**: Creates persistent state file that survives across development sessions
- **STEERING REFERENCES**: Design document contains steering document references, not duplicate content
- **USER CONFIRMATION REQUIRED**: Never create design document files without explicit user approval  
- **CONTEXT EFFICIENCY**: Design document designed for minimal context loading in subsequent sessions
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
/design AUTH-123
# 1. Check git origin → GitHub detected → master branch is "main"
# 2. Current branch: feature/oauth-login → On suitable branch
# 3. Load: issues/AUTH-123-issue.md (Add OAuth login support)
# 4. Load relevant steering docs: security-architecture.md, api-design-guidelines.md, system-overview.md
# 5. Detect keywords: "OAuth", "authentication", "integrate with" → tech-doc-agent triggered
# 6. Call tech-doc-agent with context: "designing OAuth login system, need current best practices"
# 7. Tech-doc-agent: Fetch OAuth 2.0 docs, JWT patterns, returns focused analysis
# 6.5. Design brainstorm session: Ask questions about authentication flows, token management, user experience
#      User provides context: "Support multiple OAuth providers, need refresh tokens, mobile app integration"
#      Follow-up questions about security and scalability requirements
#      User clarifies: "Need PKCE for mobile, rate limiting for auth endpoints, audit logging"
# 8. Present proposed design with tech-doc-agent insights + brainstorm architectural decisions: OAuth flows, JWT structure, security best practices
# 9. User: "modify API Specifications to include JWT token structure"
# 10. Show updated design with JWT details from tech-doc-agent analysis + brainstorm insights
# 11. User: "add section 'Error Handling' with specific error codes"
# 12. Show updated design with error handling section
# 13. User: "create file"
# 14. Create design/AUTH-123-design.md with tech-doc-agent analysis + design brainstorm insights
# 15. Ready for: /plan AUTH-123

/design "migrate from React 16 to React 18"
# 1. Check git origin → GitLab detected → master branch is "master"
# 2. Search issues for "React 16" or "React 18" and present top matches for user selection
# 3. Detect keywords: "migrate from", "React 16", "React 18" → tech-doc-agent triggered
# 4. Call tech-doc-agent with context: "designing React 16 to 18 migration, need upgrade guidance"
# 5. Tech-doc-agent: Fetch React 18 migration guide, breaking changes, returns focused analysis
# 6. Load issue + relevant steering documents (ui-library-guidelines.md, coding-standards.md)
# 6.5. Design brainstorm session: Ask questions about migration strategy, compatibility, performance impact
#      User provides context: "Incremental migration, maintain compatibility, leverage concurrent features gradually"
#      Follow-up questions about testing and rollback strategies
#      User clarifies: "A/B test new features, automated testing for both versions, phased rollout"
# 7. Present proposed design with tech-doc-agent migration insights + brainstorm migration strategy: breaking changes, upgrade path, new features to leverage
# 8. User iterates on the design: "add concurrent features section", "clarify testing strategy for hooks"
# 9. User: "save the design"
# 10. Create design/react-migration-design.md with comprehensive tech-doc-agent analysis + design brainstorm insights
# 11. Ready for: /plan react-migration
```

## What Happens After This Command?

**Design Complete:**
- **Design document created** - External file with comprehensive technical solution
- **Branch prepared** - Correct git branch used for design work
- **Steering analyzed** - Relevant patterns and standards integrated into design
- **Design brainstorm captured** - Architectural decisions and user preferences integrated into design
- **Risks mitigated** - Trade-offs and open questions identified

**Ready for Planning:**
- **Clear your context** - Use `/clear` to start with fresh context
- **Start planning** - Use `/plan {ticket-id}` to break down the design into tasks
- **Review design** - Check design/{ticket-id}-design.md anytime
- **Refine if needed** - Edit the design file to add more details
