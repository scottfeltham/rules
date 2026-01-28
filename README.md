# 🚀 Universal Development Standards & TDD/BDD Enforcement System

A comprehensive, **technology-agnostic** set of development standards, coding practices, and accessibility guidelines that can be applied to **any software project**. These rules enforce Test-Driven Development (TDD), Behavior-Driven Development (BDD), and ensure high-quality, accessible, and maintainable code across **all programming languages and platforms**.

## 📦 What's Included

### Core Rule Files (100% Tech-Agnostic)
1. **`rules.md`** - Universal development rules with TDD/BDD enforcement for any language
2. **`coding-standards.md`** - Cross-language implementation patterns and best practices
3. **`accessibility-checklist.md`** - Platform-agnostic WCAG AA compliance guidelines

### Supporting Templates
*Note: Configuration examples are provided inline for each technology stack below*

## 🌍 Technology Coverage

### Programming Languages Supported
- **Compiled**: Java, C#, C++, Rust, Go, Swift, Kotlin, Scala
- **Interpreted**: Python, Ruby, JavaScript, PHP, Perl
- **Functional**: F#, Haskell, Clojure, Erlang, Elixir
- **Systems**: C, Assembly, WebAssembly
- **Mobile**: Swift (iOS), Kotlin/Java (Android), Dart (Flutter)
- **Web**: JavaScript/TypeScript, HTML/CSS

