# Mastering Defensive Coding in C#: Protecting Your Software from the Unexpected

In the world of software development, writing code that works is only half the battle. The other half is ensuring that your code can handle unexpected situations, invalid inputs, and potential errors gracefully. This is where defensive coding comes into play. Defensive coding is a practice that aims to improve software and source code, in terms of quality and comprehension, reducing the number of software bugs and issues.

In this post, we'll explore the concept of defensive coding and how to implement it effectively in C#. We'll cover various techniques and best practices, complete with code examples to illustrate each point.

## 1. Input Validation
One of the fundamental aspects of defensive coding is input validation. Never trust user input or data from external sources. Always validate and sanitize inputs before processing them.

```csharp
public class UserRegistration
{
    public bool RegisterUser(string username, string email, int age)
    {
        if (string.IsNullOrWhiteSpace(username))
        {
            throw new ArgumentException("Username cannot be empty or whitespace.", nameof(username));
        }

        if (!IsValidEmail(email))
        {
            throw new ArgumentException("Invalid email format.", nameof(email));
        }

        if (age < 18 || age > 120)
        {
            throw new ArgumentOutOfRangeException(nameof(age), "Age must be between 18 and 120.");
        }

        // Proceed with user registration
        return true;
    }

    private bool IsValidEmail(string email)
    {
        // Implement email validation logic
        return System.Text.RegularExpressions.Regex.IsMatch(email, @"^[^@\s]+@[^@\s]+\.[^@\s]+$");
    }
}

```
