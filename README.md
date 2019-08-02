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
