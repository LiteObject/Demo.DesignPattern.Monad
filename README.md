# Demo DesignPattern Monad (with C#)
>Monad is a design pattern used to describe computations as a series of steps.

Let's take the Maybe monad as an example. The Maybe monad is used to represent optional values or computations that may or may not have a result. It is helpful for handling null or missing values in a more functional and expressive way. Here's an example using the Maybe monad in C#:

```csharp
using System;

public class Maybe<T>
{
    private readonly T value;
    private readonly bool hasValue;

    private Maybe(T value)
    {
        this.value = value;
        this.hasValue = true;
    }

    public static Maybe<T> Some(T value)
    {
        return new Maybe<T>(value);
    }

    public static Maybe<T> None()
    {
        return new Maybe<T>(default(T));
    }

    public TResult Match<TResult>(Func<T, TResult> someFunc, Func<TResult> noneFunc)
    {
        if (hasValue)
        {
            return someFunc(value);
        }
        else
        {
            return noneFunc();
        }
    }
}
```
In the code above, we define a generic `Maybe<T>` class that represents an optional value of type `T`. The `Maybe<T>` class has two static factory methods: `Some` to create a `Maybe<T>` instance with a value, and `None` to create a `Maybe<T>` instance without a value.

The `Match` method allows us to pattern match on the `Maybe<T>` instance. It takes two functions: `someFunc` is called when the `Maybe<T>` instance has a value, and `noneFunc` is called when the instance doesn't have a value. This allows us to perform different computations based on the presence or absence of a value.

Let's see an example usage of the Maybe monad:

```csharp
Maybe<int> CalculateSquareRoot(double number)
{
    if (number >= 0)
    {
        return Maybe<int>.Some((int)Math.Sqrt(number));
    }
    else
    {
        return Maybe<int>.None();
    }
}

void Main()
{
    double input = 25;
    var result = CalculateSquareRoot(input).Match(
        value => $"Square root of {input} is {value}",
        () => $"Invalid input: {input}"
    );
    Console.WriteLine(result);
}
```
In this example, the `CalculateSquareRoot` function returns a `Maybe<int>` instance that represents the square root of a given number. If the number is non-negative, it returns `Some` with the square root value. Otherwise, it returns `None`.

In the `Main` method, we call `CalculateSquareRoot` with the input value of 25. We then use the `Match` method to handle the result. If the `Maybe<int>` instance has a value, we print the square root. Otherwise, we print an error message.

The Maybe monad helps us handle the case where the square root may not exist (for negative numbers) in a more functional and expressive manner. It promotes a more structured and composable approach to dealing with optional values.
