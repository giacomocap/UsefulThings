find the key of a value in a nested object recursively
```
function * nestedEntries (obj) {
  for (let [k, v] of Object .entries (obj)) {
    yield [k, v]
    if (Object (v) === v) {yield * nestedEntries (v)}
  }
}

const findKey = (obj, target) => {
  for (let [k, v] of nestedEntries (obj)) {
    if (v === target) return k
  }
  return null
}
```

how to use it
```
const foo = {data01: 'rand01', data: {data21: 'rand', data2: { data3: 'worked' } }}

console .log (findKey (foo, 'worked'))
```
