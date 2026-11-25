# Corporate Esolang — Language Specification

**Version:** 0.1 — Draft
**Status:** Work in progress

## Table of Contents

1. [Program Structure](#1-program-structure)
2. [Data Types](#2-data-types)
3. [Variables](#3-variables)
4. [Departments and Operations](#4-departments-and-operations)
5. [Expressions](#5-expressions)
6. [Input and Output](#6-input-and-output)
7. [Conditional Branching](#7-conditional-branching)
8. [Loops](#8-loops)
9. [Functions](#9-functions)
10. [Exception Handling](#10-exception-handling)
11. [Complete Example — FizzBuzz](#11-complete-example--fizzbuzz)
12. [Architectural Rules](#12-architectural-rules)

---

# 1. Program Structure

Every program begins with:

```text
We need to talk:
```

And ends with:

```text
That's a wrap!
```

Example:

```text
We need to talk:

For visibility, "Hello World!"

That's a wrap!
```

## 1.1 Block Boundaries

All block-based constructs have explicit opening and closing keywords.

Indentation does not determine scope.

| Construct           | Opening                             | Closing                                   |
| ------------------- | ----------------------------------- | ----------------------------------------- |
| Program             | `We need to talk:`                  | `That's a wrap!`                          |
| Conditional         | `Let me check with my manager:`     | `Thank you for your time!`                |
| While loop          | `Let's keep the ball rolling until` | `Thank you for your time!`                |
| For-each loop       | `Let's touch base with each`        | `Thank you for your time!`                |
| Function definition | `Let's speak meeting:`              | `If that's everything, we're good to go.` |
| Exception handling  | `Let's manage expectations:`        | TBD                                       |

Nested blocks are supported. The parser associates closing keywords with their enclosing blocks.

## 1.2 Whitespace

- Indentation is optional.
- Blank lines are ignored outside string literals.
- Statements are normally separated by newlines.
- Multiword keywords are recognized as language constructs.
- Multiline expression continuation rules remain to be defined.

## 1.3 Comments

Single-line comments:

```text
// Just between us, this is a comment
```

Proposed multiline comments:

```text
Just between us:

    This is confidential.
    Nobody needs to know.

Keep it to yourself!
```

Comments have no effect on program execution.

---

# 2. Data Types

Data types describe the values stored by variables.

Departments are not data types.

## 2.1 Primitive Types

| Type             | Meaning                         | Example         |
| ---------------- | ------------------------------- | --------------- |
| `Number`         | Integer or floating-point value | `42`, `3.14`    |
| `Message`        | String                          | `"Hello"`       |
| `Decision`       | Boolean                         | `true`, `false` |
| `Thought Leader` | Void / no returned value        | —               |


## 2.2 Collection Types

Adding `s` to a data type creates a collection of that type.

| Singular   | Plural      | Meaning          |
| ---------- | ----------- | ---------------- |
| `Number`   | `Numbers`   | List of numbers  |
| `Message`  | `Messages`  | List of strings  |
| `Decision` | `Decisions` | List of booleans |

Example:

```text
Bringing on salaries as Numbers
Bringing on names as Messages
Bringing on approvals as Decisions
```

Collections are homogeneous: their elements must match their declared element type.

Collection nesting is not yet specified.

## 2.3 Numerical Representation

`Number` supports both integer and floating-point values.

Examples:

```text
42
3.14
-100
0.5
```

Explicit conversion between integer and floating-point representations is handled by Accountant operations.

The following behavior remains to be specified:

- Integer division
- Floating-point promotion
- Overflow
- Numeric precision
- Conversion and truncation

## 2.4 User-Defined Types

User-defined types are planned.

Working corporate terminology:

```text
Department
```

The syntax for defining fields, constructing objects, and accessing members is not yet established.

---

# 3. Variables

## 3.1 Declaration

Variables must be declared before initialization, assignment, or use.

Syntax:

```text
Bringing on <identifier> as <type>
```

Examples:

```text
Bringing on salary as a Number
Bringing on employee as a Message
Bringing on approved as a Decision
Bringing on salaries as Numbers
```

Declaration establishes the identifier and its type.

It does not automatically provide a usable value.

## 3.2 Initialization

Initialization is separate from declaration.

Syntax:

```text
Everyone, <identifier>, is joining us as <expression>
```

Example:

```text
Bringing on salary as a Number

Everyone, salary, is joining us as 50000
```

Initialization provides the variable's first value.

## 3.3 Reassignment

Syntax:

```text
Everyone, going forward <identifier> will be <expression>
```

Example:

```text
Everyone, going forward salary will be 60000
```

Reassignment requires the variable to have already been declared and initialized.

## 3.4 Invalid Operations

Assignment before declaration:

```text
Everyone, going forward salary will be 50000
```

Error if `salary` has not been declared.

Reading an uninitialized variable:

```text
Bringing on salary as a Number

For visibility, salary
```

Error because `salary` has not been initialized.

## 3.5 Scope

Variables belong to lexical scopes.

Function parameters and variables declared inside functions are local to their function.

Block-local declarations and shadowing rules remain to be finalized.

---

# 4. Departments and Operations

Departments are responsible for operating on values.

A department operation may return a value, modify a collection, or perform another explicitly defined effect.

## 4.1 Accountant — Numerical Operations

Accountant operates primarily on `Number` values.

### Arithmetic

```text
Accountant, can you please calculate: <expression>
```

Examples:

```text
Accountant, can you please calculate: salary * 0.15
Accountant, can you please calculate: a + b
Accountant, can you please calculate: x % 3
Accountant, can you please calculate: (a + b) / 2
```

Supported arithmetic operators:

| Operator | Operation      |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `*`      | Multiplication |
| `/`      | Division       |
| `%`      | Modulo         |

Arithmetic operators must be evaluated within an Accountant operation.

### Other Numerical Operations

```text
Accountant, can you round up: <value>
Accountant, can you round down: <value>
Accountant, can you audit: <value>
Accountant, can you find the variance: <value> VS <value>
Accountant, can you write off: <value>
Accountant, can you invoice: <value>
Accountant, can you reconcile: <value>
Accountant, can you forecast: <value>
```

| Operation           | Intended behavior             |
| ------------------- | ----------------------------- |
| `round up`          | Ceiling                       |
| `round down`        | Floor                         |
| `audit`             | Absolute value                |
| `find the variance` | Difference between two values |
| `write off`         | Produce zero                  |
| `invoice`           | Convert a value to a Message  |
| `reconcile`         | Numerical normalization, TBD  |
| `forecast`          | Projection, TBD               |

`write off` is proposed as a value-producing operation rather than automatically mutating its argument.

### Casting

Proposed explicit numeric conversions:

```text
Accountant, can you convert to integer: <value>
Accountant, can you convert to decimal: <value>
```

These operations return `Number` values with the requested internal numerical representation.

---

## 4.2 Manager — Boolean Operations

Manager handles logical operations and comparisons.

### Check

```text
Manager, can you check: <expression>
```

Examples:

```text
Manager, can you check: salary > 50000
Manager, can you check: approved OR contracted
Manager, can you check: number == 10
```

Returns a `Decision`.

### Negation

```text
Manager, can you deny: <expression>
```

Example:

```text
Manager, can you deny: approved
```

Returns the logical negation of the supplied Decision.

### Supported Operators

Logical:

```text
AND
OR
NOT
```

Comparisons:

```text
==
!=
>
<
>=
<=
```

Comparisons produce Decisions.

---

## 4.3 Marketing — String Operations

Marketing operates primarily on `Message` values.

```text
Marketing, can you soften: <value>
Marketing, can you amplify: <value>
Marketing, can you dial back: <value>
Marketing, can you bundle: <value> WITH <value>
Marketing, how long is: <value>
Marketing, can you locate: <value> IN <value>
Marketing, can you rebrand: <value> TO <value>
```

| Operation     | Meaning                  | Return type |
| ------------- | ------------------------ | ----------- |
| `soften`      | Text transformation, TBD | Message     |
| `amplify`     | Uppercase                | Message     |
| `dial back`   | Lowercase                | Message     |
| `bundle`      | Concatenation            | Message     |
| `how long is` | String length            | Number      |
| `locate`      | Find substring index     | Number      |
| `rebrand`     | Replace text             | Message     |

Example:

```text
Bringing on name as a Message
Bringing on result as a Message

Everyone, name, is joining us as "alice"

Everyone, result, is joining us as Marketing, can you amplify: name
```

The result is `"ALICE"`.

---

## 4.4 HR — Collection Operations

HR operates on plural data types.

Examples:

```text
Bringing on employees as Messages
Bringing on salaries as Numbers
Bringing on approvals as Decisions
```

### Collection Operations

```text
HR, can you onboard: <value> INTO <collection>
HR, can you let go of the last one: <collection>
HR, can you let go of the first one: <collection>
HR, can you fast track: <value> INTO <collection>

HR, can you introduce: AT <index> IN <collection>
HR, can you reassign: AT <index> TO <value> IN <collection>

HR, what's the headcount: <collection>
HR, can you locate: <value> IN <collection>
HR, can you confirm: <value> IS on board IN <collection>

HR, can you let go of: AT <index> IN <collection>
HR, can you rank: <collection>

HR, can you restructure: <collection> FROM <start> TO <end>

HR, can you put everyone through: <function> IN <collection>
HR, can you performance review: <collection> USING <function>
HR, can you consolidate: <collection> USING <function>
```

### Operation Semantics

| Operation                 | Meaning              |
| ------------------------- | -------------------- |
| `onboard`                 | Append               |
| `let go of the last one`  | Pop                  |
| `let go of the first one` | Shift                |
| `fast track`              | Prepend              |
| `introduce`               | Get element by index |
| `reassign`                | Set element by index |
| `headcount`               | Length               |
| `locate`                  | Index of             |
| `confirm`                 | Contains             |
| `let go of AT`            | Remove at index      |
| `rank`                    | Sort                 |
| `restructure`             | Slice                |
| `put everyone through`    | Map                  |
| `performance review`      | Filter               |
| `consolidate`             | Reduce               |

Mutation versus value-return behavior, indexing conventions, sorting rules, and callback signatures require further specification.

---

# 5. Expressions

Expressions produce values.

An expression may be:

- A literal
- A variable reference
- A departmental operation
- A function call
- A parenthesized expression

## 5.1 Departmental Expressions

Examples:

```text
Accountant, can you please calculate: salary * 0.15
Manager, can you check: approved OR contracted
Marketing, can you amplify: name
HR, what's the headcount: employees
```

Departmental expressions can appear wherever an expression of the appropriate type is expected.

## 5.2 Assignment from Department Operations

```text
Bringing on bonus as a Number

Everyone, bonus, is joining us as Accountant, can you please calculate: salary * 0.15
```

## 5.3 Nested Expressions

Proposed parenthesized syntax:

```text
Manager, can you check: (Accountant, can you please calculate: number % 3) == 0
```

The inner Accountant expression returns a Number.

The outer Manager expression returns a Decision.

A formal grammar will need to specify where each departmental expression ends.

## 5.4 Type Checking

The language is statically typed.

An expression's resulting type must be compatible with its destination.

For example, a Marketing length operation returns a Number, even though Marketing primarily operates on Messages.

---

# 6. Input and Output

## 6.1 Printing

Syntax:

```text
For visibility, <expression>
```

Examples:

```text
For visibility, "Hello World!"
For visibility, salary
For visibility, approved
```

Printing converts the supplied value to a textual representation.

The exact formatting rules for numbers, booleans, and collections remain to be defined.

## 6.2 Input

User input syntax has not yet been finalized.

---

# 7. Conditional Branching

## 7.1 Conditional Block

Opening:

```text
Let me check with my manager:
```

Closing:

```text
Thank you for your time!
```

## 7.2 If

```text
Provided that <condition>:
    <statements>
```

## 7.3 Else If

```text
That said <condition>:
    <statements>
```

## 7.4 Else

```text
Worst case:
    <statements>
```

## 7.5 Complete Example

```text
Let me check with my manager:

    Provided that Manager, can you check: salary > 100000:
        For visibility, "Promotion approved"

    That said Manager, can you check: salary > 50000:
        For visibility, "Under review"

    Worst case:
        For visibility, "Budget cuts"

Thank you for your time!
```

Conditions must evaluate to Decisions.

Only the first matching branch executes.

`Worst case` executes when no preceding condition is satisfied.

---

# 8. Loops

## 8.1 While Loop

```text
While we're still working on <condition>:
    <statements>
Thank you for your time!
```

The loop continues while its condition is true.

## 8.2 Until Loop

```text
Let's keep the ball rolling until <condition>:
    <statements>
Thank you for your time!
```

The loop continues until its condition becomes true.

The condition is evaluated before each iteration.

## 8.3 For-Each Loop

```text
Let's touch base with each <element> in <collection>:
    <statements>
Thank you for your time!
```

Example:

```text
Let's touch base with each employee in employees:
    For visibility, employee
Thank you for your time!
```

The loop element is a locally bound identifier whose type matches the collection element type.

## 8.4 Break

```text
Let's take this offline.
```

Terminates the nearest enclosing loop.

## 8.5 Continue

```text
Moving on to the next item.
```

Skips to the next iteration of the nearest enclosing loop.

---

# 9. Functions

Functions use the corporate meeting metaphor.

Function declaration, definition, and invocation are distinct operations.

## 9.1 Function Declaration

```text
Let's book a meeting: <name>
Attendees: <type>, <type>, ...
Delivers: <return_type>
```

Example:

```text
Let's book a meeting: calculateBonus
Attendees: Number, Number
Delivers: Number
```

The declaration specifies the function signature without parameter names or implementation.

## 9.2 Function Definition

Proposed syntax:

```text
Let's speak meeting: <name>
Attendees: <parameter_names>

    <statements>

If that's everything, we're good to go.
```

Example:

```text
Let's speak meeting: calculateBonus
Attendees: salary, rate

    Going forward, the outcome will be Accountant, can you please calculate: salary * rate

If that's everything, we're good to go.
```

The definition matches the previously declared function signature.

Parameter types are resolved positionally from the declaration.

## 9.3 Function Invocation

The original meeting-call syntax is retained as a candidate:

```text
Meeting in 5: <name>
Attendees: <arguments>
```

Example:

```text
Meeting in 5: calculateBonus
Attendees: 50000, 0.15
```

The expression form of a multiline meeting invocation, especially when used inside assignments, remains to be finalized.

## 9.4 Return

```text
Going forward, the outcome will be <expression>
```

The returned expression must match the function's declared return type.

For a `Thought Leader` function, reaching the closing keyword may terminate execution without returning a value.

## 9.5 Function Rules

- Functions must be declared before definition and invocation.
- Definitions must match their declared signatures.
- Attendee names are local to their function.
- Arguments are matched positionally.
- Return values are type-checked.
- Recursion is permitted once the function declaration is in scope.
- Overloading and default arguments remain unspecified.

---

# 10. Exception Handling

## 10.1 Throw

```text
I'd like to escalate: <error>
```

## 10.2 Try

```text
Let's manage expectations:
    <statements>
```

## 10.3 Catch

```text
In light of recent developments:
    <statements>
```

The exception block closing phrase, error types, and catch binding syntax remain TBD.

---

# 11. Complete Example — FizzBuzz

This example uses the proposed inline departmental-expression syntax.

```text
We need to talk:

Bringing on number as a Number

Everyone, number, is joining us as 1

Let's keep the ball rolling until Manager, can you check: number > 100:

    Let me check with my manager:

        Provided that Manager, can you check: (Accountant, can you please calculate: number % 15) == 0:
            For visibility, "FizzBuzz"

        That said Manager, can you check: (Accountant, can you please calculate: number % 3) == 0:
            For visibility, "Fizz"

        That said Manager, can you check: (Accountant, can you please calculate: number % 5) == 0:
            For visibility, "Buzz"

        Worst case:
            For visibility, number

    Thank you for your time!

    Everyone, going forward number will be Accountant, can you please calculate: number + 1

Thank you for your time!

That's a wrap!
```

Expected behavior: print numbers from 1 to 100, replacing multiples of 3 with `Fizz`, multiples of 5 with `Buzz`, and multiples of both with `FizzBuzz`.

---

# 12. Architectural Rules

## Rule 1 — Declaration Before Initialization

Every identifier must be declared before receiving or supplying a value.

## Rule 2 — Initialization Before Read

An uninitialized variable cannot be read.

## Rule 3 — Explicit Block Boundaries

Blocks begin and end with designated keywords.

Indentation is not semantically significant.

## Rule 4 — Departments Operate on Values

Departments perform operations. They are not types.

## Rule 5 — Plural Types Are Collections

Pluralization creates a collection of the singular type.

## Rule 6 — Arithmetic Requires Accountant

Arithmetic operators are evaluated within Accountant expressions.

## Rule 7 — Conditions Produce Decisions

Loop and conditional expressions must evaluate to `Decision`.

## Rule 8 — Expressions Are Composable

Departmental operations and function calls can supply values to other expressions.

## Rule 9 — Type Compatibility Is Enforced

Assignments, parameters, and returns must satisfy declared types.

## Rule 10 — Function Signatures Precede Implementations

Functions are booked before they are defined or called.

---

#
