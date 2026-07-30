# 🧰 Terraform Functions

## 📖 Introduction

Terraform provides **built-in functions** that help you manipulate data, transform values, perform calculations, work with files, handle dates, and simplify Infrastructure as Code (IaC).

Functions make Terraform configurations more **dynamic, reusable, and easier to maintain**.

Instead of hardcoding values, you can use functions to calculate or transform them automatically.

---

# What is a Function?

A function takes one or more input values (arguments), performs an operation, and returns a result.

General syntax:

```hcl
function_name(argument1, argument2, ...)
```

Example:

```hcl
upper("terraform")
```

Result:

```
TERRAFORM
```

---

# Why Use Functions?

Without functions:

```hcl
tags = {
  Name = "WEB-SERVER"
}
```

With functions:

```hcl
tags = {
  Name = upper(var.server_name)
}
```

Now the value changes automatically based on the variable.

---

# Categories of Terraform Functions

Terraform includes many built-in functions, commonly grouped into:

- String Functions
- Numeric Functions
- Collection Functions
- Type Conversion Functions
- Encoding Functions
- File Functions
- Date & Time Functions
- IP Network Functions

---

# String Functions

## upper()

Converts text to uppercase.

```hcl
upper("terraform")
```

Result:

```
TERRAFORM
```

---

## lower()

Converts text to lowercase.

```hcl
lower("DEVOPS")
```

Result:

```
devops
```

---

## title()

Capitalizes the first letter of each word.

```hcl
title("terraform for devops")
```

Result:

```
Terraform For Devops
```

---

## length()

Returns the number of characters in a string.

```hcl
length("Terraform")
```

Result:

```
9
```

---

## replace()

Replaces text within a string.

```hcl
replace("terraform", "terra", "cloud")
```

Result:

```
cloudform
```

---

## substr()

Returns part of a string.

```hcl
substr("Terraform", 0, 4)
```

Result:

```
Terr
```

---

## trim()

Removes leading and trailing spaces.

```hcl
trim("  Terraform  ")
```

Result:

```
Terraform
```

---

# Numeric Functions

## max()

Returns the largest value.

```hcl
max(10, 20, 30)
```

Result:

```
30
```

---

## min()

Returns the smallest value.

```hcl
min(10, 20, 30)
```

Result:

```
10
```

---

## abs()

Returns the absolute value.

```hcl
abs(-15)
```

Result:

```
15
```

---

## ceil()

Rounds up.

```hcl
ceil(3.2)
```

Result:

```
4
```

---

## floor()

Rounds down.

```hcl
floor(3.9)
```

Result:

```
3
```

---

# Collection Functions

## length()

Returns the number of elements.

```hcl
length(["dev", "test", "prod"])
```

Result:

```
3
```

---

## element()

Returns an item by index.

```hcl
element(["dev", "test", "prod"], 1)
```

Result:

```
test
```

---

## contains()

Checks if a collection contains a value.

```hcl
contains(["aws", "azure"], "aws")
```

Result:

```
true
```

---

## concat()

Combines multiple lists.

```hcl
concat(["dev"], ["test"], ["prod"])
```

Result:

```
["dev","test","prod"]
```

---

## distinct()

Removes duplicate values.

```hcl
distinct(["dev","dev","prod"])
```

Result:

```
["dev","prod"]
```

---

## sort()

Sorts values alphabetically.

```hcl
sort(["c","a","b"])
```

Result:

```
["a","b","c"]
```

---

# Type Conversion Functions

## tostring()

```hcl
tostring(100)
```

Result:

```
"100"
```

---

## tonumber()

```hcl
tonumber("50")
```

Result:

```
50
```

---

## tolist()

Converts a collection to a list.

```hcl
tolist(var.items)
```

---

## tomap()

Converts a value into a map.

```hcl
tomap({
  Name = "Web"
})
```

---

## toset()

Removes duplicate values.

