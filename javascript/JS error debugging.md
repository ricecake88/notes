# Table of Contents
[[#1. Checking if a key exists in a JavaScript object?]]

## 1. Checking if a key exists in a JavaScript object?

This is not a good solution:
```javascript
var obj = { key: undefined };
console.log(obj["key"] !== undefined); // false, but the key exists!
```
This is the better solution:
```javascript
var obj = { key: undefined };
console.log("key" in obj); // true, regardless of the actual value
```

Resource:
https://stackoverflow.com/questions/1098040/checking-if-a-key-exists-in-a-javascript-object