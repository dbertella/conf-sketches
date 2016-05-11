# What we need from the web and what it needs from us | Shwetank Dixit, Opera developer

 Where it begun: people sharing documents and links. Web was a passion project not a commercial one.

 The web is going mobile.

  - Always with you
  - Lot of capabilities
  - People are more on mobile than desktop
  - Is personal, you can't share a desktop

HTML 5 comes with a lot of capabilities, but then mobile went native

Native isn't perfect, different app store have different problems.
Every platform needs a dedicated Dev team.

Web solves native problems, no one owns the web, you have the control over what you want to do.

Web problems are not about technology, are mostly about user experience and user engagement.

## Progressive web app, the web of the future.

Basically 3 things:

 - Add to home screen
 - Work offline
 - The web's Power, urls, seo, accessibility

**Https only!**

You can get free ssl:
- lesenscry.org
- githubpages
- ...

### Add to home screen
 Manifest: `<link rel="manifest" href="/manifest.json">`.
 You can add information for the splash screen, the icon, the way it'll open after the click.

### Working offline
Don't focus on offline but maybe slow connections.
It's very difficult to handle unpredictable state where the connection get slow.

India: 2G 109 Milions, 3G 86 Milions

Africa: 80% of mobile, EDGE only

In the metro, or while using hotel wifi, you can have unpredictable connections. Or abroad you usually choose to be offline.

In the end it's not about being online or online, the keyword is unpredictable, and apps needs to make sure they works in unpredictable state.

Service worker and PWAs can solve this problems.

#### Service worker
 - acts as a proxy
 - Has access to a special cache
 - More than offline

**Things to know:**

 - fully async
 - HTTPS only
 - No DOM access

Cache API, comes with service worker but you can use it even without it.

Utility and libraries that can help with service worker
 - Up-Up: opera
 - sw-toolbox: google
 - sw-precache: google

## Discovery
Web offer: `try before install` model vs native that force you to install and app to try it.

This is the key.

Pretty much same capabilities and user engagement without even the needs of installing.

App install prompt, browsers ask you if you want to install a web app on your device.

**HTTPS only, Service Worker + Web Manifest and regularly visited**

Let the user choose.

> I don't want your fucking app: [link](https://idontwantyourfuckingapp.tumblr.com/)

List of quality PWA
https://pwa.rocks
