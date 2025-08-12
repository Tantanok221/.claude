# Validate Command

Comprehensive pre-code-review validation using LLM analysis to ensure code quality, consistency, and readiness for review.

## Usage
```
/validate <ticket-id> [--auto-fix]
/validate --current [--auto-fix]
/validate --branch <branch-name> [--auto-fix]
```

## Instructions
You are performing comprehensive validation of code changes before they go to code review. This command leverages steering documents, Gemini CLI MCP integration, and Linear context to ensure the highest quality code that follows all architectural patterns and business requirements.

**ULTRATHINK MODE ACTIVATED**: This is critical quality assurance that requires deep analysis. Use maximum tokens to thoroughly validate all aspects of the code changes, identify potential issues, and provide actionable recommendations. Think deeply about code quality, architectural compliance, testing coverage, and business requirements alignment.


**--auto-fix Mode Instructions**: When `--auto-fix` is used, automatically apply approved fixes for common issues like formatting, linting, simple refactoring, and documentation updates. Always show what will be fixed and get user approval before applying changes.

**PURPOSE**: Ensure code is production-ready and follows all architectural patterns, coding standards, and business requirements before entering the code review process.

## Process

### 1. **Git Branch Management & Target Identification**
   **Determine what to validate and ensure correct git context:**
   
   **Determine Master Branch:**
   - Check git origin URL: `git remote get-url origin`
   - If origin contains "gitlab" → master branch is "master"
   - If origin contains "github" → master branch is "main"
   - Default to "main" if unable to determine
   
   **Validation Target Identification:**
   ```bash
   # For ticket-id provided
   if ticket_id_provided:
     target_branch = "pg/tk-{ticket-id}"
     compare_against = master_branch
     validation_scope = "feature-branch"
   
   # For --current flag
   elif current_flag:
     target_branch = current_branch
     compare_against = master_branch
     validation_scope = "current-changes"
   
   # For --branch flag
   elif branch_provided:
     target_branch = provided_branch
     compare_against = master_branch
     validation_scope = "specified-branch"
   ```
   
   **Branch Validation Process:**
   ```bash
   # Check current branch matches target
   current_branch=$(git branch --show-current)
   
   if current_branch != target_branch and not current_flag:
     echo "✗ Not on target branch. Current: ${current_branch}, Expected: ${target_branch}"
     echo "Switch to correct branch? (y/n)"
     # Handle branch switching if user confirms
   else:
     echo "✓ On correct branch: ${current_branch}"
   ```
   
   **Change Detection:**
   ```bash
   # Get all changes for validation
   git fetch origin ${master_branch}
   changed_files=$(git diff origin/${master_branch}...HEAD --name-only)
   commit_count=$(git rev-list --count origin/${master_branch}...HEAD)
   
   echo "Validation scope: ${commit_count} commits, ${#changed_files[@]} files"
   ```

### 2. **Load Steering Context & Validation Rules**
   **Load steering configuration and validation-specific context:**
   
   **Load Steering Configuration:**
   - Read `steering/config.md` for always-include docs and feature mappings
   - If config.md doesn't exist, use default validation rules
   
   **Determine Validation Context:**
   ```
   Load always-include docs from config.md, then analyze changes to determine:
   IF changes involve API files → load api-design-guidelines.md
   IF changes involve database → load database-conventions.md, data-flow.md
   IF changes involve security → load security-architecture.md
   IF changes involve UI components → load ui-guidelines.md (if exists)
   IF changes involve integrations → load integration-patterns.md
   IF changes involve tests → load testing-standards.md
   IF changes involve configuration → load technical-constraints.md
   ```
   
   **Linear Ticket Context (if available):**
   - If ticket-id provided and follows Linear format (e.g., "PG-123"), fetch ticket details
   - Load requirements, acceptance criteria, and business context from Linear
   - Use Linear context to validate completeness against original requirements

### 3. **Comprehensive Code Quality Analysis**
   **Perform multi-layered quality analysis using Gemini CLI MCP when needed:**
   
   **Syntax & Linting Validation:**
   - Run project-specific linters and syntax checkers
   - Validate against steering document coding standards
   - Check for deprecated APIs or language features
   - Identify potential runtime errors and type issues
   
   **Code Style Consistency:**
   - Compare against `coding-standards.md` requirements
   - Validate naming conventions for variables, functions, classes
   - Check indentation, spacing, and formatting consistency
   - Verify import organization and dependency management
   
   **Performance Analysis:**
   - Identify potential performance bottlenecks
   - Check for anti-patterns from `anti-patterns.md`
   - Analyze database query efficiency (if database changes)
   - Validate caching strategies and resource usage
   
   **Security Vulnerability Scanning:**
   - Check for common security vulnerabilities
   - Validate input sanitization and output encoding
   - Review authentication and authorization patterns
   - Ensure compliance with `security-architecture.md` requirements
   
   **Use Gemini CLI MCP for complex analysis:**
   ```
   For large codebases or complex changes, utilize:
   mcp__gemini-cli__ask-gemini with changeMode=true for:
   - Cross-file dependency analysis
   - Complex architectural pattern validation
   - Business logic correctness verification
   - Integration impact assessment
   ```

