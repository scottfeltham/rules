# Universal Coding Standards

Technology-agnostic implementation patterns and conventions for consistent, maintainable, and high-quality code across any programming language or framework.

## 🏗️ Architecture Patterns

### Clean Architecture Principles
```
┌─────────────────────────────────────┐
│         Presentation Layer          │  UI, Controllers, Views
├─────────────────────────────────────┤
│         Application Layer           │  Use Cases, Services, Orchestration
├─────────────────────────────────────┤
│          Domain Layer               │  Business Logic, Entities, Rules
├─────────────────────────────────────┤
│        Infrastructure Layer         │  Database, External APIs, I/O
└─────────────────────────────────────┘
```

### Dependency Rules
- **Inward Dependencies**: Outer layers depend on inner layers, never the reverse
- **Dependency Injection**: Use dependency injection for loose coupling
- **Interface Abstraction**: Depend on interfaces/abstractions, not implementations
- **Single Direction**: Dependencies flow in one direction only

### SOLID Principles
- **S** - Single Responsibility: Each class/module has one reason to change
- **O** - Open/Closed: Open for extension, closed for modification
- **L** - Liskov Substitution: Subtypes must be substitutable for their base types
- **I** - Interface Segregation: Many client-specific interfaces are better than one general-purpose interface
- **D** - Dependency Inversion: Depend upon abstractions, not concretions

## 📝 Naming Conventions

### Universal Naming Principles
- **Clarity over brevity**: Names should be self-documenting
- **Consistency**: Use consistent naming patterns throughout the project
- **Context-appropriate**: Names should make sense in their domain context
- **Avoid abbreviations**: Use complete words unless abbreviations are standard

### Language-Specific Adaptations

#### Pascal Case (Classes, Types, Interfaces)
```
// Java, C#, Swift, Kotlin
class UserService
interface PaymentProcessor
enum OrderStatus

// Python (for classes)
class UserService
class PaymentProcessor

// Go (exported identifiers)
type UserService struct
type PaymentProcessor interface
```

#### Camel Case (Methods, Variables, Functions)
```
// Java, JavaScript, C#, Swift, Kotlin
function getUserById()
let userName = ""
method processPayment()

// Python (functions and variables use snake_case)
def get_user_by_id():
user_name = ""
```

#### Snake Case (Variables, Functions)
```
// Python, Ruby, Rust, C
def process_payment():
user_name = ""
const MAX_RETRY_COUNT = 3

// SQL, environment variables
SELECT user_name FROM users;
DATABASE_URL=...
```

#### Kebab Case (File names, URLs, Configuration)
```
// File names
user-service.py
payment-processor.go
order-status.rb

// URLs and routes
/api/user-profile
/admin/order-management

// Configuration keys
max-retry-count
database-connection-string
```

### Constants and Configuration
```
// All caps with underscores (most languages)
MAX_RETRY_ATTEMPTS = 3
DATABASE_CONNECTION_TIMEOUT = 30
API_BASE_URL = "https://api.example.com"

// Or language-specific conventions
const maxRetryAttempts = 3  // JavaScript
final int MAX_RETRY_ATTEMPTS = 3;  // Java
MAX_RETRY_ATTEMPTS: int = 3  # Python with type hints
```

## 🔧 Function and Method Standards

### Single Responsibility Functions
```
// ✅ GOOD: Each function does one thing
function validateEmail(email):
    pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    return pattern.matches(email)

function sendWelcomeEmail(userEmail):
    template = loadTemplate("welcome")
    return emailService.send(userEmail, template)

function createUser(userData):
    if not validateEmail(userData.email):
        throw ValidationError("Invalid email")
    
    user = User.create(userData)
    sendWelcomeEmail(user.email)
    return user

// ❌ BAD: Multiple responsibilities
function processUser(email, userData):
    // Validation AND creation AND email sending in one function
    if not email.contains("@"):
        throw Error("Invalid")
    user = saveToDatabase(userData)
    sendEmail(email, "Welcome!")
    return user
```

### Pure Functions (When Possible)
```
// ✅ GOOD: Pure function - same input, same output, no side effects
function calculateTax(price, taxRate):
    return price * taxRate

function formatCurrency(amount, currency = "USD"):
    return currencyFormatter.format(amount, currency)

// ❌ BAD: Side effects make function unpredictable
let totalProcessed = 0
function calculateTaxBad(price, taxRate):
    totalProcessed += price  // Side effect
    return price * taxRate
```

