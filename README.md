# Pipeline-Function
Pipeline function like Pipeline operator

See: https://medium.com/@venomnert/pipe-function-in-javascript-8a22097a538e
https://github.com/tc39/proposal-pipeline-operator

```javascript
const _pipe = (a, b) => (arg) => b(a(arg));
const pipe = (...ops) => ops.reduce(_pipe);

const double = (n) => n * 2;
const increment = (n) => n + 1;

// without pipeline operator
double(increment(double(double(5)))); // 42

// with pipeline operator
5 |> double |> double |> increment |> double; // 42

// with pipeline function
pipe(double,double,increment,double)(5);
```

```javascript
// Here’s the async/await version of the pipe function.
// This allows you to provide async/await functions inside of the pipe.
const _pipe = (a, b) => async (arg) => b(await a(arg));
//or
const _pipe = (a, b) => (arg) => {
  return Promise.resolve(a(arg)).then(result=>b(result));
};
const pipe = (...ops) => ops.reduce(_pipe);
```

```javascript
// Another version, with just one line
// from https://medium.com/javascript-scene/reduce-composing-software-fe22f0c39a1d
// and https://www.freecodecamp.org/news/pipe-and-compose-in-javascript-5b04004ac937/
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);
```
```javascript
// es3 version
var pipe = function() {
  var funcList = arguments;
  return function(item){
    var i;
    var func;
    for(i=0;i<funcList.length;i++){
      func = funcList[i];
      item = func(item);
    }
    return item;
  };
};
```
another way, with typescript support
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