### 4. **Architecture Compliance Validation**
   **Validate against steering document architectural patterns:**
   
   **System Architecture Compliance:**
   - Verify changes align with `system-overview.md` patterns
   - Check component boundaries and separation of concerns
   - Validate service integration patterns
   - Ensure consistency with existing architectural decisions
   
   **Data Flow Validation:**
   - Compare against `data-flow.md` established patterns
   - Check state management consistency
   - Validate event handling and async patterns
   - Ensure data validation and transformation consistency
   
   **API Design Compliance:**
   - Validate against `api-design-guidelines.md` standards
   - Check REST/GraphQL pattern consistency
   - Verify error handling and response format consistency
   - Validate versioning and backward compatibility
   
   **Integration Pattern Validation:**
   - Compare against `integration-patterns.md` requirements
   - Check external service interaction patterns
   - Validate retry logic and error handling
   - Ensure monitoring and observability compliance

### 5. **Testing Coverage & Quality Analysis**
   **Comprehensive testing validation:**
   
   **Test Coverage Analysis:**
   - Run coverage tools and analyze results
   - Compare against `testing-standards.md` requirements
   - Identify untested code paths and edge cases
   - Validate test quality and effectiveness
   
   **Test Implementation Quality:**
   - Review test organization and naming patterns
   - Check for proper mocking and test isolation
   - Validate integration test coverage
   - Ensure end-to-end test scenarios cover critical paths
   
   **Edge Case Coverage:**
   - Identify potential edge cases not covered by tests
   - Validate error handling test coverage
   - Check boundary condition testing
   - Ensure negative test case coverage

### 6. **Documentation Compliance Validation**
   **Ensure documentation standards are met:**
   
   **Code Documentation:**
   - Validate inline comments meet standards
   - Check function/method documentation completeness
   - Verify complex logic explanation quality
   - Ensure API documentation is up-to-date
   
   **Project Documentation Updates:**
   - Check if README updates are needed
   - Validate changelog entries for user-facing changes
   - Ensure migration guides are updated (if needed)
   - Verify configuration documentation accuracy
   
   **Architecture Decision Records:**
   - Check if new architectural decisions need documentation
   - Validate ADR format and completeness
   - Ensure decision context and alternatives are documented

### 7. **Git History & Commit Quality Validation**
   **Validate git history quality and standards:**
   
   **Commit Message Standards:**
   - Validate commit messages follow established patterns
   - Check for proper conventional commit format (if used)
   - Ensure commit messages are descriptive and clear
   - Validate ticket ID references in commit messages
   
   **Commit Organization:**
   - Check for logical commit separation
   - Identify commits that should be squashed
   - Validate commit size and scope appropriateness
   - Ensure no debugging or temporary commits remain
   
   **Branch Strategy Compliance:**
   - Validate branch naming follows conventions
   - Check for merge vs rebase compliance
   - Ensure feature branch is up-to-date with master
   - Validate no merge conflicts exist

### 8. **Business Requirements Validation**
   **Validate against business context and requirements:**
   
   **Linear Ticket Compliance (if available):**
   - Compare implementation against Linear ticket requirements
   - Validate all acceptance criteria are met
   - Check for scope creep or missing requirements
   - Ensure priority and timeline considerations are met
   
   **Business Domain Compliance:**
   - Validate against `business-domain.md` rules
   - Check business logic correctness
   - Ensure compliance with business constraints
   - Validate user experience considerations
   
   **Technical Constraint Compliance:**
   - Compare against `technical-constraints.md` requirements
   - Validate performance and scalability considerations
   - Ensure security and compliance requirements are met
   - Check resource usage and cost implications

### 9. **Generate Validation Report**
   **Create comprehensive validation report:**
   
   **Issue Classification:**
   ```markdown
   # Validation Report: {ticket-id or branch-name}
   
   ## Summary
   - **Status**: PASS/FAIL/WARNING
   - **Total Issues**: {count} (Critical: {n}, High: {n}, Medium: {n}, Low: {n})
   - **Auto-fixable Issues**: {count}
   - **Files Analyzed**: {count}
   - **Test Coverage**: {percentage}%
   
   ## Critical Issues (🔴)
   - Issue description with file:line reference
   - Recommended fix with code example
   - Business/technical impact explanation
   
   ## High Priority Issues (🟠)
   - Similar format for high priority issues
   
   ## Medium Priority Issues (🟡)
   - Similar format for medium priority issues
   
   ## Low Priority Issues (⚪)
   - Similar format for low priority issues
   
   ## Recommendations
   - Architectural improvements
   - Performance optimizations
   - Security enhancements
   - Documentation updates
   
   ## Code Review Readiness
   - ✅ Ready for review (if PASS)
   - ❌ Not ready - address critical/high issues first
   - ⚠️ Conditional - review with attention to specific areas
   ```

