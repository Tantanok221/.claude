# Steering Documents Setup Command

Create or update steering documents that provide persistent architectural and technical context for enterprise development.

## Usage
```
/steering-setup
```

## Instructions
You are setting up comprehensive steering documents that will provide persistent architectural and technical context for enterprise development. These documents serve as the foundational knowledge base that other commands will reference for context.

**ULTRATHINK MODE ACTIVATED**: This is a critical enterprise setup task that requires deep analysis and comprehensive thinking. Take your time to thoroughly examine every aspect of the codebase. Use as many tokens as necessary to provide exhaustive analysis and create the most comprehensive steering documents possible. Think deeply about architectural patterns, edge cases, business implications, and long-term maintainability. The quality of these documents will determine the success of all future development work.

**PURPOSE**: Create a complete steering document foundation that other commands can reference. This is a ONE-TIME setup process that establishes the knowledge base for the entire project.

**GEMINI MCP INTEGRATION**: You can use the Gemini MCP (Model Context Protocol) to help with complex analysis, pattern recognition, and comprehensive documentation generation. Leverage Gemini's capabilities for:
- Deep codebase analysis across multiple languages and frameworks
- Pattern recognition and architectural insight generation
- Business domain understanding from code structure
- Comprehensive documentation synthesis
- Cross-referencing and relationship mapping between system components

## Process

### 1. **Git Branch Management**
   **Use Git Branch Manager Agent**: Use the git-branch-manager agent to ensure the current branch is the project's master/main branch and is up-to-date, handling all branch checking and switching operations.
   
   **Context for Agent**: The agent will be configured to ensure the master/main branch is active and up-to-date for steering setup work.
   
   **Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify the user of the specific issue and exit this command.

### 2. **Check for Existing Steering Documents**
   - Look for `steering/` directory in project root
   - Check for existing documents in the core structure:
     ```
     steering/
     ├── architecture/
     ├── standards/
     ├── patterns/
     └── context/
     ```
   - **If steering documents already exist**:
     - List all existing documents found
     - Display message: "Steering documents already exist in this project. The setup process has been completed previously."
     - Ask: "What would you like to improve or update in the existing steering documents? Please specify:
       - Which document(s) need updates?
       - What specific aspects should be improved?
       - Are there new patterns or standards to document?
       - Has the architecture evolved since the last update?"
     - **STOP the setup process** and wait for user guidance on improvements
   
   - **Only proceed with full setup if NO steering documents exist**

### 2. **Analyze the Project (DEEP ANALYSIS MODE)**
   **Think ultrahard and use maximum available context. Use Gemini MCP for enhanced analysis.** Perform exhaustive codebase analysis:
   
   - **System Architecture Deep Dive**:
     - Components, services, microservices architecture
     - Data flow patterns and state management
     - Event-driven patterns, message queues, async processing
     - Caching layers, CDN usage, performance optimizations
     - Deployment architecture and infrastructure patterns
   
   - **Technology Stack Comprehensive Review**:
     - Primary and secondary languages, frameworks, libraries
     - Database technologies, ORM patterns, data modeling
     - Frontend/backend separation, API patterns
     - Development tools, build systems, CI/CD pipelines
     - Monitoring, logging, observability tools
   
   - **Code Organization & Patterns Analysis**:
     - Directory structure philosophy and naming conventions
     - Module organization, dependency injection patterns
     - Design patterns used (MVC, Repository, Factory, etc.)
     - Error handling strategies across the codebase
     - Configuration management and environment handling
   
   - **Business Domain Deep Understanding**:
     - Core business entities and their relationships
     - Workflow patterns and business process automation
     - Data validation rules and business constraints
     - User roles, permissions, and access patterns
     - Revenue models and business-critical features
   
   **Examine ALL relevant files with extreme thoroughness**:
     - All configuration files and their implications
     - Database migrations, schemas, and relationship patterns
     - API documentation, GraphQL schemas, OpenAPI specs
     - Docker configurations, deployment scripts
     - Testing frameworks and coverage patterns
     - Documentation files, ADRs (Architecture Decision Records)
     - Git history for architectural evolution patterns

