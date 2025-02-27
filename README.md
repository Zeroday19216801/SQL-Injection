# SQL Injection

SQL Injection (SQLi) is one of the most common vulnerabilities in web applications, and it allows attackers to interfere with the queries an application makes to its database. This guide demonstrates how SQL Injection works and how to prevent it, with a focus on learning and ethical hacking.

## Prerequisites

- **Knowledge of SQL**: Understanding SQL queries and how they interact with databases.
- **A Web Application to Test**: Set up a local testing environment using tools like [XAMPP](https://www.apachefriends.org/) or [Django](https://www.djangoproject.com/) (if you're working with a web framework).
- **Legal Consent**: Always ensure you have explicit permission to test applications for vulnerabilities. Use environments specifically designed for learning, such as [DVWA (Damn Vulnerable Web Application)](https://github.com/ethicalhack3r/DVWA).

## What is SQL Injection?

SQL Injection occurs when an attacker manipulates an application's SQL query by injecting arbitrary SQL code. This can lead to unauthorized access to sensitive data, data manipulation, or even complete control over the server hosting the database.

### Types of SQL Injection

1. **Classic SQL Injection**: Manipulating the SQL query directly to access unauthorized data.
2. **Blind SQL Injection**: No error messages are returned, but the attacker can infer information based on the application's behavior.
3. **Error-Based SQL Injection**: Exploiting detailed error messages returned by the database to gain information about the database structure.
4. **Union-Based SQL Injection**: Using the `UNION` SQL operator to combine results from multiple queries into a single response.

## Example of SQL Injection 

### Scenario: User Login Form

Let's consider an example where a user submits a login form with `username` and `password` fields.

The application may execute a query like this:

```sql
SELECT * FROM users WHERE username = 'input_username' AND password = 'input_password';
```
Explanation
- Importing the requests library: This library is used to send HTTP requests in Python.
- Defining the sql_injection function: This function takes a URL and a payload as arguments.
- Constructing the full URL: The payload is appended to the URL to form the complete request URL.
- Sending a GET request: The script sends a GET request to the constructed URL.
- Checking the response: The script checks if the response contains a specific string (e.g., "error") that indicates a successful SQL injection.
## Example usage: 

The script demonstrates how to use the sql_injection function with a sample URL and payload.
### Code Breakdown
```python
import requests
```
- This library is used to send HTTP requests in Python. It allows us to interact with web servers and handle responses.
```python
def sql_injection(url, payload):
    # Construct the full URL with the payload
    full_url = f"{url}{payload}"
```
This function takes two arguments: 


url: The base URL of the vulnerable page. 


payload: The SQL injection payload to be appended to the URL.
- Constructing the full URL:
The payload is appended to the URL to form the complete request URL. This is done using an f-string, which allows for easy string interpolation.
```python
    # Send a GET request to the URL
    response = requests.get(full_url)
```
- Sending a GET request: The script sends a GET request to the constructed URL using the requests.get method. The response from the server is stored in the response variable.
    ### Check if the response contains a specific string that indicates a successful injection.
```pythn
  if "error" in response.text.lower():
        print("SQL Injection detected!")
        print("Response:", response.text)
    else:
        print("No SQL Injection detected.")
```
Checking the response: The script checks if the response text contains a specific string (e.g., "error") that indicates a successful SQL injection. The lower() method is used to make the check case-insensitive.
If the string is found, it prints a message indicating that SQL injection was detected and displays the response text.
If the string is not found, it prints a message indicating that no SQL injection was detected.
# Mitigation
To prevent SQL injection attacks, use prepared statements and parameterized queries. Here is an example of how to modify the vulnerable code to use prepared statements:
```php
<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "testdb";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

$username = $_GET['username'];
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ?");
$stmt->bind_param("s", $username);
$stmt->execute();
$result = $stmt->get_result();

if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        echo "id: " . $row["id"]. " - Name: " . $row["username"]. "<br>";
    }
} else {
    echo "0 results";
}
$stmt->close();
$conn->close();
?>
```
## Conclusion
SQL injection remains a significant security threat to web applications, including those hosted on GitHub. This vulnerability can lead to unauthorized access, data breaches, and other malicious activities. To safeguard your GitHub project, it is crucial to implement robust security measures.

### Key Takeaways:
- **Understand the Risk:** Recognize that SQL injection can exploit vulnerabilities in your application's database interactions, potentially leading to severe security breaches.

- **Use Prepared Statements:** Always use prepared statements and parameterized queries to ensure that user inputs are treated as data, not executable code.

- **Input Validation:** Validate and sanitize all user inputs to prevent malicious code from being injected into your SQL queries.

- **Regular Security Audits:** Conduct regular security audits and penetration testing to identify and fix vulnerabilities in your codebase.

- **Stay Updated:** Keep your dependencies and frameworks up-to-date with the latest security patches to protect against known vulnerabilities.

By following these best practices, you can significantly reduce the risk of SQL injection in your GitHub project, ensuring a more secure and reliable application.

