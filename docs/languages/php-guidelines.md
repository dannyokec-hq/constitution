# PHP Development Standards at Dannyokec

This document outlines our custom PHP development standards, focusing on object-oriented programming, secure practices, and consistent code style.

## Table of Contents
1. [Project Structure](#project-structure)
2. [Code Documentation](#code-documentation)
3. [Code Style and Formatting](#code-style-and-formatting)
4. [Security Guidelines](#security-guidelines)
5. [Component Integration](#component-integration)
6. [Database Standards](#database-standards)
7. [API Response Standards](#api-response-standards)
8. [Authentication Guidelines](#authentication-guidelines)
9. [Best Practices](#best-practices)
10. [Code Examples](#code-examples)

## Project Structure

### Web Application Structure
Two Separate Repositories:
- Client repo (public_html/*):
  - Contains frontend components, assets, and includes
  - Frontend components included via PHP require/include statements
  - Has its own readme.md documentation

- Server repo (api/):
  - Contains all API endpoints and core functionality 
  - Uses Composer (vendor folder) for dependencies
  - Core API functionality inherited through coreAPI.php
  - Database connection centralized in database-connection.php
  - API endpoints documented in endpoint-cat.md
  - API usage detailed in server's readme.md
```
public_html/
├── * (client repository)
    | readme.md
│   ├── component/           # Reusable components
│   │   ├── header.php
│   │   ├── footer.php
│   │   └── session_engage.php
│   ├── css/                # Stylesheets
│   ├── js/                 # JavaScript files
│   ├── images/             # Local images only
│   └── index.php           # Main entry point

└── api/                    <== # Server repository (separate git)
    | index.php             # An empty file or php documentation showing the endpoint cat resources and how to reach them
    | .gitignore            # Files to ignore example vendor folder
    | coreAPI.php           # A php class that other directory/endpoints category inherits from. this coreAPI has things like the connection to the database and othere endpoint commonly use methods other endpoints
    | database-connection.php
    | readme.md             # describing the backend endpoints and the expected request, return, etc of the web server
    | endpoint-cat.md # graphic representation to show the title, descripton and enpoint nodes on the project
    ├── user_auth_api/ # example of an endpoint/resource category
    ├── user_data_api/ # example of an endpoint/resource category
    |–– vendor # PHP vendors where they are used
    etc..
```

## Code Documentation

### PHPDoc Comments
All classes, methods, and functions must include comprehensive PHPDoc comments:

```php
/**
 * Class description.
 *
 * Detailed description of the class purpose and functionality.
 *
 * @package PackageName
 * @author Dannyokec Dev Hub
 * @version 1.0.0
 * @contributor [Contributor Name] - [Date Modified]
 */
class ExampleClass {
    /**
     * Method description.
     *
     * Detailed description of what the method does.
     *
     * @param string $param1 Description of first parameter
     * @param int $param2 Description of second parameter
     * @return array Description of return value
     * @throws Exception Description of when this exception is thrown
     */
    public function exampleMethod($param1, $param2) {
        // Method implementation
    }
}
```

### Inline Comments
- Use inline comments to explain complex logic
- Keep comments concise and meaningful
- Use `//` for single-line comments
- Use `/* */` for multi-line comments

## Code Style and Formatting

### General Formatting
- Use 4 spaces for indentation (no tabs)
- Use Prettier extension for consistent formatting
- Maximum line length: 120 characters
- Use camelCase for method names
- Use PascalCase for class names
- Use snake_case for variables and properties

### Braces and Spacing
```php
// Correct: Use proper spacing around braces and keywords
if ($condition) {
    // 2 lines space after before code
    // Code block goes here
    // Notice the space after if and before/after braces
    // 2 line space after code
} else {
    // Code block goes here
    // Notice the space before else
}

// Incorrect: Missing spaces makes code harder to read
if($condition){
    // No space after if
    // No spaces around braces
}else{
    // No space before else
    // No spaces around braces
}

// Incorrect: One-line format reduces readability
if($condition){ /* code */ }else{ /* code */ }

// Correct: Even with short code blocks, use proper formatting
if ($condition) {
    return true;
} else {
    return false;
}
```

### Naming Conventions
```php
// Class names: PascalCase
class UserAuthentication {}

// Method names: camelCase
public function getUserData() {}

// Variable names: snake_case
$user_id = 123;
$first_name = "John";

// Constants: UPPER_CASE
const MAX_LOGIN_ATTEMPTS = 3;
```

## Security Guidelines

### File Naming
- Never save files with `.php` extension in public directories
- Use `.php` extension only for files that should be executed
- Use `.inc.php` for included files
- Use `.class.php` for class files

### SQL Injection Prevention
Always use parameterized queries with PDO:

```php
// Correct
$stmt = $pdo->prepare("SELECT * FROM users WHERE id = :id");
$stmt->bindParam(':id', $user_id, PDO::PARAM_INT);
$stmt->bindParam(':username', $user_name, PDO::PARAM_STRING);
$stmt->execute();

# Fetch as associative array for JSON compatibility
$result = $stmt->fetch(PDO::FETCH_ASSOC);

// Incorrect
$query = "SELECT * FROM users WHERE id = $user_id";
$result = mysql_query($query);
```

### Authentication
- Always use session-based authentication
- Implement CSRF protection
- Use secure password hashing (bcrypt)
- Implement rate limiting for login attempts

## Component Integration

### Required Components
- Logged-in navbar must be included on authenticated pages
- Session management component for all pages requiring authentication
- Error handling component for consistent error display

### Component Inclusion
```php
// Correct
require_once 'component/session_engage.php';
require_once 'component/logged_in_navbar.php';

// Incorrect
include 'component/session_engage.php';
include 'component/logged_in_navbar.php';
```

## API Response Standards

### JSON Response Format
All API responses must follow this structure:

```php
// Success response
[
    "status" => true,
    "res" => "Operation successful message",
    "data" => [] // Optional data payload
]

// Error response
[
    "status" => false,
    "res" => "Error message",
    "error" => "Detailed error information" // Optional
]
```

### Example API Endpoint
```php
if (isset($decoded_Data['connectMachine']) && ($_SERVER['REQUEST_METHOD'] == 'POST')) {

    /**
     * Approve a machine connection to the network node.
     * Action block to handle machine connection requests.
     * Requires an active user session to process the request.
     * 
     * This block validates the session and processes machine connection status updates.
     * Updates connection status to 'connected' if validation passes.
     *
     * @session_required true
     * @action connectMachine
     * @method POST
     */
    if (!isset($_SESSION['name'])) {


        print(json_encode([
            "status" => false, 
            "error" => "Access denied, this request is not from a logged in user"
        ]));


    } else {


        // Extract the users current API key from their session
        $email = $_SESSION['name'];
        $api_key = API::userProfile($email);
        $api_object = new API($api_key);

        // always return JSON except the repo documentation states otherwise
        print json_encode($api_object->connectMachine( $_GET["connectMachine"] ) );


    }
}
```

## Code Examples

### Good Code Example
```php
/**
 * Connect a machine to the network node.
 *
 * This method updates the machine's connection status in the database to 'connected'.
 * It ensures that the machine exists in the node list before updating.
 *
 * @param string $machine_name The name of the machine to connect.
 * @return array JSON response indicating success or failure.
 */
public function connectMachine($machine_name) {
    // Check the current connection status of the machine
    $connectionStatus = $this->checkMachine($machine_name);

    // If the machine is not found in the node list, return an error
    if ($connectionStatus["res"] == "no_connection") {
        return [
            "status" => false,
            "res" => "Machine:: $machine_name is not on node list"
        ];
    }

    // If the machine is already connected, return an error
    if ($connectionStatus["body"] == "connected") {
        return [
            "status" => false,
            "res" => "Machine:: $machine_name is already connected"
        ];
    }

    // Establish a database connection
    $db = $this->connect();

    // Prepare an SQL statement to update the machine's connection status
    $sql = "
        UPDATE okecbot_engines 
        SET connection_status = 'connected', 
            updated_timeStamp = current_timestamp() 
        WHERE machine_name = :machine_name 
        AND owner = :apikey;
    ";

    $query = $db->prepare($sql);
    $query->bindParam(':apikey', $this->key, PDO::PARAM_STRING);
    $query->bindParam(':machine_name', $machine_name, PDO::PARAM_STRING);

    // Execute the query
    $query->execute();

    // Check if the update was successful
    if ($query->rowCount() > 0) {
        return [
            "status" => true,
            "res" => "Machine:: $machine_name Connected to network node"
        ];
    } else {
        return [
            "status" => false,
            "res" => "Machine:: $machine_name Could not be connected to node"
        ];
    }
}
```

### Bad Code Example
```php
// Bad: No documentation, inconsistent formatting, insecure
function connectMachine($machine_name) {
    $status = checkMachine($machine_name);
    if($status["res"]=="no_connection")return["status"=>false,"res"=>"Machine:: $machine_name is not on node list"];
    if($status["body"]=="connected")return["status"=>false,"res"=>"Machine:: $machine_name is already connected"];
    $db=mysql_connect("localhost","user","pass");
    $query="UPDATE okecbot_engines SET connection_status='connected' WHERE machine_name='$machine_name'";
    mysql_query($query);
    return["status"=>true,"res"=>"Connected"];
}
```

## Database Standards

### 1. Database Overview
```markdown
# Database: project_name_db

## Description
Brief description of the database purpose and main entities.

## Version
Current version: 1.0.0
Last updated: YYYY-MM-DD

## Tables
- users
- transactions
- customer_engagement
etc...
```

### 2. Table Documentation Template
```markdown
## Table: table_name

### Purpose
Description of the table's purpose and usage.

### Columns
| Column Name | Type | Nullable | Default | Description |
|------------|------|----------|---------|-------------|
| id | BIGINT UNSIGNED | NO | AUTO_INCREMENT | Primary key |
| created_at | DATETIME | NO | CURRENT_TIMESTAMP | Record creation timestamp |
| updated_at | DATETIME | NO | CURRENT_TIMESTAMP | Last update timestamp |

### Indexes
| Index Name | Type | Columns | Purpose |
|------------|------|---------|---------|
| idx_column_name | BTREE | column_name | Optimize search queries |

### Foreign Keys
| Column | References | On Delete | On Update |
|--------|------------|-----------|-----------|
| user_id | users(id) | CASCADE | CASCADE |

### Example Usage
```php
// Example query
$sql = "SELECT * FROM table_name WHERE id = :id";
```

### Migration History
- v1.0.0: Initial table creation
- v1.1.0: Added new_column


### 3. Example Table Documentation

#### Users Table
```markdown
## Table: users

### Purpose
Stores user account information and authentication details.

### Columns
| Column Name | Type | Nullable | Default | Description |
|------------|------|----------|---------|-------------|
| id | BIGINT UNSIGNED | NO | AUTO_INCREMENT | Primary key |
| username | VARCHAR(50) | NO | NULL | Unique username |
| email | VARCHAR(255) | NO | NULL | User's email address |
| password_hash | VARCHAR(255) | NO | NULL | Bcrypt hashed password |
| status | ENUM('active','inactive','suspended') | NO | 'active' | User account status |
| created_at | DATETIME | NO | CURRENT_TIMESTAMP | Account creation time |
| updated_at | DATETIME | NO | CURRENT_TIMESTAMP | Last update time |

### Indexes
| Index Name | Type | Columns | Purpose |
|------------|------|---------|---------|
| idx_username | BTREE | username | Optimize username lookups |
| idx_email | BTREE | email | Optimize email lookups |

### Example Usage
```php
// Create new user
$sql = "INSERT INTO users (username, email, password_hash) 
        VALUES (:username, :email, :password_hash)";

// Get user by email
$sql = "SELECT * FROM users WHERE email = :email";
```

### Migration History
- v1.0.0: Initial table creation
- v1.1.0: Added status column
- v1.2.0: Added updated_at column
```

#### Machine Nodes Table
```markdown
## Table: machine_nodes

### Purpose
Stores information about connected machines and their status.

### Columns
| Column Name | Type | Nullable | Default | Description |
|------------|------|----------|---------|-------------|
| id | BIGINT UNSIGNED | NO | AUTO_INCREMENT | Primary key |
| owner | VARCHAR(255) | NO | NULL | API key of the owner |
| machine_name | VARCHAR(100) | NO | NULL | Name of the machine |
| originating_ip | VARCHAR(45) | NO | NULL | IP address of the machine |
| connection_status | ENUM('connected','requesting','disconnected') | NO | 'requesting' | Current connection status |
| initiated_timeStamp | DATETIME | NO | CURRENT_TIMESTAMP | Initial connection time |
| updated_timeStamp | DATETIME | NO | CURRENT_TIMESTAMP | Last status update |

### Indexes
| Index Name | Type | Columns | Purpose |
|------------|------|---------|---------|
| idx_owner | BTREE | owner | Optimize owner lookups |
| idx_machine_name | BTREE | machine_name | Optimize machine name lookups |

### Example Usage
```php
// Check machine status
$sql = "SELECT connection_status FROM machine_nodes 
        WHERE owner = :owner AND machine_name = :machine_name";

// Update connection status
$sql = "UPDATE machine_nodes 
        SET connection_status = :status, updated_timeStamp = CURRENT_TIMESTAMP 
        WHERE id = :id";
```

### Migration History
- v1.0.0: Initial table creation
- v1.1.0: Added originating_ip column
- v1.2.0: Added updated_timeStamp column


## Database Version Control

1. **Migration Files**
   - Use timestamp-based naming: `YYYYMMDD_HHMMSS_description.sql`
   - Include both up and down migrations
   - Document changes in migration file header

2. **Version Tracking**
   - Maintain a `database_version` table
   - Track all schema changes
   - Include rollback procedures

## Best Practices

1. **Code Organization**
   - One class per file
   - Logical file naming
   - Proper directory structure

2. **Error Handling**
   - Use try-catch blocks
   - Log errors appropriately
   - Return meaningful error messages

3. **Security**
   - Validate all input
   - Sanitize output
   - Use prepared statements
   - Implement proper access controls

4. **Performance**
   - Optimize database queries
   - Use appropriate caching
   - Minimize database connections

5. **Maintainability**
   - Write self-documenting code
   - Follow DRY principle
   - Use meaningful variable names
   - Keep methods focused and small

[Rest of the existing content continues...] 