### 3. **Present Comprehensive Inferred Details**
   **Provide exhaustive analysis results.** Show the user everything you've discovered:
   ```
   Based on my deep analysis, here's my comprehensive understanding:
   
   **System Architecture (Detailed):**
   - [Complete component architecture with relationships]
   - [Data flow patterns and state management strategies]
   - [Integration architecture and external dependencies]
   - [Performance and scalability patterns observed]
   - [Security architecture and authentication flows]
   
   **Technology Stack (Complete Inventory):**
   - [All languages, frameworks, and major libraries with versions]
   - [Database technologies and data storage patterns]
   - [Infrastructure tools and deployment technologies]
   - [Development toolchain and build processes]
   - [Monitoring, logging, and observability stack]
   
   **Coding Standards & Patterns (Comprehensive):**
   - [Naming conventions across all code types]
   - [Architectural patterns and design principles used]
   - [Code organization philosophy and module structure]
   - [Error handling, logging, and debugging patterns]
   - [Testing strategies and quality assurance approaches]
   
   **Business Domain (Deep Understanding):**
   - [Core business entities and their complex relationships]
   - [Business workflows and process automation patterns]
   - [Critical business rules and validation logic]
   - [User experience patterns and interface conventions]
   - [Revenue-critical features and business constraints]
   
   **Technical Constraints & Decisions:**
   - [Performance requirements and bottlenecks identified]
   - [Security requirements and compliance considerations]
   - [Scalability limitations and growth patterns]
   - [Technical debt areas and architectural evolution needs]
   ```
   
   - Ask: "I've provided a comprehensive analysis above. Please review each section carefully and let me know:
     - Which details are accurate and should be preserved?
     - What corrections or clarifications are needed?
     - What important aspects might I have missed?
     - Are there any sensitive details that should be excluded?"

### 4. **Gather Missing Information (Comprehensive Inquiry)**
   **Think deeply about gaps and ask detailed, probing questions.** Based on user feedback and comprehensive analysis, systematically gather missing information:
   
   **Architecture Deep-Dive Questions:**
   - What are the specific performance SLAs and how do they impact architectural decisions?
   - Are there disaster recovery requirements that affect system design?
   - How should the system handle peak load scenarios and auto-scaling?
   - What are the data retention policies and how do they affect storage architecture?
   - Are there specific compliance requirements (SOX, GDPR, HIPAA) that constrain architecture?
   - How should inter-service communication be handled in failure scenarios?
   
   **Standards Comprehensive Questions:**
   - Are there industry-specific coding standards that must be followed?
   - What are the code review processes and quality gates?
   - How should documentation be maintained and what level of detail is required?
   - What are the debugging and troubleshooting standards for production issues?
   - How should feature flags and configuration management be handled?
   - What are the accessibility and internationalization requirements?
   
   **Pattern Implementation Questions:**
   - What patterns have caused technical debt and should be avoided?
   - How should database transactions and data consistency be handled?
   - What are the established patterns for handling background jobs and queues?
   - How should third-party integrations be abstracted and tested?
   - What patterns exist for handling user authentication and authorization?
   - How should caching be implemented consistently across the application?
   
   **Business Context Deep Questions:**
   - What are the most business-critical features that require extra attention?
   - How do regulatory requirements impact feature development?
   - What are the key business metrics that development decisions should optimize for?
   - How do different user segments use the system differently?
   - What are the competitive differentiators that must be preserved?
   - How do seasonal or cyclical business patterns affect system requirements?

### 5. **Generate Steering Documents**
   Create `steering/` directory structure and generate documents:
   
   **Architecture Documents:**
   - `steering/architecture/system-overview.md` - High-level system architecture
   - `steering/architecture/data-flow.md` - Data flow and state management
   - `steering/architecture/integration-patterns.md` - External integrations
   - `steering/architecture/security-architecture.md` - Security patterns and requirements
   
   **Standards Documents:**
   - `steering/standards/coding-standards.md` - Code formatting, naming, structure
   - `steering/standards/api-design-guidelines.md` - API design patterns
   - `steering/standards/database-conventions.md` - Database design standards
   - `steering/standards/testing-standards.md` - Testing approaches and requirements
   
   **Pattern Documents:**
   - `steering/patterns/common-patterns.md` - Recommended implementation patterns
   - `steering/patterns/anti-patterns.md` - Patterns to avoid
   - `steering/patterns/decision-trees.md` - Decision guidance for common scenarios
   
   **Context Documents:**
   - `steering/context/business-domain.md` - Business rules and domain knowledge
   - `steering/context/technical-constraints.md` - Performance, security, compliance

   **IMPORTANT**: Create focused, actionable documents without versioning info, dates, or metadata. Include only technical content that directly helps during coding sessions.

   **Note**: Configuration file will be generated as the final step after all steering documents are created.

### 6. **Review and Confirm Each Document (Comprehensive Review)**
   **Take maximum time to create the highest quality documents possible.** For each document to be created:
   
   - **Present Complete Document Content**: Show the full, detailed content with comprehensive coverage
   - **Explain Document Purpose**: Clearly articulate how this document will guide future development
   - **Highlight Key Decisions**: Point out critical architectural or business decisions embedded in the document
   - **Show Interconnections**: Explain how this document relates to and references other steering documents
   - **Quality Validation**: Confirm the document covers all edge cases and scenarios relevant to its domain
   
   Ask: "I'm about to create `steering/[category]/[document-name].md` with this comprehensive content. Please review thoroughly:
   - Does this document provide sufficient detail for enterprise-level development guidance?
   - Are there any critical aspects missing that should be included?
   - Should any sections be expanded or clarified further?
   - Are there any business-sensitive details that should be removed or generalized?
   - Should I proceed with creating this document, or would you like modifications?"
   
   **Wait for explicit confirmation** before proceeding to the next document. Make any requested adjustments with the same level of thoroughness.

