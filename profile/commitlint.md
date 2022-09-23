# commitlint

commitlint는 커밋 메시지에 대해서 lint를 할 수 있게 해주는 툴입니다.

깔끔한 커밋 메시지와 함께 git history를 관리하면 유지 보수에 큰 도움이 됩니다.

또한, 커밋 메시지를 규격화해서 작성하면 커밋 메시지를 바탕으로 CHANGELOG 나 Release Notes를 자동으로 작성하게 할 수도 있고, semver 같은 툴을 이용하면 커밋 메시지에 따라 자동으로 버전을 올려주는 커밋이 생성되는 기능 등을 툴로 구현할 수 있게 됩니다.

반대로 말하면 이러한 툴들이 올바르게 동작하게 만들려면 커밋 메시지에 오타가 있는지, 규격에 맞게 작성되었는지 등을 검사해야 합니다.

이때 필요한 툴이 commitlint 입니다. 설치 방법은 다음과 같습니다.

```bash
npm install husky --save-dev
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit'
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

루니버스 개발팀은 커밋 메시지 작성 시 conventional commits (https://www.conventionalcommits.org/ko/v1.0.0-beta.4/) 규격을 따르고 있어서, 해당 규격에 맞는 preset을 함께 설치했습니다.

package.json 수정사항은 다음과 같습니다.

```json
// ...생략
"scripts": {
  // ...생략
  "prepare": "husky install",
  "commitlint": "commitlint -e",
  // ...생략
},
// ...생략
"commitlint": {
  "extends": [
    "@commitlint/config-conventional"
  ]
},
// ...생략
```

.commitlintrc.json 파일을 생성한다.
```json
{
    "extends": ["@commitlint/config-angular"],
    "rules": {
      "subject-case": [
        2,
        "always",
        ["sentence-case", "start-case", "pascal-case", "upper-case", "lower-case"]
      ],
      "type-enum": [
        2,
        "always",
        [
          "build",
          "chore",
          "ci",
          "docs",
          "feat",
          "fix",
          "perf",
          "refactor",
          "revert",
          "style",
          "test",
          "sample"
        ]
      ]
    }
  }
```

여기까지 설정했다면, 이제부터는 git commit 시 커밋 메시지가 규격에 맞지 않게 작성된 경우 다음과 같이 오류가 발생하게 됩니다.

```bash
❯ git commit -m "foo: bar"

> test-service@1.0.0 lint-staged
> lint-staged

ℹ No staged files match any configured task.

> test-service@1.0.0 commitlint
> commitlint -e

⧗   input: foo: bar
✖   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]

✖   found 1 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint
```

commitlint의 자세한 커밋 메시지 검증 룰은 https://www.npmjs.com/package/@commitlint/config-conventional 을 참고하면 되고, 커스터마이징이 필요한 경우 https://commitlint.js.org/ 를 참고하면 됩니다.
