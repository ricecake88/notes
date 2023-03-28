# Table of Contents
[[#No index signature with a parameter of type 'string' was found on type]]
[[#ts-node' is not recognized as an internal or external command, operable program or batch file]]


## 1. No index signature with a parameter of type 'string' was found on type #

**The error "No index signature with a parameter of type 'string' was found on type" occurs when we use a value of type `string` to index an object with specific keys.**

**To solve the error, type the `string` as one of the object's keys using `keyof typeof obj`.**

Resource: 
https://bobbyhadz.com/blog/typescript-no-index-signature-with-parameter-of-type-string#no-index-signature-with-a-parameter-of-type-string-was-found-on-type

## 2. ts-node' is not recognized as an internal or external command, operable program or batch file

Solution:
You need to install ts-node as global

```javascript
npm install -g ts-node
```

Resource:
https://stackoverflow.com/questions/44764004/ts-node-is-not-recognized-as-an-internal-or-external-command-operable-program