### 7. **Create Steering Configuration**
   **Create `steering/config.md` using this template:**
   ```markdown
   # Steering Configuration
   
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
   
   ## Command Integration
   - `/create-issue` reads relevant steering docs to create architecturally-aware issues
   - `/plan` loads steering docs based on issue type to create implementation plans  
   - `/continue` loads always-include docs + task-specific docs for each development session
   - All commands use this config.md to determine which steering docs to load when
   ```

### 8. **Update Git Exclusions**
   Ask the user if they want to add the `steering/` directory to `.git/info/exclude`:
   ```
   Do you want to add the `steering/` directory to `.git/info/exclude` to keep these 
   steering documents local and out of version control? 
   
   - "yes" - Add steering/ to .git/info/exclude for local-only exclusions
   - "no" - Skip git exclusion (steering documents may be committed)
   ```
   
   **If user says "yes":**
   - Add `steering/` to `.git/info/exclude` for local-only exclusions
   - Ensure steering documents stay out of version control
   - Confirm successful directory creation and exclusion setup
   
   **If user says "no":**
   - Continue without adding git exclusion
   - Inform user that steering documents may be committed to the repository

## Important Notes

- **MAXIMUM EFFORT REQUIRED**: This is enterprise-critical documentation that establishes the foundation for ALL future development commands. Use all available context and thinking capacity to create the most comprehensive and useful steering documents possible.
- **ONE-TIME SETUP**: This command only runs once per project. Future commands will READ these documents for context, not create them.
- **Steering documents are persistent** - they will be referenced by all future commands, so thoroughness now saves countless hours later
- **Each document has a specific purpose** - maintain clear separation of concerns while ensuring comprehensive coverage within each domain
- **Documents should be actionable** - provide clear, detailed guidance for implementation decisions with concrete examples
- **Think about edge cases** - Consider failure scenarios, scaling challenges, and complex business requirements
- **Keep business-sensitive information secure** - no credentials, internal URLs, or confidential data, but include all relevant technical and business context
- **Foundation for future evolution** - Other commands will handle updates and improvements to these documents

## HOW STEERING DOCUMENTS ARE USED

- **`/create-issue`** reads relevant steering docs to create architecturally-aware issues
- **`/plan`** loads steering docs based on issue type to create implementation plans
- **`/continue`** loads always-include docs + task-specific docs for each development session
- **All commands** use `config.md` to determine which steering docs to load when

## Document Templates

Each document should follow this general structure:
```markdown
# [Document Title]

## Purpose
[Why this document exists and how it should be used]

## [Main Content Sections]
[Specific guidance, patterns, or information]

## Implementation Guidelines
[Actionable steps and requirements]

## Examples
[Concrete examples where applicable]

## Related Documents
[Links to other steering documents]
```

## Example Flow

**New Project (No Existing Documents):**
1. Analyze React/Node.js enterprise application codebase thoroughly (using Gemini MCP for enhanced analysis)
2. Present comprehensive inferred architecture: microservices, REST APIs, PostgreSQL, etc.
3. User confirms architecture but clarifies authentication patterns
4. Ask detailed questions about API versioning standards and error handling approaches
5. Generate system-overview.md and present for confirmation
6. User approves, proceed to coding-standards.md
7. Continue through all 11 steering documents with user confirmation for each
8. Generate steering/config.md as final step based on created documents
9. User reviews and approves configuration
10. Create complete steering/ directory structure and save all approved documents
11. Update git exclusions and confirm setup completion

**Existing Project (Documents Found):**
1. Scan project and find existing steering documents in steering/architecture/ and steering/standards/
2. Display: "Steering documents already exist. Setup was completed previously."
3. Ask: "What would you like to improve? Architecture changes? New patterns? Updated standards?"
4. Wait for user guidance on specific improvements rather than full setup

## Next Steps

After steering documents are created, they will serve as the foundational knowledge base for other commands:
- **Issue Creation Commands** - Will reference steering documents for architectural context
- **Development Commands** - Will use steering documents to maintain consistency with established patterns
- **Review Commands** - Will validate against documented standards and principles
- **Update Commands** - Will help evolve steering documents as the project grows

**Note**: Other commands will automatically read and reference these steering documents. This setup command only needs to be run ONCE per project to establish the knowledge foundation.
