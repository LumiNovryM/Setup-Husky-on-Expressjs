# Setup-Husky-on-Expressjs
How to setup husky on your ExpressJs Application

#### Setup Husky on ExpressJs By Lumi Novry M

## Run This Step 

#### Install Eslint Husky Prettier 
```
npm install eslint prettier husky --save-dev
```

#### Exec husky
```
npx husky init
```

#### Create file .eslintrc.json
```
{
    "extends": ["eslint:recommended"],
    "parserOptions": {
        "ecmaVersion": 2021
    }
}
```

#### Create file .prettierrc.json
```
{
    "singleQuote": true,
    "trailingComma": "es5",
    "tabWidth": 2
}
```

#### Add this setting on your package.json code
```
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.js": [
      "eslint --fix",
      "prettier --write",
      "git add"
    ]
  }
```

#### Then replace code on ./husky/pre-commit
```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

#### Create new file on ./husky/commit-msg
```
npx commitlint --edit $1
```

#### Replace code on ./husky/_/husky.sh
```
#!/bin/sh
if [ -z "$husky_skip_init" ]; then
  debug () {
    if [ "$HUSKY_DEBUG" = "1" ]; then
      echo "husky (debug) - $1"
    fi
  }

  readonly hook_name="$(basename "$0")"
  debug "starting $hook_name..."

  if [ "$HUSKY" = "0" ]; then
    debug "HUSKY env variable is set to 0, skipping hook"
    exit 0
  fi

  if [ -f ~/.huskyrc ]; then
    debug "sourcing ~/.huskyrc"
    . ~/.huskyrc
  fi

  export readonly husky_skip_init=1
  sh -e "$0" "$@"
  exitCode="$?"

  if [ $exitCode != 0 ]; then
    echo "husky - $hook_name hook exited with code $exitCode (error)"
  fi

  exit $exitCode
fi
```