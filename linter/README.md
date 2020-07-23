# Linter rules vscode environment setup

#### Linter's setup
[here will be attached eslint-divisio when this one will be updated, for now ignore this step]

#### Husky + lint-staged setup
Put this in your package.json
```
  "scripts": {
    ...
    "lint": "eslint src/** --max-warnings=0",
    ...
  },
  "lint-staged": {
    "src/**/*": [
      "yarn lint --fix"
    ]
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
```

#### Setup your environment

- Make sure you have this vscode plugins installed:
- All lint fixes must run by husky + lintstange script. But to facilite it's recoomendable to format code when save the files, so put this in your .vscode/settings.json (create if the project don't have one)
```
  "editor.formatOnSave": false,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
```