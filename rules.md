# Universal Development Rules with TDD/BDD Enforcement

Technology-agnostic development standards that enforce Test-Driven Development (TDD) and Behavior-Driven Development (BDD) practices for high-quality software development in any programming language or framework.

## FORGE Phase Integration

These rules apply across FORGE development phases:

| Phase | Rules Application |
|-------|------------------|
| **Focus** 🎯 | Define testable success criteria; identify quality constraints |
| **Orchestrate** 📋 | Plan test strategy; identify test data requirements |
| **Refine** ✏️ | Write acceptance criteria (Given-When-Then); enumerate edge cases - **NO CODE YET** |
| **Generate** ⚡ | Implement TDD (RED-GREEN-REFACTOR); write tests before code |
| **Evaluate** ✅ | Verify against criteria; test edge cases; measure coverage |

**Critical**: The Refine phase defines WHAT to test (specifications). The Generate phase implements HOW to test (TDD code).

## 🔴🟢🔵 Test-Driven Development (TDD) Requirements

### RED-GREEN-REFACTOR ENFORCEMENT
- **RED FIRST**: You MUST write failing tests before any implementation code
- **GREEN SECOND**: Write minimal code to make tests pass
- **REFACTOR THIRD**: Improve code while maintaining passing tests
- **NO CODE WITHOUT TESTS**: Zero tolerance for untested code

### TDD Implementation Rules
- **Test file creation**: Create test files before implementation files
- **Function testing**: Write test cases before implementing any function or method
- **Class testing**: Write test cases before implementing any class or module
- **API testing**: Write test cases before implementing any public interface
- **Test coverage**: Maintain minimum 80% test coverage (90% for critical components)

### BDD Specification Format
All tests MUST follow Given-When-Then format with business context:

```gherkin
Feature: User Authentication
  
  Scenario: Valid user login
    Given a user with valid credentials
    When they attempt to log in
    Then they should be authenticated successfully
    And they should receive an authentication token
    And they should be redirected to the dashboard
```

Test implementations should mirror this structure:
```
describe/context: "Feature: User Authentication"
  describe/context: "Given a user with valid credentials"
    describe/context: "When they attempt to log in"
      test/it: "Then should authenticate successfully"
      test/it: "Then should receive authentication token"
      test/it: "Then should be redirected to dashboard"
```

## 📋 Component Development Rules

### Modular Design Principles
- **Single Responsibility**: Each module/class/function has one clear purpose
- **Loose Coupling**: Minimize dependencies between modules
- **High Cohesion**: Related functionality grouped together
- **Interface Segregation**: Prefer small, focused interfaces
- **Dependency Inversion**: Depend on abstractions, not concretions

### Testing Requirements by Component Type

#### Functions/Methods
```
describe "calculateTotal function"
  given "valid numeric inputs"
    when "calculating total with tax"
      then "should return correct total amount"
      then "should handle decimal precision correctly"
      then "should validate input parameters"
  
  given "invalid inputs"
    when "null or undefined values provided"
      then "should throw appropriate error"
      then "should not modify any external state"
```

#### Classes/Objects
```
describe "UserService class"
  given "a new UserService instance"
    when "creating a new user"
      then "should validate user data"
      then "should generate unique identifier"
      then "should persist user information"
      then "should return created user object"
  
  given "invalid user data"
    when "attempting to create user"
      then "should reject with validation errors"
      then "should not persist invalid data"
```

#### APIs/Interfaces
```
describe "User Management API"
  given "authenticated admin user"
    when "requesting user list"
      then "should return paginated user data"
      then "should respect access permissions"
      then "should include proper metadata"
  
  given "unauthenticated request"
    when "accessing protected endpoint"
      then "should return 401 Unauthorized"
      then "should not leak sensitive information"
```

## 🏗️ Code Architecture Rules

### Universal Architecture Principles
- **Separation of Concerns**: Business logic separate from presentation and data layers
- **Don't Repeat Yourself (DRY)**: Extract common functionality into reusable components
- **Keep It Simple (KISS)**: Prefer simple solutions over complex ones
- **You Aren't Gonna Need It (YAGNI)**: Don't build functionality until it's needed
- **Composition over Inheritance**: Prefer composition and interfaces over deep inheritance hierarchies

### Directory Structure (Adapt to your language/framework)
```
project/
├── src/main/              # Main application code
│   ├── domain/           # Business logic and entities
│   ├── application/      # Use cases and orchestration
│   ├── infrastructure/   # External concerns (database, APIs, etc.)
│   └── presentation/     # User interface layer
├── src/test/             # Test code
│   ├── unit/            # Unit tests
│   ├── integration/     # Integration tests
│   └── acceptance/      # End-to-end/acceptance tests
└── docs/                # Documentation
```

## 🔒 Security Requirements

