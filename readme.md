# peerDependencies managment example

Check out the project files to get an idea.

## Test: npm link

The doc says:
*You may also shortcut the two steps in one. For example, to do the above use-case in a shorter way:*

```bash
cd ~/projects/node-bloggy  # go into the dir of your main project
npm link ../node-redis     # link the dir of your dependency
```

This is what we are going to do:

```bash
# Link the "framework" module to the project
testPeerDependencies$ cd fw-project
npm link ../framework

# First test without require('framework-plugin') line:
testPeerDependencies/fw-project$ node index-without-plugin.js
# => framework { plugins: {}, globalValue: 'Good!!' }

# Linking plugin
testPeerDependencies/fw-project$ npm link ../framework-plugin
# and...
# first warn: npm WARN framework-plugin@1.0.0 requires a peer of framework@* but none was installed.

# Complete run with require('framework-plugin')
testPeerDependencies/fw-project$ node index.js
# Error: Cannot find module 'framework'
```

## Fix

You need to run `node` with `--preserve-symlinks` flag and all will work.
More informations here: http://codetunnel.io/you-can-finally-npm-link-packages-that-contain-peer-dependencies/

```bash
testPeerDependencies/fw-project$ node --preserve-symlinks index.js
# Good!
# => framework { plugins: { myPlugin: { hello: 'world', fwGlobalValue: 'Good!!' } },
#       globalValue: 'Good!!' }
```
