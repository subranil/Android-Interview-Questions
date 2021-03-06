# sealed classes

Sealed classes are used for representing restricted class hierarchies, when a value can have one of the types from a limited set, but cannot have any other type. They are, in a sense, an extension of enum classes: the set of values for an enum type is also restricted, but each enum constant exists only as a single instance, whereas a subclass of a sealed class can have multiple instances which can contain state.

To declare a sealed class, you put the `sealed` modifier before the name of the class. A sealed class can have subclasses, but all of them must be declared in the same file as the sealed class itself.

```
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```

Sealed class rules:
- Abstract and can have abstract members.
- Cannot be instantiated directly.
- Cannot have public constructors (The constructors are private by default).
- Can have subclasses, but they must either be in the same file or nested inside of the sealed class declaration.
- Subclass can have subclasses outside of the sealed class file.

The key benefit of using sealed classes comes into play when you use them in a when expression. If it's possible to verify that the statement covers all cases, you don't need to add an `else` clause to the statement. However, this works only if you use `when` as an expression (using the result) and not as a statement.

```
fun eval(expr: Expr): Double = when(expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
    // the `else` clause is not required because we've covered all the cases
}
```

## Links
https://kotlinlang.org/docs/reference/sealed-classes.html  
https://www.baeldung.com/kotlin-sealed-classes  
https://proandroiddev.com/understanding-kotlin-sealed-classes-65c0adad7015  
https://android.jlelse.eu/kotlin-what-is-a-sealed-classe-1e535c416519  
https://medium.com/androiddevelopers/sealed-with-a-class-a906f28ab7b5  
