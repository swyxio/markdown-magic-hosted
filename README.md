# Markdown Steriods

Automatically keep markdown files up to date with external sources and code snippets

## Install

`npm install markdown-steriods --save-dev`

## Usage

**Built in commands:**
<!-- AUTO-GENERATED-CONTENT:START (LIST_COMMANDS) - Do not remove or modify this section -->
- `CODE` Get code from file or URL and put in markdown
- `REMOTE` Get any remote Data and put in markdown
<!-- AUTO-GENERATED-CONTENT:END - Do not remove or modify this section -->

<!-- AUTO-GENERATED-CONTENT:START (CODE:src=./example/example.js) - Do not remove or modify this section -->
```js
const fs = require('fs')
const path = require('path')
const dox = require('dox')
const markdownSteriods = require('../index')

const opts = {
  commands: {
    /**
     * Custom transform command matching CUSTOM
     *
     * AUTO-GENERATED-CONTENT:START (CUSTOM:lolz=what&wow=dude)
     */
    CUSTOM: function(content, options) {
      console.log('original inner content', content)
      console.log(options) // { lolz: what, wow: dude}
      return 'This will replace all the contents of inside the comment block'
    },
    /**
     * Custom transform command matching LIST_COMMANDS
     *
     * AUTO-GENERATED-CONTENT:START (LIST_COMMANDS)
     */
    LIST_COMMANDS: function(content, options) {
      const commandsFile = path.join(__dirname, '..', 'commands.js')
      const code = fs.readFileSync(commandsFile, 'utf8', (err, contents) => {
        if (err) {
          throw err
        }
        return contents
      })
      const doxOptions = {
        raw: true
      }
      let md = ''
      const comments = dox.parseComments(code, doxOptions);
      comments.forEach(function(data) {
         md += '- ' + data.description.full + '\n'
      });
      return md.replace(/^\s+|\s+$/g, '')
    }
  },
  /* (optional) Specify different output path */
  // outputPath: path.join(__dirname, 'different-path.md')
}

const markdownPath = path.join(__dirname, '..', 'README.md')
markdownSteriods(markdownPath, opts)
```
<!-- AUTO-GENERATED-CONTENT:END -->

## Prior Art

This was inspired by the one and only Kent C Dodds and his all contributors package