# Welcome to Learning Platform

Here is the list of topics being covered within the repository.
<details>
<summary>

## Github Documentation
</summary>
<p>

- [Github Documentation Syntax](docs/Github/Github_Documentation_Syntax.md)
</p>
</details>

<details>
<summary>

## Git Commands & Usage Documentation
</summary>
<p>

**Git Status**
Show the working tree status

```bash
git status
On branch Main
Your branch is up to date with 'origin/Main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

**Git Checkout**
Switch branches or restore working tree files

Updates files in the working tree to match the version in the index or the specified tree. If no pathspec was given, git checkout will also update HEAD to set the specified branch as the current branch.

git checkout \<branch>

```bash
git checkout main

Switched to branch 'main'
M       README.md
Your branch is up to date with 'origin/main'.
```

**Git Branch**
git-branch - List, create, or delete branches

git branch \<new branch name>
git branch --list

- Get list of branches
```bash
git branch --list
  Main---Learning---1412_01
* main
```

**Git Log**
Lists the log of the current branch

```bash
git log
git log --all --oneline
```

**Git Stash**
Stash the changes in a dirty working directory away. Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

The modifications stashed away by this command can be listed with git stash list, inspected with git stash show, and restored (potentially on top of a different commit) with git stash apply. Calling git stash without any arguments is equivalent to git stash push. A stash is by default listed as "WIP on branchname …​", but you can give a more descriptive message on the command line when you create one.

```bash
git stash list //show the stash list
```

**Git Branch**
List, create, or delete branches

If --list is given, or if there are no non-option arguments, existing branches are listed; the current branch will be highlighted in green and marked with an asterisk. Any branches checked out in linked worktrees will be highlighted in cyan and marked with a plus sign. Option -r causes the remote-tracking branches to be listed, and option -a shows both local and remote branches.

If a <pattern> is given, it is used as a shell wildcard to restrict the output to matching branches. If multiple patterns are given, a branch is shown if it matches any of the patterns.

Note that when providing a <pattern>, you must use --list; otherwise the command may be interpreted as branch creation.

```bash
git branch --list //List the branches in Git local

