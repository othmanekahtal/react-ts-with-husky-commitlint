![blog-img-husky](https://user-images.githubusercontent.com/57900722/165662166-ea3e94db-7a46-4623-be81-223acba59155.png)

# React + Typescript + Eslint + Prettier + Husky + commitLint starter boilerplate

This repo already contains all these tools pre-configure, just clone it and get
started with it

```bash
git clone https://github.com/othmanekahtal/react-ts-with-husky-commitlint.git && cd react-ts-with-husky-commitlint && yarn
```

## Create your own config :

The first of all, you will create your react application ⚛️:

```bash
yarn create react-app app-name --template typescript
```

**Note:** You can create your application react with pre-configure with redux or
react-router [for more info](https://create-react-app.dev/docs/custom-templates)

now after create your react app, we add husky to project and configure it :

```bash
npx husky-init && yarn
```

after that husky create a folder `.husky/` that contains `pre-commit` file, This
file by default run :

```bash
 npm test
```

you add all scripts you want be run before you commit your changes :

**Tips:** `pre-commit` file run as bash executable, you can add a
functions,conditions and more [check this !](.husky/pre-commit)

now your config almost done, we just need to add prettier for formatting your
code :

```bash
yarn add prettier -D
```

in root folder create `.prettierrc` file and put your rules
[check all rules here](https://prettier.io/docs/en/options.html) and install
prettier exention in your vscode, it's great addition

you can configure your prettier to format your code on save ,just add this rule
in your vscode :

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

all we need to finished is commitLint:

```bash
yarn add commitizen @commitlint/cli cz-conventional-changelog @commitlint/config-conventional -D
```

Add this configuration in your `package.json` :

```json
"config": {
    "commitizen": {
      "path": "./node_modules/cz-conventional-changelog"
    }
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  }
```

create commitlint.config.js file in your root folder

```json
module.exports = {
  /*
  * Resolve and load @commitlint/config-conventional from node_modules.
  * Referenced packages must be installed
  */
	extends: ['@commitlint/config-conventional'],
	/*
	 * Any rules defined here will override rules from @commitlint/config-conventional
	 */
	rules: {
		'type-enum': [
			2,
			'always',
			['build', 'chore', 'ci', 'docs', 'improvement', 'feat', 'fix', 'perf', 'refactor', 'revert', 'style', 'test'],
		],
	},
};
```

add in `package.json`:

```json
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "jest",
    "commit": "git-cz",
    "eject": "react-scripts eject",
    "prepare": "husky install",
    "lint": "eslint . --ext .ts",
    "check-types": "tsc --pretty --noEmit",
    "check-lint": "eslint . --ext ts --ext tsx --ext js",
    "prettier-write": "prettier --write .",
    "check-format": "prettier --check ."
  },
```

it's finished, you can add this great addition in husky `pre-commit` file:

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"


echo '😒😒😒😒wait a minute, we format your code 😒😒😒😒'

yarn prettier-write

yarn check-format ||
(
    echo '🤢🤮🤢🤮 Your styling looks disgusting. 🤢🤮🤢🤮'
    false;
)
# Check tsconfig standards
yarn check-types ||
(
    echo '🤡😂❌🤡 Failed Type check.Are you seriously trying to write that? Make the changes required above.🤡😂❌🤡'
    false;
)
# If everything passes... Now we can commit
echo '🤔🤔🤔🤔... Alright.... Code looks good to me... Trying to build now. 🤔🤔🤔🤔'
yarn build ||
(
    echo '❌👷🔨❌ What I say to you What I say to you ❌👷🔨❌'
    false;
)
# If everything passes... Now we can commit
echo '✅✅✅✅ You win this time... I am committing this now. ✅✅✅✅'

```

enjoy!
