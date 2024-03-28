find the path of a value in a nested object recursively.
it includes a failsafe to stop it after x milliseconds
```javascript
function * nestedEntries(obj, path = [], timeout) {
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

  for (const [k, v] of Object.entries(obj)) {
    if (expired) {
      clearInterval(timeoutInterval);
      return; // Stop the generator
    }

    const currentPath = [...path, k];

    yield [currentPath, v];

    if (Object(v) === v) {
      yield * nestedEntries(v, currentPath, timeout);
    }
  }

  clearInterval(timeoutInterval);
}

const findKey = (obj, target, timeout) => {
  for (const [path, v] of nestedEntries(obj, [], timeout)) {
    if (v === target) return path;
  }
  return null;
};
```

how to use it
```javascript
const foo = {data01: 'rand01', data: {data21: 'rand', data2: { data3: 'worked' } }}

console .log (findKey (foo, 'worked', 500))
```

typescript version
```typescript
function* nestedEntries<T>(obj: Record<string, any>, path: string[], timeout: number): Generator<[string[], T], void, unknown> {
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

    for (const [k, v] of Object.entries(obj)) {
        if (expired) {
            clearInterval(timeoutInterval);
            return; // Stop the generator
        }

        const currentPath = [...path, k];

        yield [currentPath, v as T];

        if (Object(v) === v) {
            yield* nestedEntries<T>(v, currentPath, timeout);
        }
    }

    clearInterval(timeoutInterval);
}

export const findKey = <T>(obj: Record<string, any>, target: T, timeout: number): string[] | null => {
    for (const [path, v] of nestedEntries<T>(obj, [], timeout)) {
        if (v === target) return path;
    }
    return null;
};
```