git branch <New Branch Name> //Creates a branch from the branch you are currently working on
```
</p>
</details>

<details>
<summary>

## C# Fundamentals
</summary>
<p>
Table of Contents:

- [C# Coding Conventions](docs/CSharpFundamentals/CSharp_Coding_Conventions.md)
- **Core C# Programming Constucts**
    - [Core C# Programming Constructs - Part 01](docs/CSharpFundamentals/CSharp_Programming_Constructs_Part01.md)
        - The anatomy of a simple C# program
        - System Data Types and corresponding C# Keywords
        - How to find default and Min/Max Values of C# Types
        - The Data Type Class Hierarchy
        - Parsing Values and Using TryParse from String Data
        - Working with String Data
        - String Interpolation and Escape Characters, Verbatim Strings
        - The Checked and Unchecked Keyword
        - C# Iteration Constructs
        -. The if/else and switch statement
    - [Core C# Programming Constructs - Part 02](docs/CSharpFundamentals/CSharp_Programming_Constructs_Part02.md)
        - Understanding C# Arrays
        - C# Array Initialization
        - Defining an Array of Objects
        - Multidimensional Arrays
        - Jagged Arrays
        - The System.Array Base Class
        - Methods
        - Expression-Bodied Members
        - Static Local Functions
        - Method Parameter Modifiers
        - Understanding the enum Type
        - Understanding the Structure (Struct) type (aka Value Type)
        - Understanding the Value Types and Reference Types
        - Understanding C# Nullable Types
        - Tuples (Not documented yet!)
- **Object Oriented Programming with C#**
    - [Object Oriented Programming with C# - Part 01](docs/CSharpFundamentals/CSharp_Object_Oriented_Programming_Part01.md)
        - Defining the Pillars of Object-Oriented Programming
        - First Pillar of OOP - Understanding Encapsulation
        - Second Pillar of OOP - Understanding Inheritance
        - Third Pillar of OOP - Understanding Polymorphism
        - Fourth Pillar of OOP - Abstraction
        - Understanding Base Class/Derived Class Casting Rules
        - The C# as Keyword
        - The C# is Keyword
        - Cast Expression
        - Pattern Matching - Reading
        - The Super Parent Class: System.Object
    - [Object Oriented Programming with C# - Part 02](docs/CSharpFundamentals/CSharp_Object_Oriented_Programming_Part02.md)
        - Exception Handling
        - Working with Interfaces
        - Interfaces vs. Abstract Base Classes
        - Understanding Object Lifetime
    - [Object Oriented Programming with C# - Part 03](docs/CSharpFundamentals/CSharp_Object_Oriented_Programming_Part03.md) 

        **Collections**
        - System.Collections Classes
        - System.Collections.Generic Classes
        - Key Interfaces supported by Classes of System.Collections.Generic
        - Working with List\<T>
        - Search and sort lists
        - Implementing a Collection of Key/Value Pairs (Dictionary)
        - Using LINQ to Access Collection
        - Working with the Stack<T> Class
        - Working with the Queue\<T> Class
    - [Object Oriented Programming with C# - Part 04](docs/CSharpFundamentals//CSharp_Object_Oriented_Programming_Part04.md)

        **Generics**
        - Creating Custom Generic Methods
        - Constraints on type Parameters
        - Enum Constraint
        - Creating Custom Generic Structures and Classes
        - Generics overview
        - Pattern Matching with Generics (7.1)
    - [Object Oriented Programming with C# - Part 5](docs/CSharpFundamentals//CSharp_Object_Oriented_Programming_Part05.md)

        **Advanced C# Languages Features**
        - Understanding Indexer Methods
        - Understanding Operator Overloading
        - Understanding Custom Type Conversions
        - Understanding Extension Methods
        - Understanding Anonymous Types
    - [Object Oriented Programming with C# - Part 6](docs/CSharpFundamentals/CSharp_Object_Oriented_Programming_Part06.md)

        **LINQ to Objects**
        - What is LINQ (Language Integrated Query)?
        - Introduction to LINQ Queries
        - Basic LINQ Query Operations
        - Standard Query Operators
            - Sorting Data
            - Set Operations
            - Filtering Data
            - Quantifier Operations
            - Projection Operations
            - Partioning Data
            - Join Operations
            - Grouping Data
            - Generation Operations
            - Element Operations
            - Converting Data Types
            - Concatenation Operations
            - Aggregation Operations

</p>
</details>

<details>
<summary>

## SOLID Principles and Design Patterns
</summary>
<p>
Table of contents:

- [SOLID Principles](docs/SolidnPatterns/SolidPrinciples.md)
    - (**S**)ingle Responsibility Principle
    - (**O**)pen-Closed Principle
    - (**L**)iskov Substitution Principle
    - (**I**)nterface Segregation Principle
    - (**D**)ependency Inversion Principle
- **Design Patterns**
  - [Builder](docs/SolidnPatterns/BuilderPattern.md)
  - [Factory](docs/SolidnPatterns/FactoryPattern.md)
  - [Singleton](docs/SolidnPatterns/SingletonPattern.md)
  - [Proxy](docs/SolidnPatterns/ProxyPattern.md)
  - [Command](docs/SolidnPatterns/CommandPattern.md)
  - [Publisher/Subscriber](docs/SolidnPatterns/PublisherSubscriberPattern.md)
  - [Dependency Injection](docs/SolidnPatterns/DependencyInjectionPattern.md)
  - [Specification](docs/SolidnPatterns/SpecificationPattern.md)

</p>
</details>

<details>
<summary>

## Azure
</summary>
<p>

</p>
</details>

<details>
<summary>

## ASP.Net Fundamentals
</summary>
<p>

- [Testing ASP.Net Core Application](docs/ASPNetCore/ASPNetCore_Testing.md)
- [Dependency Injection over Extension](docs/ASPNetCore/DependencyInjection_over_Extensions.md)
- [Configuration and IOptions, IOptionsSnapshot, IOptionsMonitor](docs/ASPNetCore/IOptionsnConfiguration.md)
</p>
</details>

<details>
<summary>

## Nuget Packages
</summary>
<p>

- **AutoFac**
- **SeriLog**
- **FluentValidation**
- **AutMapper**
</p>
</details>