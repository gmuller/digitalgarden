---
{"dg-publish":true,"dg-permalink":"seven-languages-in-seven-weeks","permalink":"/seven-languages-in-seven-weeks/","title":"Seven Languages in Seven Weeks","created":"2022-04-19T14:14:29.000-04:00","updated":"2022-10-21T20:26:59.000-04:00"}
---


# Seven Languages in Seven Weeks
## Introduction

Think about the way you think. Think about that thought, and this one. Did you think using words? Did you see the words? Sound them out mentally? If someone asked you describe yourself, you would probably think of a series of adjectives (at least if you're an English speaker).

We think via language, spoken or written. It's the source of our intelligence and in some ways the root of our consciousness. Helen Keller is quoted with communicating that:

> When I learned the meaning of 'I' and 'me' and found that I was something, I began to think. Then consciousness first existed for me

The languages you learn are the languages you express yourself with. They mold the way that you think about things and create who you are within your own mind. [I've written about this before](http://grantmuller.com/hacking-the-way-we-think/) and it's not an entirely new concept.

Recently I've gone to great lengths to change the way I think. Finding new ways to solve problems, especially software problems, often involves learning new languages, syntaxes or paradigms. You can force Java or C to do just about anything, just as you can force the English language to \*describe\* just about anything, but it might be that by using Java instead of Haskell, you're using the wrong tool for the job.

I wanted to expose myself to a breadth of different software paradigms in as little time as possible. Rather than reading dozens of tutorials, or poring through hundreds of pages of reference manuals to get maximum exposure, I bought a book I called [Seven Languages in Seven Weeks][8]. Packed into this dense little tome is an overview of seven syntaxes from different families and programming paradigms.
## The Languages
### Ruby

The book begins with Ruby. It's a fairly common syntax and I considered skipping this chapter. Indeed, with the relative ubiquity of the language I wondered why it had been included at all. In the spirit of playing along with the author I read through the sections and did the exercises as described. It turned out to be a good idea; some of the concepts around using [method_missing as a DSL generator](http://www.artima.com/rubycs/articles/ruby_as_dsl3.html) I had never put into practice.

From a comfort standpoint starting with a language you're familiar with is also a bit like reading the introduction to a Latin grammar text book in English. I know the language and therefore the author can present his approach to me with words I can understand before I try to make my way through the rest of his presentation.

Speaking of presentation, Tate clearly has a grasp of basic pedagogy. From the beginning he uses a mnemonic device to help the reader put a face to the chapter and the methodology. For Tate, every language is like a character in a movie. They have their own personalities; something that makes them unique within the dozens of lexical environments out there. For Ruby it's Mary Poppins. You know, syntactical sugar. Get it.
### IO

After Ruby Tate introduces a language I had never heard of, Io. Just try searching for information about this little language on the web. You won't exactly find the throbbing community that surrounds java or ruby to back you up. No, if you choose to use Io to solve something new, you'll likely find yourself in uncharted territory. Not necessarily a bad thing if your approach to the text is to learn new ways to think.

A prototype language, Io is described by Tate as Ferris Bueller. In use I got the distinct impression that Io was heavily influenced by [Smalltalk](http://en.wikipedia.org/wiki/Smalltalk); everything you send is a message, and their are nothing but senders and receivers of messages. Method or function? Not really, there are 'slots' with message handlers. Can they be construed as the same thing, abstractly yes, but that is to avoid thinking in a way that make the language unique. Sending messages between objects is a powerful concept, and will help you better understand Objective-C and Smalltalk.

Using Io feels a bit like working in JavaScript, the only other prototype language I have any experience with. The concurrency framework is dead simple and provides the reader with a taste of things to come from languages like Scala and Closure. In fact, the actor framework in Io is so simple and impressive it feels like a great environment in which to teach concepts of asynchronous behavior and concurrent development.
- ### Prolog
  
  After Io we get to Prolog, the most frustrating language paradigm for me to grasp in the book. Tate says Prolog is like Rainman. That must make me Tom Cruise.
  
  The logic programming paradigm was at once the most fascinating and frustrating for me to study. At first I was enthralled. A language that I can plug values into and simply query against to get the answer like a super-powered database? Sign me up. I immediately found myself fighting the syntax. It took me some time to grasp the recursive nature of the language as well; no looping structures.
  
  Solving the sudoku problem at the end is the best example of the power of Prolog and languages like it. Reducing a game to a couple of line of syntax, injecting the rules and simply asking questions is a beautiful way to solve many of the problems modern engineers are presented with... if Rainman doesn't drive you nuts along the way.
### Scala

With Scala we take a detour back to familiar territory. Scala is the first variant on the java language I'd had the opportunity to use, so when I began the chapter I had some exposure to it. Most of the concepts in this language sunk right in.

Tate says we can think of Scala as Edward Scissorhands. He is the construction of spare parts and a lot of paradigms that already exist. I prefer to think of Scala as MacGuyver; It can do pretty much anything in a pinch. Scala was a comfortable environment to take a break in for a while. It sports functional programming paradigms like [higher-order functions](http://en.wikipedia.org/wiki/Higher-order_function), while retaining many imperative concepts held over from C-based languages. Its also completely interoperable with Java, so all of those libraries we've grown attached to like [joda](http://joda-time.sourceforge.net/) and [jsyn](http://www.softsynth.com/jsyn/) can be reused in the same lexical environment.

For concurrency Scala provides an actor system, much like Io. Tate clearly planned the book to address concurrency in a methodical way, first by introducing simple examples with Io, then advancing to Scala before diving headlong into the deep waters of Erlang and Clojure.
- ### Erlang
  
  Things get uncomfortable again as Tate introduces erlang. From the get go Erlang baffled me, and when it was revealed that it was modeled after Prolog, I understood why. The only language compared to an antagonist in a movie, Tate describes Erlang as Agent Smith from the Matrix. Tate says that this is due to the self-replicating capabilities of Agent Smith in relation to the fail-safes built into Erlang, allowing the user to build highly fault tolerant concurrent systems that "just won't die". I think it's because Erlang is evil.
  
  Erlang is clearly very powerful, so as with Prolog I struggled through the examples and problem sets. I still don't feel like I fully grasp how to do anything useful with it. Of the languages in the book, I feel like this is the one I need to spend the most time with to really understand.
- ### Clojure
  
  Next we get a [lisp](http://en.wikipedia.org/wiki/Lisp\_(programming\_language)). Clojure, a language fully compatible with the jvm is a lisp not at all unlike [Scheme](http://schemers.org/), minus a few parentheses. For Tate this language is like Yoda, no doubt due to the reverse notation of the arguments and the inside-outness of the code construction, at least compared to C.
  
  Surprisingly, I took right to it. Of the new lexical environments this felt the most comfortable, but then, I've played with emacs a bit. The concurrency framework is not at all unlike scala with some notable additions. The concept of [STM](http://en.wikipedia.org/wiki/Software\_transactional\_memory) was awkward at first, but after fiddling with it for a while I was comfortable producing usable code.
  
  The interoperability with Java is another major benefit to using Clojure. For [Dijkstra's Sleeping Barber problem](http://en.wikipedia.org/wiki/Sleeping\_barber\_problem), rather than struggle through writing a queue from scratch, I just borrowed the existing Java [LinkedBlockingQueue](http://docs.oracle.com/javase/6/docs/api/java/util/concurrent/LinkedBlockingQueue.html), cranking up one actor to poll it, and another to deliver to it:
  
  In just under 1000 parentheses the barbershop problem was solved. The wrapper around it is unnecessary, but then the whole solution is a little bit wordy for Clojure.
### Haskell

Impressed as I was with Clojure, it was time to study the final language in the book, Haskell. I originally selected the book based on the inclusion of Haskell. For some time now I've wanted to take a crack at this pure functional, almost entirely mathematical language.

Compared with the ever logical Spock, I'm still dazzled by Haskell. Having read the chapter and gone through the exercises, I feel like I've only scratched the surface of what it can do. It's unofficial tag is that it makes hard things easy, and easy things hard. That ain't no lie. Try reading a file with it. Do something simple, like open a socket. I feels like pulling teeth. Now go write a Fibonacci solver. Chances are you'll have cooked up something that can't be done as succinctly or quickly as Haskell can do it. Of all the languages in the book, this is the one I intend to dig the deepest with.
## Wrap-up

When learning anything, I generally feel like a breadth-first overview is the best method of getting started. When learning new ways to think, this breadth-first search seems even more important. Get all your options on the table, see what's been discovered before deciding how to tackle the problem. Selecting a strategy to go deeper with is a decision that can always be deferred until you know what your strategies are (Of course, you can only defer for so long before you just need to make a damned decision based on what you already know).

The real value of Seven Languages is that it provides this kind of breadth-first overview. You may know Java or C already. That's great. What else is out there? What can a language like Io make easy? Clojure will help you understand lisp. Haskell will help you understand \_any\_ functional and improve your understanding of modern math. Scala will let you build damn near anything.

Tate's progression makes a lot of sense as well. If I was creating a curriculum to prepare a developer for the real world, I would start a youngster out with something like Ruby. This is an obvious ramp into Java and C. Then I might introduce something like Io to explain prototype languages and concurrency in a simple way. This is an step towards a better understanding of both Javascript and Objective-C. Then I might start them on Scala. It's maximum exposure to as many concepts as possible. From scala, Learning a functional language is made easier if the programmer has been using Scala's higher-order stuff like fold and map, and is used to immutable variables. Tate's text provides a decent way to do all of this, introducing a young developer not only to the syntaxes but to paradigms that are broad enough provide insight to damn near any language out there.

If you're interested in seeing my solutions to the exercises and problem sets, you can find them [here](https://github.com/gmuller/7L7W). I learned a lot along the way, and I think I achieved the goal I had set out to achieve: Learning new ways to think.