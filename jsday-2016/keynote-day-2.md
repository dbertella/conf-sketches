# Shipping one of the largest Microsoft JavaScript applications (Visual Studio Code's story) | Keynote by Alexandru Dima @alexdima123

Visual studio code, made in typescript

Goal: build a developer experience directly on the browser
Browsers are really good at text layout

## Virtual scrolling
Add and remove lines from the Dom

Sometime Browser are not smart enough you have to use gpu accelerated animation

## VS code
- 2014
- Used node-webkit
- 2 hours to put it together
- 6 months to launch

Started with node-webkit but then passed to `electron` by github.

Even if you load the code from the same machine bundled and minified code load faster.

Fast editor - build tools - debugger - git integration

**April 2015 launch preview**
Monthly updates, people were surprised about editor changing after their feedbacks.

### Extension isolation
Controlled extensibility.
1000 + Extensions built only in javascript/typescript

## Language / Debug services
Problem: ctrl + space and have autocomplete, c++ like.
Trying to build the standard based on json, all the language support a single protocol.

**November 2015 Beta, extensions and open sourced**
Really lot of issues, and some great PRs.

Try to be transparent as possible, may interation plan, and a roadmap that cover next 6 months.
Questions on stack overflow
Issue on github

**April 2016 1.0, Localization, Accessibility**