### Parameter Management
```
// ✅ GOOD: Use objects/structs for multiple parameters
struct CreateUserRequest:
    email: String
    name: String
    role: UserRole = USER
    sendWelcomeEmail: Bool = true

function createUser(request: CreateUserRequest):
    // Implementation using request.email, request.name, etc.

// ✅ GOOD: Builder pattern for complex construction
UserBuilder.new()
    .withEmail("user@example.com")
    .withName("John Doe")
    .withRole(ADMIN)
    .build()

// ❌ BAD: Too many parameters
function createUser(email, name, role, sendEmail, isActive, department, manager):
    // Hard to use and maintain
```

### Error Handling Patterns

#### Result/Option Types (Functional Languages)
```
// Rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err("Division by zero".to_string())
    } else {
        Ok(a / b)
    }
}

// Swift
func divide(_ a: Double, _ b: Double) -> Result<Double, ArithmeticError> {
    guard b != 0 else {
        return .failure(.divisionByZero)
    }
    return .success(a / b)
}

// F#
let divide a b =
    if b = 0.0 then
        Error "Division by zero"
    else
        Ok (a / b)
```

#### Exception-Based (OOP Languages)
```
// Java
public class ValidationException extends Exception {
    private final String field;
    
    public ValidationException(String field, String message) {
        super(message);
        this.field = field;
    }
}

public User createUser(UserData userData) throws ValidationException {
    if (!isValidEmail(userData.getEmail())) {
        throw new ValidationException("email", "Invalid email format");
    }
    return userRepository.save(new User(userData));
}

// Python
class ValidationError(Exception):
    def __init__(self, field: str, message: str):
        self.field = field
        super().__init__(message)

def create_user(user_data: dict) -> User:
    if not is_valid_email(user_data['email']):
        raise ValidationError('email', 'Invalid email format')
    return user_repository.save(User(user_data))

// C#
public class ValidationException : Exception
{
    public string Field { get; }
    
    public ValidationException(string field, string message) : base(message)
    {
        Field = field;
    }
}
```

#### Error Codes (C-style, Go)
```
// Go
func createUser(userData UserData) (*User, error) {
    if !isValidEmail(userData.Email) {
        return nil, fmt.Errorf("invalid email: %s", userData.Email)
    }
    
    user, err := userRepository.Save(userData)
    if err != nil {
        return nil, fmt.Errorf("failed to save user: %w", err)
    }
    
    return user, nil
}

// C
enum ErrorCode {
    SUCCESS = 0,
    INVALID_EMAIL = 1,
    DATABASE_ERROR = 2
};

enum ErrorCode create_user(const UserData* user_data, User** result) {
    if (!is_valid_email(user_data->email)) {
        return INVALID_EMAIL;
    }
    
    *result = malloc(sizeof(User));
    if (!*result) {
        return DATABASE_ERROR;
    }
    
    // Initialize user...
    return SUCCESS;
}
```

## 🗂️ Code Organization

### File and Module Structure

#### Object-Oriented Languages
```
// Java package structure
com.company.project.domain.entities/
    User.java
    Order.java
com.company.project.domain.services/
    UserService.java
    OrderService.java
com.company.project.infrastructure.repositories/
    UserRepository.java
    OrderRepository.java

// C# namespace structure
Company.Project.Domain.Entities/
    User.cs
    Order.cs
Company.Project.Domain.Services/
    IUserService.cs
    UserService.cs
Company.Project.Infrastructure.Repositories/
    IUserRepository.cs
    UserRepository.cs
```

#### Functional Languages
```
// F# module structure
module Company.Project.Domain.Users
    type User = { Id: int; Name: string; Email: string }
    let validateEmail email = ...
    let createUser userData = ...

module Company.Project.Domain.Orders
    type Order = { Id: int; UserId: int; Total: decimal }
    let calculateTotal items = ...
    let createOrder orderData = ...

// Haskell module structure
module Company.Project.Domain.Users
  ( User(..)
  , validateEmail
  , createUser
  ) where

data User = User
  { userId :: Int
  , userName :: String
  , userEmail :: String
  }
```

#### Procedural/Scripting Languages
```
# Python package structure
company_project/
    domain/
        entities/
            user.py
            order.py
        services/
            user_service.py
            order_service.py
    infrastructure/
        repositories/
            user_repository.py
            order_repository.py

# Ruby module structure
module Company
  module Project
    module Domain
      module Users
        class User
          attr_reader :id, :name, :email
        end
        
        class UserService
          def create_user(user_data)
          end
        end
      end
    end
  end
end
```

