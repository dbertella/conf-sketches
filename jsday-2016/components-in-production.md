# (Web?) Components in production | Olga Madejska

## Amazon web services
55 services

Service teams:
 - small
 - indipendent (can choose tech stack)
 - strong ownership

No golden hammer, you choose your process, languages, tools.

## How to handle differents teams working on the same product

- Option 1: *Design guide*, implement components in every js framework
- Option 2: *Css framework*, bootstrap style, hard to mantain, set the particular class to particular html
- Option 3: ???, something in code that support all frameworks, browser, performance and accessibility, has tests, development strategy and contribution model

### Web Components
Templates, Html import, Custom elements, Shadow DOM

You can use a polyfill but you have to stick on it

## What they kept
- Custom elements
- Style encapsulation
- Defined API

## Benefits
Easy to grab for new teams, no longer reviews on the components implementation.
Zero effort after a redesign and sparked community engagement.
