# What I've learned at jsday 2016 in Verona

I had the chance to participate to [jsday](http://2016.jsday.it/) in Verona early this year and I wanted to make a small summary to all of you that couldn't be there. I really liked [Kitze](https://medium.com/@kitze/lessons-learned-at-react-amsterdam-51f2006c4a59#.leg4e0mjn)'s' idea to write about React Amsterdam conference he attended.

Unfortunately this summary can't be complete since there were 2 distinct tracks and I couldn't follow both of them at the same time but still, I hope this can be interesting for you anyway.

I'll add slides and videos links as soon as they are available.

##TL;DR
The big topic was *Functional reactive programming*, or RxJs if you prefer.
That seems like the big thing in javascript 2016.

<blockquote class="twitter-tweet" data-lang="it"><p lang="en" dir="ltr">Recap from <a href="https://twitter.com/hashtag/jsday?src=hash">#jsday</a> 2016 <a href="https://twitter.com/hashtag/functional?src=hash">#functional</a> <a href="https://twitter.com/hashtag/reactive?src=hash">#reactive</a> <a href="https://twitter.com/hashtag/programming?src=hash">#programming</a> is the new black <a href="https://twitter.com/hashtag/javascript?src=hash">#javascript</a></p>&mdash; Daniele Bertella (@_denb) <a href="https://twitter.com/_denb/status/730801587493437440">12 maggio 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

2 talks, a workshop and mentions in many talks, all about *Functional reactive programming*.

# Day 1
## First Keynote: What we need from the Web, and what it needs from us.
[@shwetank](https://twitter.com/shwetank), Opera developer, talked about *Progressive web apps*, probably the second big thing in javascript 2016. It was fun because I just attended another great conference at google Italia few days before the jsday on the exactly same topic. I was thinking *PWA* were a Google mostly-only thing but I was happy to realize other browsers thinking about it as a way to save `the web` against `mobile native` development.

How *PWA* can beat native?
Basically web offer: `try before install` model vs native that force you to install an app to try it.

Pretty much same capabilities and user engagement without even the needs of installing.

 - Add to home screen
 - Work offline
 - Push notifications
 - Plus the web's power: different urls, SEO, accessibility...

With *PWA* you let the user choose and that for @shwetank can be the key.

## Functional Programming and Async Programming Workshop
The topic was tempting but then after a short and interesting introduction from [@mattpodwysocki](https://twitter.com/mattpodwysocki) people lost interest.

Everybody was following the exercise from the reactivex [site](http://reactivex.io/learnrx/), that I really encourage you to do, but I'd rather have done it at home than spenging my time there during the conference.

Not to criticise him or the organization but just to remind myself it may be better to focus on talks.

Anyway presentation's slide can be found [here](https://github.com/mattpodwysocki/jsday-workshop-2016) along with all the other assets and I encourage you to check them out.

## Forgotten funky functions
[@jakobmattsson](https://twitter.com/jakobmattsson) gave a quick talk about javascript in general and what you can easily accomplish with function that you could have even forgotten. In javascript everything seems really simple, but still you can do great things.

He talked about 3 main topics:
 - functional programming
 - meta programming
 - there is no class

> Whenever you have a problem you can solve it with another function.

@jakobmattsson discuss about `apply`, `call`, `clojures`, `eval`. Topics that (almost) every js developer has doubt about.

He concluded with a useful explanation on ES6 or coffeescript classes syntactic sugar that you may not need if you first fully understand **Object.create**.

## Functional Reactive programming with React.js
**Spoiler alert**, [@lucamezzalira](https://twitter.com/lucamezzalira) didn't talk about react at all! At the end he talked about cycle.js but it was a good *excursus* about RxJs.

You have to start learning a few concepts to understand reactive programming, like *streams*, [cold and hot observables](http://reactivex.io/documentation/observable.html) and [operators](http://reactivex.io/documentation/operators.html).

If you are using a *Flux* implementation for your front end architecture or even worst MVC / MVVM / MV* pattern, you are living in the past! Say hello to **MVI** aka [Model View Intent](http://thenewstack.io/developers-need-know-mvi-model-view-intent/).

I'm personally quite happy with *Redux* for now but I think you need at least to know what's going on around you.

The conclusion was that *Functional Reactive programming* isn't always the best thing to do but for sure it's great when you need to fetch lots of data and react.
You can find slides [here](http://www.slideshare.net/flashplatform/reactive-programming-with-cyclejs).

## Out of the browser and onto the streets
[@Rumyra](https://twitter.com/Rumyra) did the most pleasant talk of the day, with music and visual, it was party time already!

Apparently she's [VJing](https://github.com/Rumyra/VJing) around projecting visual animations on buildings along with music. I'd love to see her live. Unfortunately she haven't had the chance to do it during the party in Verona, but maybe next year 😀.

She talked about the newest web APIs such as `animation`, `audio`, `strem` and `midi`. I encourage you to have check her [github](https://github.com/Rumyra) account. It's not that easy to find samples but you can have an idea about her projects.

She showed us a demo with voice control using a microphone and then another mixing music, video and visual animation.

Last she ended up showing her handmade bag with a mac and her "mini" MIDI controller inside that she's using for VJing.

# Still interested in [day 2](./day-2.md)?