### Input Validation
- **Server-side validation**: Never trust client-side validation alone
- **Sanitization**: Sanitize all user inputs before processing
- **Parameterized queries**: Use parameterized queries to prevent injection attacks
- **Output encoding**: Encode output to prevent cross-site scripting

### Authentication & Authorization
- **Secure token storage**: Store authentication tokens securely
- **Session management**: Implement proper session timeout and renewal
- **Password requirements**: Enforce strong password policies
- **Multi-factor authentication**: Implement MFA for sensitive operations

### Data Protection
- **Encryption at rest**: Encrypt sensitive data in storage
- **Encryption in transit**: Use secure protocols for all communications
- **Data minimization**: Collect and store only necessary data
- **Audit logging**: Log all access to sensitive operations and data

### Database System Roles (Supabase/PostgreSQL)
*Platform-specific example — this document is otherwise tech-agnostic; apply
this section only when the project uses Supabase or a similar managed
PostgreSQL service.*

When using managed database services like Supabase, system roles are managed by the service provider and must NEVER be modified by user code:

**Reserved System Roles - NEVER TOUCH**:
- **`postgres`**: Default superuser role with admin privileges
- **`supabase_admin`**: Internal role for administrative tasks, upgrades, and automations  
- **`supabase_auth_admin`**: Used by Auth middleware, scoped to `auth` schema
- **`supabase_storage_admin`**: Used by Auth middleware, scoped to `storage` schema
- **`dashboard_user`**: For running commands via the service provider UI

**Migration Rules for System Roles**:
- **NEVER grant permissions TO these roles** - they are managed by the service provider
- **NEVER reference these roles** in custom functions, policies, or migrations
- **Use `service_role` instead** for elevated permissions in custom code
- **Use `authenticated` and `anon`** for standard Row Level Security policies
- **Contact service provider support** if system role issues occur - never attempt to fix manually

**Why This Matters**:
Modifying system roles can cause service failures, authentication breakdowns, and database connection issues that require service provider intervention to resolve.

## ♿ Accessibility Requirements

### Universal Design Principles
- **Perceivable**: Information must be presentable in multiple ways
- **Operable**: Interface must be usable by everyone
- **Understandable**: Information and UI operation must be clear
- **Robust**: Content must work with various assistive technologies

### Implementation Requirements
- **Alternative formats**: Provide alternatives for visual, audio, and interactive content
- **Keyboard accessibility**: All functionality available via keyboard
- **Clear navigation**: Consistent and predictable navigation patterns
- **Error handling**: Clear, actionable error messages and recovery options

### Testing Requirements
- **Automated testing**: Use accessibility testing tools in your language/framework
- **Manual testing**: Test with assistive technologies
- **User testing**: Include users with disabilities in testing processes
- **Standards compliance**: Meet WCAG 2.1 AA guidelines or equivalent standards

## 🎯 Performance Standards

### Response Time Requirements
- **Critical operations**: < 200ms response time
- **Standard operations**: < 1 second response time
- **Background operations**: < 5 seconds with progress indication
- **Batch operations**: Appropriate chunking and progress reporting

### Resource Management
- **Memory efficiency**: Prevent memory leaks, clean up resources
- **CPU efficiency**: Avoid blocking operations, use appropriate algorithms
- **Network efficiency**: Minimize requests, use caching appropriately
- **Storage efficiency**: Optimize data storage and retrieval

### Scalability Requirements
- **Horizontal scaling**: Design for distributed deployment
- **Vertical scaling**: Efficient resource utilization
- **Load handling**: Graceful degradation under load
- **Monitoring**: Comprehensive performance monitoring

## 🧪 Testing Requirements

### Test Categories (All Required)
1. **Unit Tests**: Test individual functions, methods, and classes
2. **Integration Tests**: Test component interactions
3. **Contract Tests**: Test API contracts and interfaces
4. **End-to-End Tests**: Test complete user workflows
5. **Performance Tests**: Test response times and resource usage
6. **Security Tests**: Test authentication, authorization, and data protection

### Test Quality Standards
- **Deterministic**: Tests must not be flaky or time-dependent
- **Fast**: Unit tests should complete quickly (< 10ms per test)
- **Independent**: Tests must not depend on execution order
- **Descriptive**: Test names clearly describe what is being tested
- **Maintainable**: Tests should be easy to understand and modify

### Mocking and Test Doubles
- **External dependencies**: All external services must be mocked
- **Databases**: Use in-memory databases or test fixtures
- **File systems**: Mock file operations for consistency
- **Network calls**: Mock all network interactions
- **Time dependencies**: Mock date/time functions for predictability

## 🔧 Development Workflow

