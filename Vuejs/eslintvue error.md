Delete `Delete Cr`
- touch ".eslintrc.json" (for create new file)
- add to the file 
 `{`
  `"rules": {`
    `"prettier/prettier": ["error", { "endOfLine": "auto" }]`
  `}`
`}`

Component name "name" should always be multi-word


## The best way
- add "lintOnSave: false," pada vue.config.js
- Because vue-cli 3 is using a zero configuration approach, the way to disable it is to just uninstall the module:```javascript
npm remove @vue/cli-plugin-eslint