### Interface Definitions

#### Explicit Interfaces (Java, C#, TypeScript)
```
// Java
public interface UserRepository {
    User findById(Long id);
    User save(User user);
    void delete(Long id);
}

// C#
public interface IUserRepository
{
    Task<User> FindByIdAsync(long id);
    Task<User> SaveAsync(User user);
    Task DeleteAsync(long id);
}

// TypeScript
interface UserRepository {
    findById(id: string): Promise<User | null>;
    save(user: User): Promise<User>;
    delete(id: string): Promise<void>;
}
```

#### Duck Typing (Python, Ruby)
```
# Python - Protocol for typing
from typing import Protocol

class UserRepository(Protocol):
    def find_by_id(self, user_id: int) -> User | None: ...
    def save(self, user: User) -> User: ...
    def delete(self, user_id: int) -> None: ...

# Ruby - Module for shared behavior
module UserRepository
  def find_by_id(id)
    raise NotImplementedError
  end
  
  def save(user)
    raise NotImplementedError
  end
  
  def delete(id)
    raise NotImplementedError
  end
end
```

#### Traits (Rust, Scala)
```
// Rust
pub trait UserRepository {
    fn find_by_id(&self, id: u64) -> Result<Option<User>, RepositoryError>;
    fn save(&self, user: &User) -> Result<User, RepositoryError>;
    fn delete(&self, id: u64) -> Result<(), RepositoryError>;
}

// Scala
trait UserRepository {
  def findById(id: Long): Future[Option[User]]
  def save(user: User): Future[User]
  def delete(id: Long): Future[Unit]
}
```

## 📊 Data Management

### Value Objects and Entities
```
// Domain-Driven Design principles apply across languages

// Java - Value Object
public class Email {
    private final String value;
    
    public Email(String email) {
        if (!isValid(email)) {
            throw new IllegalArgumentException("Invalid email");
        }
        this.value = email;
    }
    
    // Immutable, equals/hashCode based on value
}

// Python - Value Object using dataclass
@dataclass(frozen=True)
class Email:
    value: str
    
    def __post_init__(self):
        if not self._is_valid(self.value):
            raise ValueError("Invalid email")
    
    def _is_valid(self, email: str) -> bool:
        return "@" in email  # Simplified

// Go - Value Object
type Email struct {
    value string
}

func NewEmail(email string) (*Email, error) {
    if !isValidEmail(email) {
        return nil, errors.New("invalid email")
    }
    return &Email{value: email}, nil
}

func (e Email) String() string {
    return e.value
}
```

### Data Transfer Objects
```
// Language-specific serialization patterns

// Java - DTO with validation
public class CreateUserRequest {
    @NotNull
    @Email
    private String email;
    
    @NotBlank
    @Size(min = 2, max = 100)
    private String name;
    
    // Getters, setters, validation annotations
}

// Python - Pydantic model
from pydantic import BaseModel, EmailStr, validator

class CreateUserRequest(BaseModel):
    email: EmailStr
    name: str
    
    @validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v

// Go - Struct with validation tags
type CreateUserRequest struct {
    Email string `json:"email" validate:"required,email"`
    Name  string `json:"name" validate:"required,min=2,max=100"`
}

// Rust - Serde-compatible struct
#[derive(Serialize, Deserialize, Validate)]
pub struct CreateUserRequest {
    #[validate(email)]
    pub email: String,
    
    #[validate(length(min = 2, max = 100))]
    pub name: String,
}
```

## 🔄 Asynchronous Operations

### Language-Specific Async Patterns

#### Promise-Based (JavaScript, Dart)
```
// JavaScript
async function fetchUser(id) {
    try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
            throw new Error('Failed to fetch user');
        }
        return await response.json();
    } catch (error) {
        console.error('Error fetching user:', error);
        throw error;
    }
}

// Dart
Future<User> fetchUser(String id) async {
    try {
        final response = await http.get(Uri.parse('/api/users/$id'));
        if (response.statusCode != 200) {
            throw Exception('Failed to fetch user');
        }
        return User.fromJson(jsonDecode(response.body));
    } catch (error) {
        print('Error fetching user: $error');
        rethrow;
    }
}
```

