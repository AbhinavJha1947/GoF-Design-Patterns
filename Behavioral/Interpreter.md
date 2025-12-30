# Interpreter Pattern

## Definition
The **Interpreter Pattern** is a behavioral design pattern that defines a grammatical representation for a language and provides an interpreter to deal with this grammar.

## Real-World Analogy
**Musicians**: Musical notes are a language. A musician is an interpreter. They read the notes (Expression) and produce music (Context). The same notes can be interpreted differently by different musicians, but the grammar remains the same.

## Structure
-   **Abstract Expression**: Declares an interpret operation that is common to all nodes in the abstract syntax tree.
-   **Terminal Expression**: Implements an interpret operation associated with terminal symbols in the grammar.
-   **Nonterminal Expression**: Implements an interpret operation for nonterminal symbols in the grammar.
-   **Context**: Contains information that's global to the interpreter.

## Code Example (C#)

```csharp
// Context
public class Context
{
    public string Input { get; set; }
    public int Output { get; set; }

    public Context(string input)
    {
        Input = input;
    }
}

// Abstract Expression
public abstract class Expression
{
    public void Interpret(Context context)
    {
        if (context.Input.Length == 0) return;

        if (context.Input.StartsWith(One()))
        {
            context.Output += (1 * Multiplier());
            context.Input = context.Input.Substring(2);
        }
        else if (context.Input.StartsWith(Four()))
        {
            context.Output += (4 * Multiplier());
            context.Input = context.Input.Substring(2);
        }
        else if (context.Input.StartsWith(Five()))
        {
            context.Output += (5 * Multiplier());
            context.Input = context.Input.Substring(2);
        }
        else if (context.Input.StartsWith(Nine()))
        {
            context.Output += (9 * Multiplier());
            context.Input = context.Input.Substring(2);
        }
        else
        {
            context.Output += (1 * Multiplier());
            context.Input = context.Input.Substring(1);
        }
    }

    public abstract string One();
    public abstract string Four();
    public abstract string Five();
    public abstract string Nine();
    public abstract int Multiplier();
}

// Use case: Roman Numeral to Integer Converter would have ThousandExpression, HundredExpression, etc.
```

## Pros & Cons
### Pros
-   **Extensibility**: It's easy to change and extend the grammar.
-   **Implementation**: Implementing the grammar is easy because classes representing rules have a similar structure.

### Cons
-   **Complexity**: Can become hard to maintain for complex grammars (class explosion).
-   **Performance**: Often slower than parsers or compilers for large inputs.

---

### Java Example

```java
interface Expression {
    boolean interpret(String context);
}

class TerminalExpression implements Expression {
    private String data;
    public TerminalExpression(String data) { this.data = data; }
    public boolean interpret(String context) {
        return context.contains(data);
    }
}

class OrExpression implements Expression {
    private Expression expr1 = null;
    private Expression expr2 = null;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
```

### C++ Example

```cpp
#include <iostream>
#include <string>

class Expression {
public:
    virtual bool interpret(std::string context) = 0;
    virtual ~Expression() {}
};

class TerminalExpression : public Expression {
private:
    std::string data;
public:
    TerminalExpression(std::string data) : data(data) {}
    bool interpret(std::string context) override {
        return context.find(data) != std::string::npos;
    }
};

class OrExpression : public Expression {
private:
    Expression* expr1;
    Expression* expr2;
public:
    OrExpression(Expression* e1, Expression* e2) : expr1(e1), expr2(e2) {}
    bool interpret(std::string context) override {
        return expr1->interpret(context) || expr2->interpret(context);
    }
};
```
