# terraform code styling best practices

While Terraform itself (HCL - HashiCorp Configuration Language) doesn’t enforce strict whitespace rules, consistent and readable whitespace usage is a best practice for maintainability, collaboration, and clarity. Since your previous question referenced YAML in a Terraform context, I’ll cover whitespace best practices for both **HCL** (Terraform’s primary language) and **YAML** (when used with Terraform, e.g., via providers or modules). Below are the guidelines:

---

### Terraform HCL Whitespace Best Practices

#### 1. Indentation
- **Use 2 spaces** for indentation (not tabs). This is the de facto standard in the Terraform community, aligning with tools like `terraform fmt`.
- Example:
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-12345678"
    instance_type = "t2.micro"
  }
  ```

#### 2. Block Alignment
- Align attributes within a block for readability when there are multiple related settings.
- Example:
  ```hcl
  resource "aws_vpc" "main" {
    cidr_block           = "10.0.0.0/16"
    enable_dns_support   = true
    enable_dns_hostnames = true
  }
  ```

#### 3. Spacing Around Operators
- Use a single space around operators like `=` for consistency.
- Example:
  ```hcl
  variable "region" {
    default = "us-west-2"
  }
  ```

#### 4. Blank Lines
- Add a blank line between logical sections (e.g., between resources, variables, or outputs) to improve readability.
- Example:
  ```hcl
  resource "aws_vpc" "main" {
    cidr_block = "10.0.0.0/16"
  }

  resource "aws_subnet" "subnet" {
    vpc_id     = aws_vpc.main.id
    cidr_block = "10.0.1.0/24"
  }
  ```

#### 5. Lists and Maps
- For multi-line lists or maps, indent items and align them under the key.
- Example:
  ```hcl
  tags = {
    Name        = "example"
    Environment = "prod"
  }

  security_groups = [
    "sg-12345678",
    "sg-87654321",
  ]
  ```

#### 6. Use `terraform fmt`
- Run `terraform fmt` to automatically standardize whitespace and formatting across your `.tf` files. It enforces many of these conventions (e.g., 2-space indents, alignment).

---

### Terraform YAML Whitespace Best Practices

When using YAML with Terraform (e.g., for configuration files consumed by a provider or module), YAML’s whitespace rules apply, as it’s indentation-sensitive.

#### 1. Indentation
- **Use 2 spaces** for indentation (YAML standard). Avoid tabs, as YAML parsers reject them.
- Example:
  ```yaml
  resource:
    aws_instance:
      example:
        ami: ami-12345678
        instance_type: t2.micro
  ```

#### 2. Avoid Over-Indentation
- Don’t indent more than necessary. Each level should be exactly 2 spaces deeper than the parent.
- Bad:
  ```yaml
  resource:
      aws_instance:  # Over-indented
        example:
          ami: ami-12345678
  ```
- Good:
  ```yaml
  resource:
    aws_instance:
      example:
        ami: ami-12345678
  ```

#### 3. Key-Value Spacing
- Use a single space after the colon (`: `) separating keys and values for readability.
- Example:
  ```yaml
  enabled: true
  name: example
  ```

#### 4. Lists
- For sequences (lists), use a hyphen (`-`) followed by a space, and align nested items.
- Example:
  ```yaml
  security_groups:
    - sg-12345678
    - sg-87654321
  ```

#### 5. Blank Lines
- Use blank lines to separate logical sections, similar to HCL.
- Example:
  ```yaml
  resource:
    aws_vpc:
      main:
        cidr_block: 10.0.0.0/16

  resource:
    aws_subnet:
      subnet:
        cidr_block: 10.0.1.0/24
  ```

#### 6. Consistency with HCL
- If your YAML is part of a broader Terraform project, match the HCL 2-space convention for consistency across files.

---

### General Best Practices Across Both Formats

1. **Consistency**:
   - Choose a style (e.g., 2-space indentation) and stick to it across all files in your project.

2. **Readability Over Density**:
   - Prioritize clarity over compactness. Extra spaces or blank lines can make complex configs easier to scan.

3. **Team Standards**:
   - If working in a team, agree on a style guide (or adopt an existing one, like HashiCorp’s recommendations) and document it (e.g., in a `.editorconfig` file).

4. **Tooling**:
   - Use `terraform fmt` for HCL files.
   - For YAML, use a linter like `yamllint` to enforce consistent spacing and catch errors.

5. **Comments**:
   - In HCL, use `#` or `//` for comments with a space after.
     ```hcl
     # This is a VPC
     resource "aws_vpc" "main" {
       cidr_block = "10.0.0.0/16"
     }
     ```
   - In YAML, use `#` with a space after.
     ```yaml
     # VPC configuration
     resource:
       aws_vpc:
         cidr_block: 10.0.0.0/16
     ```

---

### Example: HCL vs YAML Side-by-Side

#### HCL
```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  tags = {
    Name = "example-instance"
  }
}
```

#### YAML (Equivalent)
```yaml
resource:
  aws_instance:
    example:
      ami: ami-12345678
      instance_type: t2.micro
      tags:
        Name: example-instance
```

Both examples use 2-space indentation and consistent spacing for readability.

---

### Final Recommendation
- For **HCL**: Stick to 2-space indentation, align attributes, and use `terraform fmt` to enforce consistency.
- For **YAML**: Use 2-space indentation, respect YAML’s syntax rules, and keep spacing minimal but readable.
- Always test your configs (`terraform validate`) to ensure formatting doesn’t introduce errors.

Let me know if you’d like help applying this to a specific Terraform config!
