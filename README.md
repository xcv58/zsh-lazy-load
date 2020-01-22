# zsh Lazy Load

Provides `lazy_load()` to lazy load functionality.

## Why you need this

The zsh setup is slow because startup time is too long.

Some functions take more than 1000 milliseconds. The startup time can reach more than one second if you have several such functions to execute.

For example, the nvm takes ~ 0.3 seconds to load and you are not necessary to use node every time open a new shell. This module helps to reduce the startup time by lazy load other functionalities.
Don’t preload nvm but register some trigger commands to load nvm
When user type any trigger command, i.e. type in a command likes `yarn add react`, it automatically loads `nvm`
Keep executing the command (`yarn add react`) after loading nvm

## Install
`cd` to your zprezto repo, and execute:

```
git submodule add https://github.com/xcv58/zsh-lazy-load.git modules/lazy-load
```

Then add `'lazy-load'` after `zstyle ':prezto:load' pmodule` in the `zpreztorc` file, example: https://github.com/xcv58/prezto-1/pull/2/files#diff-c43fcdb2d3c410d66f0fb3305cc43606

## Usage

***Please include lazy-load in zpreztorc before any module you want to use lazy load function***

```shell
lazy_load load_func command1 command2 command3 ...
```

A 4 lines example for lazy load nvm: https://github.com/xcv58/prezto-1/pull/2/files


## How

The lazy_load() will:

1. create a lazy function to wrap your load_func.
2. alias all the commands you provide by following arguments to the lazy function and command itself

So that when you type in any of your trigger commands, the lazy function is called. It will:

1. unset the lazy function itself
2. un-alias all commands
3. eval your load function
4. unset your load function
5. eval the arguments ($@) you typed in, so the command will run as normal
