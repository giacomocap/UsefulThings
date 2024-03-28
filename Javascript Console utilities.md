find the key of a value in a nested object recursively.
it includes a failsafe to stop it after x milliseconds
```javascript
function * nestedEntries(obj, timeout) {
  let expired = false;
  const startTime = Date.now();

  // Function to check if timeout has been reached
  const checkTimeout = () => {
    const elapsedTime = Date.now() - startTime;
    if (elapsedTime >= timeout) {
      expired = true;
    }
  };

  // Check timeout every 100 milliseconds
  const timeoutInterval = setInterval(checkTimeout, 100);

  for (let [k, v] of Object.entries(obj)) {
    if (expired) {
      clearInterval(timeoutInterval);
      return; // Stop the generator
    }

    yield [k, v];

    if (Object(v) === v) {
      yield * nestedEntries(v, timeout);
    }
  }

  clearInterval(timeoutInterval);
}

const findKey = (obj, target, timeout) => {
  for (let [k, v] of nestedEntries(obj, timeout)) {
    if (v === target) return k;
  }
  return null;
};
```

how to use it
```
const foo = {data01: 'rand01', data: {data21: 'rand', data2: { data3: 'worked' } }}

console .log (findKey (foo, 'worked'))
```
