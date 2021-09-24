https://losikov.medium.com/part-4-node-js-express-typescript-unit-tests-with-jest-5204414bf6f0

### VS Code

Install vscode extension by Orta.

Auto run jest on vscode startup is configured currently.

### Testing function that throws

You need to wrap the function that is expected to throw as follows:

```js
expect(() => getMapWithValueArray({}, '')).toThrowError();
```

### Jest HTML reporter

HTML reports
