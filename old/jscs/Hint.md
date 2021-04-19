# Hint

```sh
touch .jscsrc || exit
```

```json
{
  "preset": "google",
  "disallowSpacesInAnonymousFunctionExpression": null,
  "validateLineBreaks": "LF",
  "validateIndentation": 2,
  "excludeFiles": ["build/**", "node_modules/**"],
  "esprima": "esprima-fb"
}
```