# Pipeline-Function
Pipeline function like Pipeline operator

with typescript support
```typescript
/**
 * pipe function, like pipeline operator
 * ```ts
// without pipeline operator
double(increment(double(double(5)))); // 42

// with pipeline function
pipe(5)(double)(double)(increment)(double).value; // 42
// or
pipe(5).to(double).to(double).to(increment).to(double).value; // 42

// with pipeline operator
5 |> double |> double |> increment |> double; // 42
 * ```
 */
const pipe = <T>(value: T) => {
  const obj: Pipe<T> = (func) => pipe(func(value))
  obj.to = (func) => pipe(func(value))
  obj.value = value
  return obj
}

interface Pipe<T> {
  <S>(func: ((obj: T) => S)): Pipe<S>
  to: <S>(func: ((obj: T) => S)) => Pipe<S>
  value: T
}

const double = (n: number) => n * 2;
const increment = (n: number) => n + 1;

pipe(5)(double)(double)(increment)(double).value; // 42
pipe(5).to(double).to(double).to(increment).to(double).value; // 42
```

python version, with typing
```python
import typing

T = typing.TypeVar('T')
S = typing.TypeVar('S')
class pipe(typing.Generic[T]):
    """pipe function
        ```py
        # without pipeline operator
        double(increment(double(double(5)))) # 42

        # with pipeline function
        pipe(5)(double)(double)(increment)(double).value # 42
        pipe(5).to(double).to(double).to(increment).to(double).value # 42

        # with pipeline operator
        5 |> double |> double |> increment |> double; # 42
        ```
    """
    value:T
    def __init__(self,obj:T) -> None:
        self.value = obj

    def to(self,func:typing.Callable[[T],S]):
        return pipe(func(self.value))

    def __call__(self,func:typing.Callable[[T],S]):
        return pipe(func(self.value))

def double(n:int): return n * 2
def increment(n:int): return n + 1

pipe(5)(double)(double)(increment)(double)(hex)(print)
pipe(5).to(double).to(double).to(increment).to(double).to(hex).to(print)
```
