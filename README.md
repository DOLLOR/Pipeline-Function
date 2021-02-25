# Pipeline-Function
Pipeline function like Pipeline operator

See: https://medium.com/@venomnert/pipe-function-in-javascript-8a22097a538e

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
