+++
title = "first go at elm"
date = 2016-02-16
aliases = ["/posts/2016-02-16-first-go-at-elm", "/posts/2016-02-16-first-go-at-elm.html"]
[taxonomies]
+++

As mentioned in my [previous post](2016-02-14-silly-side-projects.html), this
blog documents the silly side projects that I do for fun and education.  I have
decided to take a stab at learning [Elm](https://elm-lang.org/), a strongly
typed ML style functional programing language that compiles to Javascript.  If
I like it, I may use it as the front-end of an as of yet half-baked silly side
project that has been stewing in my head for a month or two.

But first: why Elm?

I decided to start learning Elm because I have heard from a number of
individuals in the functional programing community, including a good friend of
mine, [Lane Seppala](https://github.com/lseppala) who writes Haskell
professionally report that Elm not only makes front-end development much more
deterministic than vanilla Javascript, but also much more fun.  Also, I have
heard Elm described as 'Haskell Lite', given that it has similar syntax,
encourages a similar programing style and provides similar guarantees about
type safety, all with a much more shallow learning curve.

I hope that learning Elm will make front-end development much more interesting
and fun, as well as expose me to ML style ideas, patterns, and ways of
thinking. It might even make my third attempt at learning Haskell easier and
more fun as well.

I have started by installing Elm and signing up for pragmatic studio's
[Elm](https://pragmaticstudio.com/elm) course.  I just started the first few
chapters tonight and really like what I see so far.

On an amusing note: I think it is funny that Elixir and Elm both have a pipe
`|>` operator, but that Elixir passes the return value of the preceding
expression into the first parameter of the following function, or in other
words `a |> b(c) == b(a,c)`, whereas Elm (it appears) passes it as the next
parameter of the following partially applied function, or `a |> b c == b c a`.
I wonder how easy it will be to remember how the same operator behaves in the
two languages when working in a project with an Elixir back-end and an Elm
front-end.

Hopefully that will be the hilarious topic of a later post!