### 10. **Auto-Fix Mode Implementation**
   **When --auto-fix is enabled:**
   
   **Auto-fixable Issue Categories:**
   - Code formatting and style issues
   - Import organization and unused imports
   - Simple linting violations
   - Documentation formatting
   - Commit message formatting
   
   **Auto-fix Process:**
   ```markdown
   Found {count} auto-fixable issues:
   
   1. Fix trailing whitespace in {files}
   2. Organize imports in {files}
   3. Update documentation formatting in {files}
   4. Fix linting violations: {specific issues}
   
   Apply these fixes automatically? (y/n/selective)
   - y: Apply all fixes
   - n: Skip auto-fixes
   - selective: Choose which fixes to apply
   ```
   
   **Fix Application:**
   - Apply approved fixes automatically
   - Create micro-commits for each category of fixes
   - Show diff of changes made
   - Re-run validation to confirm fixes

### 11. **Present Results & Next Steps**
   **Final validation output:**
   
   ```markdown
   # Validation Complete: {target}
   
   ## Overall Status
   - **Code Review Ready**: YES/NO/CONDITIONAL
   - **Issues Found**: {total} ({breakdown by severity})
   - **Coverage**: {percentage}% test coverage
   - **Compliance**: {passing/failing} steering document compliance
   
   ## Next Steps
   - **If PASS**: Ready for code review and merge
   - **If FAIL**: Address critical and high-priority issues first
   - **If CONDITIONAL**: Review with special attention to flagged areas
   
   ## Quick Actions
   - Re-run validation: `/validate {same-params}`
   - Fix issues: Use provided recommendations
   - Continue development: `/continue {ticket-id}`
   - Create PR: Ready for pull request creation
   ```

## Important Notes

- **COMPREHENSIVE ANALYSIS**: Perform thorough validation across all quality dimensions
- **STEERING INTEGRATION**: Always validate against project-specific steering documents
- **ACTIONABLE FEEDBACK**: Provide specific, fixable recommendations with code examples
- **BUSINESS CONTEXT**: Consider business requirements and Linear ticket compliance
- **GEMINI MCP USAGE**: Leverage for complex codebase analysis and architectural validation
- **AUTO-FIX SAFETY**: Only auto-fix safe, reversible changes with user approval
- **VALIDATION BEFORE REVIEW**: Catch issues before human code review process

## Error Handling

**If steering documents missing:**
- Use default validation rules and patterns
- Inform user about missing steering context
- Suggest running `/steering-setup` for enhanced validation

**If branch/ticket not found:**
- List available branches or ticket IDs
- Suggest alternatives based on recent activity
- Allow manual branch specification

**If validation tools fail:**
- Continue with available validation methods
- Report tool failures in validation report
- Provide manual validation recommendations

## Example Usage

```bash
/validate AUTH-123
# 1. Switch to pg/tk-AUTH-123 branch
# 2. Load Linear ticket AUTH-123 context
# 3. Load security-architecture.md + api-design-guidelines.md steering docs
# 4. Run comprehensive validation on OAuth implementation
# 5. Generate detailed report with security focus
# 6. Show 3 critical issues, 5 high priority, 12 medium
# 7. Provide specific fix recommendations
# 8. Status: NOT READY - address critical security issues first

/validate --current --auto-fix
# 1. Validate current branch changes against master
# 2. Present formatting fixes and ask for approval before applying
# 3. Generate comprehensive report with detailed analysis
# 4. Status: CONDITIONAL - ready with attention to performance concern

/validate --branch feat/dashboard-updates
# 1. Switch to feat/dashboard-updates branch
# 2. Load UI and performance steering docs
# 3. Focus validation on frontend patterns and performance
# 4. Generate report with UI/UX compliance focus
# 5. Status: PASS - ready for code review
```

## Integration with Existing Workflow

**With /create-issue:**
- Validate implementations match issue requirements
- Check against Linear ticket acceptance criteria
- Ensure architectural decisions align with issue scope

**With /plan:**
- Validate completed tasks match planned implementation
- Check that all todo items from plan are properly implemented
- Ensure steering document compliance as planned

**With /continue:**
- Validate task completion before marking complete
- Check micro-commits meet quality standards
- Ensure incremental changes maintain overall quality

**Before Code Review:**
- Final quality gate before human review
- Catch issues that would be flagged in review
- Ensure consistent quality across team contributions