## Git Guidelines
- Dont take credit when commit and push code, only do if i specify

## MCP Server Integrations

### Linear MCP Integration
- Linear MCP is available for managing issues and projects
- Use mcp__linear-server__* functions to interact with Linear tickets
- Key functions:
  - mcp__linear-server__get_issue: Get issue details by ID
  - mcp__linear-server__list_issues: List issues with filters
  - mcp__linear-server__create_issue: Create new issues
  - mcp__linear-server__update_issue: Update existing issues
  - mcp__linear-server__list_comments: Get issue comments
  - mcp__linear-server__create_comment: Add comments to issues
  - mcp__linear-server__list_my_issues: List issues assigned to current user
  - mcp__linear-server__list_issue_statuses: List available issue statuses
  - mcp__linear-server__get_issue_status: Get specific issue status details
  - mcp__linear-server__list_issue_labels: List available labels
  - mcp__linear-server__list_projects: List projects in workspace
  - mcp__linear-server__get_project: Get specific project details
  - mcp__linear-server__create_project: Create new projects
  - mcp__linear-server__update_project: Update existing projects
  - mcp__linear-server__list_project_labels: List project labels
  - mcp__linear-server__list_cycles: List team cycles
  - mcp__linear-server__list_teams: List teams in workspace
  - mcp__linear-server__get_team: Get specific team details
  - mcp__linear-server__list_users: List users in workspace
  - mcp__linear-server__get_user: Get specific user details
  - mcp__linear-server__get_document: Get Linear documents
  - mcp__linear-server__list_documents: List Linear documents
  - mcp__linear-server__search_documentation: Search Linear docs
- When ticket IDs like "PG-123" are mentioned, fetch context from Linear to enhance understanding

### Gemini CLI MCP Integration
- Enhanced AI capabilities through Google's Gemini models
- **ALWAYS use Gemini CLI when dealing with large codebase searching and analyzing**
- Available functions:
  - mcp__gemini-cli__ask-gemini: Query Gemini models with optional change mode and sandbox
  - mcp__gemini-cli__brainstorm: Generate creative ideas with various methodologies
  - mcp__gemini-cli__fetch-chunk: Retrieve cached response chunks
  - mcp__gemini-cli__timeout-test: Test timeout behavior
  - mcp__gemini-cli__ping: Echo/ping functionality
  - mcp__gemini-cli__Help: Get help information
- Supports different models (gemini-2.5-pro, gemini-2.5-flash)
- Change mode for structured code edits
- Sandbox mode for safe code execution
- Use @ syntax to include files (e.g., '@largefile.js explain what this does')
- Preferred for complex codebase analysis and large file processing

### Context7 MCP Integration
- Retrieve up-to-date documentation for libraries and frameworks
- Available functions:
  - mcp__context7__resolve-library-id: Convert library names to Context7 IDs
  - mcp__context7__get-library-docs: Fetch library documentation
- Supports major libraries and frameworks
- Always resolve library ID first before fetching docs
- Useful for getting current documentation and code examples

### Tech Doc Agent Integration
- **IMPORTANT**: Use tech-doc-agent automatically when you need current library/framework documentation
- **Automatic Triggers**: Whenever user mentions or you encounter:
  - Specific libraries or frameworks (React, Node.js, Express, etc.)
  - Version conflicts or compatibility issues
  - Migration tasks ("migrate from X to Y", "upgrade to", "switch from")
  - Integration problems (third-party APIs, SDKs, services)
  - Best practices for specific technologies
  - Implementation questions for specific APIs or libraries

### How to Use Tech Doc Agent Automatically
- **Always provide rich context** when calling tech-doc-agent:
  - What problem you're trying to solve
  - Current technical context (stack, versions, constraints)
  - Specific requirements or goals
  - Scope of work (implementation, design, debugging, migration)
  - Dependencies and related systems

- **Example Context to Pass**:
  ```
  "I'm helping the user implement JWT authentication in a Node.js Express application. 
  They currently use sessions and want to migrate to JWT. I need current best practices 
  for JWT libraries, security considerations, and migration strategies. The application 
  has existing user management and needs to maintain backward compatibility during transition."
  ```

- **Use tech-doc-agent proactively** - don't wait for user to ask for documentation
- **Focus requests** - ask for specific documentation sections relevant to the immediate problem
- **Prevent context rot** - rely on tech-doc-agent to provide focused, relevant information

