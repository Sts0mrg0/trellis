
A `Spek` is any object that implements the following interface:

```kotlin
interface Spek<T, U> {
    suspend fun evaluate(visitor: SpekVisitor, candidate: T): U
}
```

You'll notice that the method is marked with `suspend`, which means it is equipped to be run as a Kotlin Coroutine, and 
so building complex speks from other speks is particularly nice in Kotlin. Using this library from Java, the equivalent 
interface is similar, but a callback is used instead of a return type:

```java
public interface Spek<T, U> {
    void evaluate(SpekVisitor visitor, T input, Continuation<U> callback);
}
```

There are 3 other important classes which may be of use when chaining speks together, the `ValueSpek`, the 
`CandidateSpek`, and the `EqualsSpek`. 

`ValueSpek` wraps a single value passed to its constructor, and the `evaluate()` method just returns that value. This is
useful for parameterizing your spec, so that thresholds can be set without changing the spec model itself, and so give
some of the more abstract speks concrete values to check against. Many of the spek extension functions include a method
which accepts either a Spek or a raw value, and the raw value is just wrapped up in a spek behind-the-scenes.

`CandidateSpek` returns the candidate directly. 

The `EqualsSpek` checks for the equality of the result of 2 other speks. If the values are both instances of `Number`, 
they are converted to `Double`s first before checking equality. All other values are compared using `.equals()`.