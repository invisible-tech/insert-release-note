# @invisible/release-note

[![CircleCI](https://circleci.com/gh/invisible-tech/release-note/tree/master.svg?style=svg)](https://circleci.com/gh/invisible-tech/release-note/tree/master)

Provides two helper functions to publish release notes.

## assert-release-note

Asserts if there is a release note in the current Pull Request.

## push-release-note

Pushes the release note to Slack.

# Install

1. Install the package as devDependency:
```sh
yarn add -D @invisible/release-note
# or
npm install -D @invisible/release-note
```

2. If you wish to use the `push-release-note` method, set up a [Slack Webhook](https://my.slack.com/services/new/incoming-webhook/). NOTE: Slack will reject mutliple `POST`'s to the same webhook that have identical messages, so you might run into this while testing.

# Usage

## Programmatically

```javascript
  'use strict'

  const {
    assertReleaseNote,
    pushReleaseNote,
  } = require('@invisible/release-note')

  // changelogFile defaults to CHANGELOG.md if no argument given
  // This method will throw if no addition has been made to your changelogFile since
  // the last merge commit
  assertReleaseNote({ changelogFile: 'CHANGELOG.txt' })

  const webhookUrl = process.env.CHANGELOG_WEBHOOK_URL

  // This method is async so it returns a promise that resolves the Response object from POST'ing to the webhook
  pushReleaseNote({
    changelogFile: 'CHANGELOG.txt', // defaults to CHANGELOG.md
    iconEmoji: 'joy', // defaults to :robot_face:
    slackbotName: 'Cool Bot Name' , // defaults to Changelog
    webhookUrl,
  }).then(console.log).catch(console.error)

```

## Hook scripts

### `assert-release-note`
1. Append `assert-release-note` to `posttest` on `scripts` section of your `package.json`.
```json
  // It would look something like this:
  "scripts": {
    "posttest": "assert-release-note"
  }
```

You can also run it at any time from your CLI.
```
$ assert-release-note
```

### `push-release-note`
1. Add to the `deployment` section of your project `circle.yml` file the following:
`- push-release-note`

```yaml
# Your circle.yml should look like the below:
deployment:
  production:
    branch: master
    commands:
      - push-release-note
```

You can also run it at any time from your CLI.
```
$ push-release-note
```

2. Optional: set a name for your slack bot and an icon emoji in your `package.json`

```JSON
  "releaseNote": {
    "slackbotName": "Changelog Robot",
    "iconEmoji": "joy"
  }
```

3. If using Circle CI, add the `CHANGELOG_WEBHOOK_URL` variable to your project. This package will optionally load `dotenv` if it's present, so you may add this to your `.env` file as well.

## TODO
- Unit Tests
- Testing on multiple platforms