### Platforms & Frameworks Supported
- **Web**: React, Vue.js, Angular, Svelte, HTML5
- **Backend**: Node.js, Python (Django/Flask), Ruby on Rails, Java Spring, .NET, Go, Rust
- **Mobile**: iOS (Swift), Android (Java/Kotlin), React Native, Flutter, Xamarin
- **Desktop**: Electron, Qt, WPF, SwiftUI, JavaFX
- **CLI**: Bash, PowerShell, Python CLI, Go CLI, Rust CLI
- **Game Development**: Unity (C#), Unreal Engine (C++), Godot (GDScript)

## 🏁 Quick Start Installation

### For Any Project Type

#### Step 1: Copy Universal Rules
```bash
# Create .claude directory in your project root
mkdir -p .claude

# Copy the universal rule files (works for ANY language)
cp rules/rules.md .claude/
cp rules/coding-standards.md .claude/
cp rules/accessibility-checklist.md .claude/
```

#### Step 2: Language-Specific Setup

Choose your technology stack and follow the appropriate setup:

## 🎯 Technology-Specific Implementations

### JavaScript/TypeScript Projects

#### Web Applications (React, Vue, Angular)
```bash
# Install dependencies
npm install --save-dev jest @testing-library/dom @testing-library/user-event
npm install --save-dev @testing-library/react  # For React
npm install --save-dev @vue/test-utils  # For Vue
npm install --save-dev @angular/testing  # For Angular

# Create configuration files (examples provided below)
# Package.json scripts
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "lint": "eslint . --ext .ts,.tsx,.js,.jsx"
  }
}
```

#### Node.js Backend
```bash
# Install testing dependencies
npm install --save-dev jest supertest @types/supertest
npm install --save-dev eslint prettier husky

# Update jest.config.js
testEnvironment: 'node'
coveragePathIgnorePatterns: ['/node_modules/', '/dist/']
```

### Python Projects

#### Django/Flask Applications
```python
# requirements-dev.txt
pytest>=7.0
pytest-cov>=4.0
pytest-django>=4.5  # For Django
pytest-flask>=1.2   # For Flask
black>=22.0
flake8>=5.0
mypy>=1.0

# pytest.ini
[tool:pytest]
testpaths = tests
python_files = test_*.py *_test.py
python_classes = Test*
python_functions = test_*
addopts = --cov=. --cov-report=html --cov-report=term-missing --cov-fail-under=80

# Apply BDD format in Python
def test_user_authentication():
    """
    Feature: User Authentication
    Given a user with valid credentials
    When they attempt to log in
    Then they should be authenticated successfully
    """
    # Test implementation
```

#### FastAPI
```python
# Additional dependencies
pytest-asyncio>=0.20
httpx>=0.24  # For async client testing

# conftest.py
import pytest
from fastapi.testclient import TestClient
from httpx import AsyncClient
from app import app

@pytest.fixture
def client():
    return TestClient(app)

@pytest.fixture
async def async_client():
    async with AsyncClient(app=app, base_url="http://test") as ac:
        yield ac
```

### Java Projects

#### Spring Boot Applications
```xml
<!-- pom.xml dependencies -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
</dependency>

<!-- Maven Surefire Plugin for coverage -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.8</version>
    <configuration>
        <rules>
            <rule>
                <element>CLASS</element>
                <limits>
                    <limit>
                        <counter>LINE</counter>
                        <value>COVEREDRATIO</value>
                        <minimum>0.80</minimum>
                    </limit>
                </limits>
            </rule>
        </rules>
    </configuration>
</plugin>
```

```java
// BDD-style test example
@DisplayName("Feature: User Authentication")
class UserAuthenticationTest {
    
    @Nested
    @DisplayName("Given a user with valid credentials")
    class GivenValidCredentials {
        
        @Nested
        @DisplayName("When they attempt to log in")
        class WhenAttemptingLogin {
            
            @Test
            @DisplayName("Then should authenticate successfully")
            void shouldAuthenticateSuccessfully() {
                // Test implementation
            }
            
            @Test
            @DisplayName("Then should receive authentication token")
            void shouldReceiveAuthToken() {
                // Test implementation
            }
        }
    }
}
```

### C# .NET Projects

#### ASP.NET Core
```xml
<!-- .csproj test dependencies -->
<PackageReference Include="Microsoft.AspNetCore.Mvc.Testing" Version="7.0.0" />
<PackageReference Include="xunit" Version="2.4.2" />
<PackageReference Include="xunit.runner.visualstudio" Version="2.4.5" />
<PackageReference Include="FluentAssertions" Version="6.8.0" />
<PackageReference Include="Moq" Version="4.18.4" />

<!-- Coverage settings in Directory.Build.props -->
<PropertyGroup>
    <CollectCoverage>true</CollectCoverage>
    <CoverletOutputFormat>cobertura</CoverletOutputFormat>
    <Threshold>80</Threshold>
    <ThresholdType>line,branch,method</ThresholdType>
</PropertyGroup>
```

```csharp
// BDD-style test example
[Trait("Feature", "User Authentication")]
public class UserAuthenticationTests
{
    [Fact]
    [Trait("Scenario", "Given valid credentials When login Then success")]
    public async Task GivenValidCredentials_WhenAttemptingLogin_ThenShouldAuthenticateSuccessfully()
    {
        // Given
        var validCredentials = new LoginRequest("user@test.com", "password");
        
        // When
        var result = await _authService.LoginAsync(validCredentials);
        
        // Then
        result.Should().BeSuccessful();
        result.Token.Should().NotBeNullOrEmpty();
    }
}
```

### Ruby on Rails Projects

#### Rails Application Setup
```ruby
# Gemfile
group :development, :test do
  gem 'rspec-rails', '~> 6.0'
  gem 'factory_bot_rails', '~> 6.2'
  gem 'shoulda-matchers', '~> 5.3'
  gem 'database_cleaner-active_record', '~> 2.1'
  gem 'simplecov', '~> 0.22', require: false
  gem 'rubocop', '~> 1.50', require: false
  gem 'rubocop-rails', '~> 2.19', require: false
  gem 'rubocop-rspec', '~> 2.20', require: false
end

# .rspec
--require spec_helper
--format documentation
--color

# spec/spec_helper.rb
require 'simplecov'
SimpleCov.start 'rails' do
  minimum_coverage 80
  refuse_coverage_drop
end

RSpec.configure do |config|
  config.expect_with :rspec do |expectations|
    expectations.include_chain_clauses_in_custom_matcher_descriptions = true
  end
end
```

```ruby
# BDD-style test example
RSpec.describe 'User Authentication', type: :request do
  context 'given a user with valid credentials' do
    let(:user) { create(:user, email: 'user@test.com', password: 'password') }
    
    context 'when they attempt to log in' do
      before do
        post '/api/auth/login', params: { 
          email: user.email, 
          password: 'password' 
        }
      end
      
      it 'then should authenticate successfully' do
        expect(response).to have_http_status(:ok)
      end
      
      it 'then should receive authentication token' do
        expect(JSON.parse(response.body)['token']).to be_present
      end
    end
  end
end
```

### Go Projects

#### Go Backend Services
```go
// go.mod
module myproject

go 1.21

require (
    github.com/stretchr/testify v1.8.4
    github.com/golang/mock v1.6.0
    github.com/onsi/ginkgo/v2 v2.11.0  // For BDD-style tests
    github.com/onsi/gomega v1.27.8
)

// Makefile
.PHONY: test test-coverage
test:
	go test -v ./...

test-coverage:
	go test -v -coverprofile=coverage.out ./...
	go tool cover -html=coverage.out -o coverage.html
	@echo "Coverage must be >= 80%"
	go tool cover -func=coverage.out | grep total | grep -Eo '[0-9]+\.[0-9]+' | awk '{if ($$1 < 80.0) exit 1}'
```

```go
// BDD-style test example using Ginkgo
package auth_test

import (
    . "github.com/onsi/ginkgo/v2"
    . "github.com/onsi/gomega"
    "myproject/auth"
)

var _ = Describe("User Authentication", func() {
    Context("Given a user with valid credentials", func() {
        var user *auth.User
        
        BeforeEach(func() {
            user = &auth.User{Email: "user@test.com"}
        })
        
        Context("When they attempt to log in", func() {
            var result *auth.LoginResult
            var err error
            
            BeforeEach(func() {
                result, err = auth.Login("user@test.com", "password")
            })
            
            It("Then should authenticate successfully", func() {
                Expect(err).ToNot(HaveOccurred())
                Expect(result.Success).To(BeTrue())
            })
            
            It("Then should receive authentication token", func() {
                Expect(result.Token).ToNot(BeEmpty())
            })
        })
    })
})
```

### Rust Projects

#### Rust Applications
```toml
# Cargo.toml
[dev-dependencies]
tokio-test = "0.4"
mockall = "0.11"
rstest = "0.18"  # For parameterized tests
proptest = "1.2"  # For property-based testing

# Enable coverage
[profile.dev]
opt-level = 0
debug = true
overflow-checks = true

# .cargo/config.toml
[build]
rustflags = ["-C", "instrument-coverage"]

[env]
LLVM_PROFILE_FILE = "target/coverage/cargo-test-%p-%m.profraw"
```

```rust
// BDD-style test example
#[cfg(test)]
mod user_authentication_tests {
    use super::*;
    
    mod given_valid_credentials {
        use super::*;
        
        mod when_attempting_login {
            use super::*;
            
            #[tokio::test]
            async fn then_should_authenticate_successfully() {
                // Given
                let credentials = LoginCredentials {
                    email: "user@test.com".to_string(),
                    password: "password".to_string(),
                };
                
                // When
                let result = authenticate_user(credentials).await;
                
                // Then
                assert!(result.is_ok());
                assert!(result.unwrap().is_authenticated);
            }
            
            #[tokio::test]
            async fn then_should_receive_auth_token() {
                // Given
                let credentials = LoginCredentials {
                    email: "user@test.com".to_string(),
                    password: "password".to_string(),
                };
                
                // When
                let result = authenticate_user(credentials).await.unwrap();
                
                // Then
                assert!(!result.token.is_empty());
            }
        }
    }
}
```

### iOS (Swift) Projects

#### iOS Application Testing
```swift
// Package.swift or Xcode project
dependencies: [
    .package(url: "https://github.com/Quick/Quick.git", from: "7.0.0"),
    .package(url: "https://github.com/Quick/Nimble.git", from: "12.0.0")
]

// XCTestCase with BDD style
import XCTest
import Quick
import Nimble
@testable import MyApp

class UserAuthenticationSpec: QuickSpec {
    override func spec() {
        describe("User Authentication") {
            context("given a user with valid credentials") {
                var user: User!
                
                beforeEach {
                    user = User(email: "user@test.com")
                }
                
                context("when they attempt to log in") {
                    var result: LoginResult?
                    
                    beforeEach {
                        result = AuthService.shared.login(
                            email: "user@test.com", 
                            password: "password"
                        )
                    }
                    
                    it("then should authenticate successfully") {
                        expect(result?.isSuccess).to(beTrue())
                    }
                    
                    it("then should receive authentication token") {
                        expect(result?.token).toNot(beNil())
                        expect(result?.token).toNot(beEmpty())
                    }
                }
            }
        }
    }
}
```

### Android (Kotlin/Java) Projects

#### Android Application Testing
```kotlin
// build.gradle (app module)
android {
    testOptions {
        unitTests {
            isIncludeAndroidResources = true
        }
    }
}

dependencies {
    testImplementation 'junit:junit:4.13.2'
    testImplementation 'org.mockito:mockito-core:5.3.1'
    testImplementation 'org.robolectric:robolectric:4.10'
    testImplementation 'androidx.test:core:1.5.0'
    testImplementation 'org.jetbrains.kotlinx:kotlinx-coroutines-test:1.7.1'
    
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
}

// BDD-style test example
@RunWith(RobolectricTestRunner::class)
class UserAuthenticationTest {
    
    @Test
    fun `Feature User Authentication - Given valid credentials When login Then success`() {
        // Given - a user with valid credentials
        val credentials = LoginCredentials("user@test.com", "password")
        val authService = AuthService()
        
        // When - they attempt to log in
        val result = authService.login(credentials)
        
        // Then - should authenticate successfully
        assertThat(result.isSuccess).isTrue()
        assertThat(result.token).isNotEmpty()
    }
}
```

## 📋 Universal Implementation Checklist

### Phase 1: Foundation (Any Language)
- [ ] Copy universal rules to `.claude/` directory
- [ ] Set up testing framework for your language
- [ ] Configure coverage reporting (80% minimum)
- [ ] Implement BDD Given-When-Then test format
- [ ] Set up linting/formatting tools

### Phase 2: TDD Enforcement
- [ ] Create test validation script for your language
- [ ] Set up pre-commit hooks
- [ ] Configure CI/CD pipeline with quality gates
- [ ] Establish code review process
- [ ] Train team on TDD/BDD practices

### Phase 3: Quality Integration
- [ ] Integrate accessibility testing tools
- [ ] Set up security scanning
- [ ] Configure performance monitoring
- [ ] Implement documentation standards
- [ ] Establish metrics tracking

## 🌟 Cross-Language BDD Examples

### User Registration Feature

#### JavaScript
```javascript
describe('Feature: User Registration', () => {
  describe('Given valid user data', () => {
    describe('When creating account', () => {
      it('Then should create user successfully', () => {})
      it('Then should send welcome email', () => {})
    })
  })
})
```

#### Python
```python
def test_user_registration_with_valid_data():
    """
    Feature: User Registration
    Given valid user data
    When creating account
    Then should create user successfully
    """
    pass
```

#### Java
```java
@DisplayName("Feature: User Registration")
@Nested class GivenValidUserData {
    @Nested class WhenCreatingAccount {
        @Test void thenShouldCreateUserSuccessfully() {}
        @Test void thenShouldSendWelcomeEmail() {}
    }
}
```

#### C#
```csharp
[Trait("Feature", "User Registration")]
public class UserRegistrationTests {
    [Fact] public void GivenValidData_WhenCreating_ThenShouldSucceed() {}
}
```

#### Go
```go
var _ = Describe("User Registration", func() {
    Context("Given valid user data", func() {
        Context("When creating account", func() {
            It("Then should create successfully", func() {})
        })
    })
})
```

#### Rust
```rust
mod user_registration {
    mod given_valid_data {
        mod when_creating_account {
            #[test] fn then_should_create_successfully() {}
        }
    }
}
```

## 🚨 Universal Anti-Patterns

### ❌ What NOT to Do (Any Language)
- Writing implementation before tests
- Skipping BDD format in test descriptions  
- Ignoring accessibility requirements
- Not mocking external dependencies
- Allowing coverage below 80%
- Committing without code review
- Hardcoding secrets in code

### ✅ What TO Do (Any Language)
- Write failing tests first (RED)
- Make tests pass with minimal code (GREEN)
- Refactor while maintaining coverage (REFACTOR)
- Follow Given-When-Then format
- Mock all external dependencies
- Maintain accessibility standards
- Use meaningful commit messages

## 📚 Language-Specific Resources

### Documentation & Guides
- **JavaScript/TypeScript**: [Jest](https://jestjs.io/), [Testing Library](https://testing-library.com/)
- **Python**: [pytest](https://pytest.org/), [Django Testing](https://docs.djangoproject.com/en/stable/topics/testing/)
- **Java**: [JUnit 5](https://junit.org/junit5/), [Spring Boot Testing](https://spring.io/guides/gs/testing-web/)
- **C#**: [xUnit](https://xunit.net/), [ASP.NET Core Testing](https://docs.microsoft.com/en-us/aspnet/core/test/)
- **Ruby**: [RSpec](https://rspec.info/), [Rails Testing](https://guides.rubyonrails.org/testing.html)
- **Go**: [Testing Package](https://pkg.go.dev/testing), [Ginkgo BDD](https://onsi.github.io/ginkgo/)
- **Rust**: [Rust Testing](https://doc.rust-lang.org/book/ch11-00-testing.html)
- **Swift**: [XCTest](https://developer.apple.com/documentation/xctest), [Quick/Nimble](https://github.com/Quick/Quick)

### Testing Tools by Platform
- **Web**: Cypress, Playwright, WebdriverIO
- **Mobile**: Detox (React Native), XCUITest (iOS), Espresso (Android)
- **Desktop**: Appium, WinAppDriver, White
- **API**: Postman, Insomnia, Pact
- **Performance**: k6, Artillery, JMeter

## 🎯 Success Metrics (Universal)

### Technical Metrics
- **Coverage**: >80% line coverage, >75% branch coverage
- **Build Time**: <10 minutes for full test suite
- **Test Speed**: <5 seconds for unit tests
- **Flakiness**: <1% flaky test rate

### Process Metrics  
- **TDD Adoption**: >90% of commits include tests first
- **Review Coverage**: 100% of code changes reviewed
- **Documentation**: All public APIs documented
- **Accessibility**: WCAG AA compliance achieved

## 🤝 Contributing & Evolution

These rules are designed to evolve with your team and the broader software development community:

1. **Regular Reviews**: Update rules quarterly based on experience
2. **Team Feedback**: Incorporate lessons learned from real projects  
3. **Industry Updates**: Stay current with testing and accessibility best practices
4. **Tool Evolution**: Adapt configurations as tools and frameworks evolve
5. **Cross-Pollination**: Share learnings across different technology stacks

---

**Remember**: The goal isn't perfection, but **continuous improvement** toward building **high-quality, accessible, maintainable software** that delivers real value to users across any technology platform! 🚀