### Version Control
- **Atomic commits**: Each commit represents one logical change
- **Conventional commits**: Use consistent commit message format
- **Trunk-based development (default)**: Commit directly to `main` in small
  (~15 min) TDD-driven increments; every commit leaves `main` releasable.
  Hide incomplete features via dark launch or branch-by-abstraction
  (feature flags as last resort). Review happens via pairing or post-commit
  review on trunk — not pre-merge PR gates. Only fall back to feature
  branches if the project explicitly overrides this default (e.g. it lacks
  fast CI, a strong test suite, or fast rollback).
- **Linear history**: Maintain clean, understandable history

### Continuous Integration
- **Automated testing**: All tests run on every commit
- **Coverage gates**: Prevent integration if coverage drops below threshold
- **Quality gates**: Automated code quality checking
- **Security scanning**: Automated vulnerability detection
- **Build verification**: Ensure code compiles and packages correctly

### Documentation
- **Code documentation**: Document complex algorithms and business rules
- **API documentation**: Document all public interfaces
- **Architecture documentation**: Document significant design decisions
- **User documentation**: Document user-facing features
- **Deployment documentation**: Document deployment and configuration procedures

## 🚫 Prohibited Practices

### Absolutely Forbidden
- **Implementing without tests**: No code without tests written first
- **Skipping code review**: All code changes must be reviewed
- **Committing secrets**: Never commit passwords, API keys, or sensitive data
- **Ignoring accessibility**: All features must be accessible
- **Breaking backward compatibility**: Without proper versioning and migration paths

### Code Smells to Avoid
- **God objects**: Classes or modules doing too much
- **Deep nesting**: Excessive nested structures (> 3-4 levels)
- **Magic numbers**: Use named constants instead of hardcoded values
- **Duplicate code**: Extract common functionality
- **Long functions**: Functions should be focused and concise
- **Tight coupling**: Minimize dependencies between modules

## 📊 Quality Metrics

### Coverage Requirements
- **Code Coverage**: Minimum 80% (90% for critical paths)
- **Branch Coverage**: Test all conditional branches
- **Path Coverage**: Test key execution paths
- **Mutation Coverage**: Verify test quality through mutation testing

### Complexity Limits
- **Cyclomatic Complexity**: Maximum 10 per function/method
- **Cognitive Complexity**: Keep code easy to understand
- **Nesting Depth**: Maximum 4 levels of nesting
- **Function Length**: Prefer shorter, focused functions
- **File Size**: Keep files manageable and focused

### Performance Benchmarks
- **Response Time**: Meet performance requirements for your domain
- **Resource Usage**: Monitor memory, CPU, and I/O consumption
- **Scalability**: Measure performance under load
- **Availability**: Target appropriate uptime for your service level

## 🚨 Enforcement Mechanisms

### Pre-commit Validation
- Run all tests before allowing commits
- Validate test coverage meets minimum threshold
- Check code quality metrics
- Verify no secrets are being committed
- Ensure proper commit message format

### Continuous Integration Pipeline
- **Build Stage**: Compile/package application
- **Test Stage**: Run all automated tests
- **Quality Stage**: Check code quality and coverage
- **Security Stage**: Run security scans
- **Deploy Stage**: Deploy only if all checks pass

### Code Review Requirements
- **Test Coverage**: Must meet minimum threshold
- **Code Quality**: Must meet complexity and maintainability standards
- **Documentation**: Public interfaces must be documented
- **Security**: Security implications must be reviewed
- **Performance**: Performance impact must be considered

## 📈 Continuous Improvement

### Regular Reviews
- **Weekly**: Review test failures and quality metrics
- **Monthly**: Review coverage trends and technical debt
- **Quarterly**: Review and update development standards
- **Annually**: Comprehensive process and tooling review

### Feedback Loops
- **Team retrospectives**: Regular team feedback sessions
- **Metrics analysis**: Data-driven process improvements
- **Tool evaluation**: Regular evaluation of development tools
- **Training**: Ongoing team skill development
- **Industry practices**: Stay current with industry best practices

### Adaptation Guidelines
- **Standards evolution**: Update standards based on experience and industry changes
- **Tool migration**: Gradually migrate to better tools and practices
- **Team feedback**: Incorporate team feedback into standard improvements
- **Performance optimization**: Continuously optimize development processes
- **Knowledge sharing**: Share learnings across teams and projects

---

**Important**: These rules are designed to be adapted to your specific technology stack, domain, and team needs while maintaining the core principles of quality, testability, security, accessibility, and maintainability.

**Technology Adaptation Examples**:
- **Python**: Use pytest, apply to Django/Flask/FastAPI
- **Java**: Use JUnit/TestNG, apply to Spring/Android
- **C#**: Use xUnit/NUnit, apply to .NET/Unity
- **Go**: Use built-in testing, apply to microservices
- **Rust**: Use built-in testing, apply to systems programming
- **Swift**: Use XCTest, apply to iOS development
- **Kotlin**: Use JUnit, apply to Android/server-side development