#### Future-Based (Scala, Java)
```
// Scala
def fetchUser(id: Long): Future[User] = {
    httpClient.get(s"/api/users/$id")
        .map(response => Json.parse(response.body).as[User])
        .recover {
            case ex: Exception => 
                logger.error(s"Error fetching user $id", ex)
                throw ex
        }
}

// Java with CompletableFuture
public CompletableFuture<User> fetchUser(Long id) {
    return httpClient.sendAsync(
            HttpRequest.newBuilder()
                .uri(URI.create("/api/users/" + id))
                .build(),
            HttpResponse.BodyHandlers.ofString())
        .thenApply(response -> {
            if (response.statusCode() != 200) {
                throw new RuntimeException("Failed to fetch user");
            }
            return objectMapper.readValue(response.body(), User.class);
        })
        .exceptionally(ex -> {
            logger.error("Error fetching user " + id, ex);
            throw new RuntimeException(ex);
        });
}
```

#### Async/Await (C#, Python)
```
// C#
public async Task<User> FetchUserAsync(long id)
{
    try
    {
        using var response = await httpClient.GetAsync($"/api/users/{id}");
        response.EnsureSuccessStatusCode();
        
        var content = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<User>(content);
    }
    catch (Exception ex)
    {
        logger.LogError(ex, "Error fetching user {UserId}", id);
        throw;
    }
}

// Python
async def fetch_user(user_id: int) -> User:
    try:
        async with httpx.AsyncClient() as client:
            response = await client.get(f"/api/users/{user_id}")
            response.raise_for_status()
            return User(**response.json())
    except httpx.HTTPError as ex:
        logger.error(f"Error fetching user {user_id}: {ex}")
        raise
```

## 🧹 Code Quality Principles

### DRY - Don't Repeat Yourself
```
// ✅ GOOD: Extract common functionality
// Java
public class StringUtils {
    public static boolean isEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }
}

// Usage
if (StringUtils.isEmpty(user.getName())) {
    throw new ValidationException("Name cannot be empty");
}
if (StringUtils.isEmpty(user.getEmail())) {
    throw new ValidationException("Email cannot be empty");
}

// Python
def is_empty(value: str | None) -> bool:
    return not value or not value.strip()

# Usage
if is_empty(user.name):
    raise ValidationError("Name cannot be empty")
if is_empty(user.email):
    raise ValidationError("Email cannot be empty")
```

### KISS - Keep It Simple, Stupid
```
// ✅ GOOD: Simple and clear
function isAdult(age) {
    return age >= 18;
}

// ❌ BAD: Overcomplicated
function isAdultBad(age) {
    if (age >= 18) {
        return true;
    } else if (age < 18) {
        return false;
    } else {
        return false;
    }
}
```

### YAGNI - You Aren't Gonna Need It
```
// ✅ GOOD: Only what's needed now
class User {
    private String id;
    private String email;
    private String name;
    
    // Basic getters, setters, constructor
}

// ❌ BAD: Premature abstraction
abstract class BaseEntity<T> implements Serializable, Cloneable, Comparable<T> {
    private String id;
    private Date createdAt;
    private Date updatedAt;
    private String createdBy;
    private String updatedBy;
    private Integer version;
    private Map<String, Object> metadata;
    private List<String> tags;
    private Boolean deleted;
    
    // Lots of unused complexity
}
```

## 📏 Metrics and Limits

### Universal Complexity Limits
- **Function/Method Length**: Maximum 50 lines, prefer under 20
- **Class Length**: Maximum 500 lines, prefer under 300
- **Parameter Count**: Maximum 5 parameters, prefer 3 or fewer
- **Cyclomatic Complexity**: Maximum 10 per function/method
- **Nesting Depth**: Maximum 4 levels deep
- **File Length**: Maximum 1000 lines, prefer under 500

### Performance Guidelines
- **Method Response Time**: < 100ms for critical paths
- **Memory Usage**: Monitor and prevent memory leaks
- **Resource Cleanup**: Always clean up resources (files, connections, etc.)
- **Caching**: Use appropriate caching strategies for your use case

### Documentation Requirements
- **Public APIs**: Document all public interfaces
- **Complex Algorithms**: Document non-obvious logic
- **Business Rules**: Document domain-specific rules
- **Configuration**: Document configuration options
- **Deployment**: Document deployment procedures

These standards provide a foundation for high-quality code across any technology stack while remaining flexible enough to adapt to language-specific idioms and best practices.