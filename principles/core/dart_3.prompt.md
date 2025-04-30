# Dart 3: Advanced Pattern Matching, Branching, and Records

## Conditional Branching and Control Flow

- Use `if` statements for boolean-based branching; add `else` or `else if` for alternative flows.
- `if-case` enables pattern matching and variable extraction in a single branch; variables are scoped to the matching block.
- If the pattern in `if-case` fails, execution continues to the `else` block if present.
- `switch` statements match values against multiple patterns; each `case` can use any pattern type.
- Dart's `switch` does not require `break` after non-empty cases; use `continue`, `throw`, or `return` to exit early.
- Use `default` or `_` in `switch` to handle unmatched cases; empty cases fall through unless `break` is used.
- Use `continue` with a label for non-sequential fallthrough between cases.
- Logical-or patterns (e.g., `case a || b`) allow multiple alternatives in a single case.
- Logical-or patterns can be combined with guards (`when`) to share a body or restrict matches.
- `switch` expressions return values based on pattern matches; use `=>` for bodies and separate cases with commas.
- In `switch` expressions, the default case must use `_` (not `default`).
- Dart checks for exhaustiveness in `switch` statements and expressions, reporting a compile-time error if not all possible values are handled.
- To ensure exhaustiveness, use a default (`default` or `_`) case, or switch over enums or sealed types.
- Use the `sealed` modifier on a class to enable exhaustiveness checking when switching over its subtypes.
- Add guard clauses with `when` to restrict when a case matches; guards are checked after pattern matching.
- Guard clauses can be used in `if-case`, `switch` statements, and `switch` expressions. The guard is evaluated after pattern matching.
- If a guard clause evaluates to false, execution proceeds to the next case (does not exit the switch).

## Pattern Matching Fundamentals

- Patterns describe the structure or type of values for matching and destructuring.
- Pattern matching checks if a value has a certain shape, constant, equality, or type.
- Pattern destructuring allows extracting parts of a matched value and binding them to variables.
- Patterns can be nested for recursive matching; use `_` to ignore values or `...` in lists to skip remaining elements.
- Patterns can be used in:
  - Local variable declarations and assignments
  - For and for-in loops
  - If-case and switch-case statements
  - Control flow in collection literals
- Pattern variable declarations start with `var` or `final` and bind new variables from the matched value. Example: `var (a, [b, c]) = ('str', [1, 2]);`
- Pattern variable assignments destructure a value and assign to existing variables. Example: `(b, a) = (a, b); // swap values`
- Every case clause in `switch` and `if-case` contains a pattern. Any kind of pattern can be used in a case.
- Case patterns are refutable; if the pattern doesn't match, execution continues to the next case.
- Destructured values in a case become local variables scoped to the case body.
- Logical-or (`||`) and logical-and (`&&`) patterns combine multiple matching conditions; all branches must bind the same variables.
- Relational patterns (`==`, `!=`, `<`, `>`, `<=`, `>=`) match values based on comparison to constants.
- Use cast patterns (`as Type`), null-check (`?`), and null-assert (`!`) to refine matching and handle nullability.
- List, map, record, and object patterns destructure collections, maps, records, and objects by shape or keys.
- Patterns simplify extracting and validating complex data, such as JSON, in a concise and declarative way. Example: `if (data case {'user': [String name, int age]}) { ... }`
- Patterns provide a concise alternative to verbose type-checking and destructuring code.

## Pattern Types and Structure

- Pattern precedence determines evaluation order; use parentheses to group lower-precedence patterns.
- Logical-or patterns (`pattern1 || pattern2`) match if any branch matches, evaluated left-to-right. All branches must bind the same set of variables.
- Logical-and patterns (`pattern1 && pattern2`) match if both subpatterns match. Bound variable names must not overlap between subpatterns.
- Relational patterns (`==`, `!=`, `<`, `>`, `<=`, `>=`) match if the value compares as specified to a constant. Useful for numeric ranges and can be combined with logical-and.
- Cast patterns (`subpattern as Type`) assert and cast a value to a type before passing it to a subpattern. Throws if the value is not of the type.
- Null-check patterns (`subpattern?`) match if the value is not null, then match the inner pattern. Binds the non-nullable type. Use constant pattern `null` to match null.
- Null-assert patterns (`subpattern!`) match if the value is not null, else throw. Use in variable declarations to eliminate nulls. Use constant pattern `null` to match null.
- Constant patterns match if the value is equal to a constant (number, string, bool, named constant, const constructor, const collection, etc.). Use parentheses and `const` for complex expressions.
- Variable patterns (`var name`, `final Type name`) bind new variables to matched/destructured values. Typed variable patterns only match if the value has the declared type.
- Identifier patterns (`foo`, `_`) act as variable or constant patterns depending on context. `_` always acts as a wildcard and matches/discards any value.
- Parenthesized patterns (`(subpattern)`) control pattern precedence and grouping, similar to expressions.
- List patterns (`[subpattern1, subpattern2]`) match lists and destructure elements by position. The pattern length must match the list unless a rest element is used.
- Rest elements (`...`, `...rest`) in list patterns match arbitrary-length lists or collect unmatched elements into a new list.
- Map patterns (`{"key": subpattern}`) match maps and destructure by key. Only specified keys are matched; missing keys throw a `StateError`.
- Record patterns (`(subpattern1, subpattern2)`, `(x: subpattern1, y: subpattern2)`) match records by shape and destructure positional/named fields. Field names can be omitted if inferred from variable or identifier patterns.
- Object patterns (`ClassName(field1: subpattern1, field2: subpattern2)`) match objects by type and destructure using getters. Extra fields in the object are ignored.
- Wildcard patterns (`_`, `Type _`) match any value without binding. Useful for ignoring values or type-checking without binding.
- All pattern types can be nested and combined for expressive and precise matching and destructuring.

## Comprehensive Guide to Records

- Records are anonymous, immutable, aggregate types that bundle multiple objects into a single value.
- Records are fixed-sized, heterogeneous, and strongly typed. Each field can have a different type.
- Records are real values: store them in variables, nest them, pass to/from functions, and use in lists, maps, and sets.
- Record expressions use parentheses with comma-delimited positional and/or named fields, e.g. `('first', a: 2, b: true, 'last')`.
- Record type annotations use parentheses with comma-delimited types. Named fields use curly braces: `({int a, bool b})`.
- The names of named fields are part of the record's type (shape). Records with different named field names have different types.
- Positional field names in type annotations are for documentation only and do not affect the record's type.
- Record fields are accessed via built-in getters: positional fields as `$1`, `$2`, etc., and named fields by their name (e.g., `.a`).
- Records are immutable: fields do not have setters.
- Records are structurally typed: the set, types, and names of fields define the record's type (shape).
- Two records are equal if they have the same shape and all corresponding field values are equal. Named field order does not affect equality.
- Records automatically define `hashCode` and `==` based on structure and field values.
- Use records for functions that return multiple values; destructure with pattern matching: `var (name, age) = userInfo(json);`
- Destructure named fields with the colon syntax: `final (:name, :age) = userInfo(json);`
- Using records for multiple returns is more concise and type-safe than using classes, lists, or maps.
- Use lists of records for simple data tuples with the same shape.
- Use type aliases (`typedef`) for record types to improve readability and maintainability.
- Changing a record type alias does not guarantee all code using it is still type-safe; only classes provide full abstraction/encapsulation.
- Extension types can wrap records but do not provide full abstraction or protection.
- Records are best for simple, immutable data aggregation; use classes for abstraction, encapsulation, and behavior.
