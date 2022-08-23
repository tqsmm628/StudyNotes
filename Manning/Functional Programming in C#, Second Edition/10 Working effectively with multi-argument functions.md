# 10.1 Function application in the elevated world
```csharp
var doubl = (int i) => i * 2;

Some(3).Map(doubl) // => Some(6)
```
Listing 10.1 Mapping a curried function onto an Option
```csharp
var multiply = (int x) => (int y) => x * y;
 
var multBy3 = Some(3).Map(multiply);
// => Some(y => 3 * y))
```
```
multiply              : int → int → int
Some(3)               : Option<int>
Some(3).Map(multiply) : Option<int → int>
```
```
Map : F<T> → (T → R) → F<R>
```
```
F<T> → (T → T1 → T2) → F<T1 → T2>
```
```csharp
public static Option<Func<T2, R>> Map<T1, T2, R>
   (this Option<T1> opt, Func<T1, T2, R> func)
   => opt.Map(func.Curry());
```
Listing 10.2 Mapping a binary function onto an Option
```csharp
var multiply = (int x, int y) => x * y;
 
var multBy3 = Some(3).Map(multiply);
multBy3 // => Some(y => 3 * y))
```
Figure 10.1 Mapping a binary function onto an Option yields a unary function wrapped in an Option.
![](https://drek4537l1klr.cloudfront.net/buonanno2/HighResolutionFigures/figure_10-1.png)
```csharp
var multiply = (int x, int y) => x * y;
 
Option<int> optX = Some(3)
          , optY = Some(4);
 
var result = optX.Map(multiply).Match
(
   () => None,
   (f) => optY.Match
   (
      () => None,
      (y) => Some(f(y))
   )
);
 
result // => Some(12)
```
## 10.1.1 Understanding applicatives
```
Apply : (T → R) → T → R
Apply : (T1 → T2 → R) → T1 → (T2 → R)
Apply : (T1 → T2 → T3 → R) → T1 → (T2 → T3 → R)
```
```
Apply : A<T → R> → A<T> → A<R>
Apply : A<T1 → T2 → R> → A<T1> → A<T2 → R>
Apply : A<T1 → T2 → T3 → R> → A<T1> → A<T2 → T3 → R>
```
Figure 10.2 Apply performs function application in the elevated world.
![](https://drek4537l1klr.cloudfront.net/buonanno2/HighResolutionFigures/figure_10-2.png)

Listing 10.3 Implementation of Apply for Option
```csharp
public static Option<R> Apply<T, R>
(
   this Option<Func<T, R>> optF,
   Option<T> optT
)
=> optF.Match
(
   () => None,
   (f) => optT.Match
   (
      () => None,
      (t) => Some(f(t))
   )
);
 
public static Option<Func<T2, R>> Apply<T1, T2, R>
(
   this Option<Func<T1, T2, R>> optF,
   Option<T1> optT
)
=> Apply(optF.Map(F.Curry), optT);
```
```csharp
var multiply = (int x, int y) => x * y;
 
Some(3).Map(multiply).Apply(Some(4));
// => Some(12)
 
Some(3).Map(multiply).Apply(None);
// => None
```
## 10.1.2 Lifting functions
```csharp
Some(3).Map(multiply)
```
```csharp
Some(multiply)
   .Apply(Some(3))
   .Apply(Some(4))
 
// => Some(12)
```
```csharp
Some(multiply)
   .Apply(None)
   .Apply(Some(4))
// => None
```
![](https://drek4537l1klr.cloudfront.net/buonanno2/HighResolutionFigures/table_10-1.png)
![](https://drek4537l1klr.cloudfront.net/buonanno2/HighResolutionFigures/table_10-2.png)
## 10.1.3 An introduction to property-based testing
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
```csharp
```
# 10.2 Functors, applicatives, and monads
# 10.3 The monad laws
## 10.3.1 Right identity
## 10.3.2 Left identity
## 10.3.3 Associativity
## 10.3.4 Using Bind with multi-argument functions
# 10.4 Improving readability by using LINQ with any monad
## 10.4.1 Using LINQ with arbitrary
## 10.4.2 Using LINQ with arbitrary
## 10.4.3 The LINQ clauses let, where, and others
# 10.5 When to use Bind vs. Apply
## 10.5.1 Validation with smart constructors
## 10.5.2 Harvesting errors with the applicative flow
## 10.5.3 Failing fast with the monadic flow