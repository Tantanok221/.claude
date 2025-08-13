# Work Command

Comprehensive development workflow that combines steering context and tech documentation for iterative or autonomous development.

## Usage

```bash
/work <description>
/work <description> --mode=iterative
/work <description> --mode=vibe  
/work <description> --focus=<area>
```

## Arguments

- **description**: Required. Task description, ticket ID (like "PG-123"), or feature request
- **--mode**: Optional. Choose workflow style:
  - `iterative` (default): Step-by-step approval workflow with micro-tasks
  - `vibe`: More autonomous execution with checkpoint reviews
- **--focus**: Optional. Focus area to prioritize relevant documentation:
  - `authentication`, `database`, `ui`, `api`, `testing`, `deployment`, `performance`, `security`

## Examples

```bash
/work "implement JWT authentication for user login"
/work "PG-123" --mode=vibe
/work "refactor payment processing" --focus=security --mode=iterative
/work "add dark mode toggle to settings page"
```

## Workflow

I'll help you work on "$ARGUMENTS" by:

1. **Context Gathering**: Using the steering-context-reader agent to gather relevant architectural guidance, coding standards, and business rules from your steering directory
2. **Tech Documentation**: Using the tech-doc-agent to fetch current best practices and documentation for any libraries or frameworks involved
3. **Process Selection**: If not specified, asking you to choose between:
   - **Iterative Mode**: Break work into small reviewable steps with approval before each change
   - **Vibe Mode**: More autonomous execution based on your high-level direction
4. **Implementation**: Executing the chosen workflow while maintaining consistency with your architectural patterns and current tech standards

Let me start by gathering the relevant context and documentation for your work.

## Step 1: Git Branch Management & Steering Context

First, I'll use the git-branch-manager agent to ensure you're on the correct Git branch for this work:

**Git Branch Manager Agent**
I'll use the git-branch-manager agent to ensure the correct Git branch is active for this work: "$ARGUMENTS"

**Context for Agent**: The expected branch pattern will be derived from the task description provided. The agent will determine the appropriate branch name and handle switching if needed.

**Agent Execution**: Wait for the git-branch-manager agent to complete its operation. If the agent indicates a failure (e.g., uncommitted changes conflict, Git operation failure), notify you of the specific issue and exit this command.

After branch management is complete, I'll use the steering-context-reader agent to find relevant guidance from your steering directory:

**Steering Context Reader Agent**
I need to gather relevant documentation and context from the /steering directory to help solve this specific problem: "$ARGUMENTS"

The context I'm looking for includes:
- Architectural patterns and constraints relevant to this work
- Coding standards and conventions that should be followed  
- Business domain knowledge that impacts implementation
- Technical constraints and requirements
- Any existing patterns or anti-patterns related to this type of work

Please provide focused, actionable context that will help ensure this work aligns with established standards and architecture.

## Step 2: Gathering Tech Documentation  

Next, I'll use the tech-doc-agent to get current technical documentation:

**Tech Documentation Agent**
I'm helping the user implement: "$ARGUMENTS"

I need current technical documentation and best practices for any libraries, frameworks, or technologies that will be involved in this implementation. Please provide:
- Current best practices and patterns
- API documentation and usage examples  
- Version-specific considerations
- Integration approaches and security considerations
- Implementation examples relevant to the work described

Focus on providing actionable, current information that will guide the implementation.

## Step 3: Process Mode Selection

If you didn't specify a mode, I'll ask:

**Choose Your Workflow Style:**

**Iterative Mode** (Recommended for complex changes):
- Break work into small, reviewable micro-tasks
- Present each proposed change before applying it
- Allow you to modify direction at each step
- Maximum control and visibility into each change

**Vibe Mode** (Good for experienced developers):  
- You provide high-level direction and constraints
- I make detailed implementation decisions autonomously
- Present larger chunks of completed work for review
- More efficient when you trust the AI to make good decisions

Which mode would you prefer for this work?

## Step 4: Implementation

Based on your chosen mode and the gathered context:

**Iterative Mode Implementation:**
- Present a detailed plan broken into micro-tasks
- Execute each task only after your approval
- Show you exactly what changes will be made before making them
- Allow course corrections and modifications at any step

**Vibe Mode Implementation:**
- Execute larger implementation chunks based on the gathered context
- Present completed work at logical checkpoints
- Focus on maintaining consistency with steering guidance and tech best practices
- Ask for direction when encountering ambiguous decisions

## Features

- **Context-Aware**: Automatically loads relevant steering docs and tech documentation
- **Resumable**: Can pick up work across multiple sessions
- **Git Integration**: Proper branch management and focused commits
- **Quality Assurance**: Ensures consistency with architectural patterns and current best practices
- **Flexible**: Switch between iterative and autonomous modes based on complexity and preference

## Error Handling

- **No Steering Directory**: Will proceed without steering context and inform you
- **No Tech Docs Found**: Will work with available information and notify you
- **Ambiguous Requests**: Will ask clarifying questions to ensure proper implementation

Ready to start working! Just provide your task description and any preferred options.