```hcl
toset(["a","a","b"])
```

Result:

```
["a","b"]
```

---

# Encoding Functions

## jsonencode()

Converts Terraform values into JSON.

```hcl
jsonencode({
  Name = "Web"
})
```

Result:

```json
{"Name":"Web"}
```

---

## jsondecode()

Converts JSON into Terraform values.

```hcl
jsondecode(file("config.json"))
```

---

## base64encode()

Encodes text into Base64.

```hcl
base64encode("Terraform")
```

---

## base64decode()

Decodes Base64 text.

```hcl
base64decode("VGVycmFmb3Jt")
```

---

# File Functions

## file()

Reads the contents of a file.

```hcl
file("script.sh")
```

---

## fileexists()

Checks whether a file exists.

```hcl
fileexists("userdata.sh")
```

Result:

```
true
```

---

## templatefile()

Reads a template file and replaces variables.

```hcl
templatefile("userdata.tpl", {
  hostname = "web01"
})
```

---

# Date & Time Functions

## timestamp()

Returns the current timestamp.

```hcl
timestamp()
```

Example:

```
2026-07-30T15:20:45Z
```

---

## formatdate()

Formats dates.

```hcl
formatdate("YYYY-MM-DD", timestamp())
```

Result:

```
2026-07-30
```

---

# IP Network Functions

## cidrsubnet()

Creates a subnet from a CIDR block.

```hcl
cidrsubnet("10.0.0.0/16", 8, 1)
```

Result:

```
10.0.1.0/24
```

---

## cidrhost()

Returns a host IP from a subnet.

```hcl
cidrhost("10.0.1.0/24", 10)
```

Result:

```
10.0.1.10
```

---

# Real-World Example

```hcl
locals {

  environment = upper(var.environment)

  server_name = "${var.project}-${lower(var.environment)}"

  subnet = cidrsubnet("10.0.0.0/16", 8, 1)

}
```

Terraform automatically:

- Converts text
- Creates subnet addresses
- Generates naming conventions

---

# Easy Way to Remember

Think of Terraform functions like **Excel formulas**.

```
Excel

SUM()

MAX()

MIN()

UPPER()

↓

Returns a Value
```

Terraform works the same way.

```
upper()

length()

max()

concat()

jsonencode()

↓

Returns a Value
```

Functions simply take input, perform an operation, and return a result.

---

# Best Practices

- Use functions instead of hardcoding values.
- Keep expressions readable.
- Store complex calculations in `locals`.
- Prefer built-in functions over custom workarounds.
- Use descriptive variable names.

---

# Common Mistakes

❌ Nesting too many functions in one expression.

❌ Using the wrong data type as a function argument.

❌ Forgetting that some functions expect lists, maps, or strings.

❌ Ignoring readability when combining multiple functions.

---

# Interview Questions

### What are Terraform functions?

Terraform functions are built-in operations that take one or more arguments and return a computed value.

---

### Name some commonly used Terraform functions.

- `upper()`
- `lower()`
- `length()`
- `contains()`
- `concat()`
- `jsonencode()`
- `file()`
- `timestamp()`
- `cidrsubnet()`

---

### What is the purpose of `jsonencode()`?

It converts Terraform values into JSON format.

---

### Which function reads the contents of a file?

```hcl
file()
```

---

### Which function is commonly used for subnet calculations?

```hcl
cidrsubnet()
```

---

### Why are functions important in Terraform?

Functions make Terraform configurations dynamic, reusable, easier to maintain, and reduce hardcoded values.

---

# Summary

Terraform functions help transform and manipulate data without writing custom code.

Some of the most commonly used categories are:

- String Functions
- Numeric Functions
- Collection Functions
- Type Conversion Functions
- Encoding Functions
- File Functions
- Date & Time Functions
- IP Network Functions

Understanding these built-in functions enables you to write cleaner, more flexible, and production-ready Terraform configurations.