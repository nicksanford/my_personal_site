+++
title = "elm is awesome"
date = 2016-02-21
aliases = ["/posts/2016-02-21-elm-is-awesome", "/posts/2016-02-21-elm-is-awesome.html"]
[taxonomies]
+++
I have spent about two days total since my [previous
post](2016-02-16-first-go-at-elm.html) completing pragmatic studio's [intro to
elm course](https://pragmaticstudio.com/courses/elm) and getting about half way
through the second [signals, mailboxes, and ports elm
course](https://pragmaticstudio.com/courses/elm-signals).  Working through these
courses has reminded me why I fell in love with Haskell when I first started
learning it over a year ago.

First off: the courses are absolutely fantastic.  I tore through them in about
a weekend and feel like I have been exposed to enough Elm to know what
questions to ask when I actually start building an Elm app.  By the time I am
completely done with the second course I expect I will have more than enough
knowledge to get started building an Elm app that has a JS dependency, which I
will need for my half baked silly project idea I alluded to in the [previous
post](2016-02-16-first-go-at-elm.html).

Elm has reinvigorated my passion and curiosity in ML style functional
programing.  The Elm courses I worked through recommend declaring function
type signatures before implementing function itself.  I love this development
workflow because it means you have to think about how your functions will
compose on a type level before you write them, which frequently leads you down
a design path where composing the values becomes trivial.

Furthermore, working with a good compiler that keeps me honest is something I
now realize I need dearly.  It is just so damn comforting to know that the
compiler is checking my assumptions even when I am not aware I am making them.
I am now aware that when I write Ruby or JS I have to constantly second guess
myself about whether I am shooting myself in the foot.

I now wish that I knew Haskell well enough to make use of it's amazing
compiler and actually use it to get shit done.  Elm offers a really nice
middle ground because I feel that I could quickly become as productive in it as
I am in React and develop habits that will make me more productive in Haskell
when I start learning it again for the third time.

Also many concepts that seemed so abstract to me when I was first learning
Haskell seem much more within reach. Things such as how to use Maybe or Either
are much more intuitive now that I have seen pattern matching on Elixir tuples 
to handle a very similar concern.  The Elixir pattern of using tuples to
represent computations that may succeed or fail like so:

```elixir
case HttPoison.get(url) do
  {:ok, value} ->
    #something with value
  {:err, err} ->
    #something with value
end
```

is very similar to Haskell or Elm's way of using Either values such as:

```elm
case String.parseInt intString of
  Ok value ->
  --something with value
  Err error ->
  --something with error
```

Elm has also make me aware of how I wish I could write React apps. The React
apps I have worked on so far are littered with state.  This greatly reduces the
team's ability to compose and reuse components.  Furthermore, the components
are also littered with domain logic, which makes refactoring and modification 
difficult.  I look forward to building Elm apps that are as stateless as
possible and isolate domain logic where it can't bleed into more general
concerns.

Also, Signals are such a better way of dealing with user input than the event
handlers we use in React.  Modeling user input as a stream of data (which is 
what I understand Signals to be at this moment) allows you to compose them in
a way I can only dream of doing in the React apps I have been involved with so
far.

In any case I am definitely going to use Elm as my dynamic web front-end of
choice in my next silly side project.  I will finish the course on signals
and then probably take a bit of a break to complete a silly side project
prototype which will be the topic of a future post.
