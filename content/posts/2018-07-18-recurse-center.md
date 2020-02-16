+++
title = "recurse center"
date = 2018-07-18
[taxonomies]
+++
This post is coming about 2 weeks late but:

1. I applied & got into the [Recurse Center](https://www.recurse.com/), YAY!
2. 2 weeks ago I started my batch (Summer 2)


My plan going in was to learn more about the following topics: 

1. Strongly Typed Functional Programming
2. Low Level Programming
3. Contributing to Open Source projects

This is a fly by summary of what I have done in my batch so far:

I have been mainly focused on the first goal of learning more about strongly typed functional programming, working through the [Haskell Book](https://haskellbook.com/) and doing all the exercises for all chapters. I am currently on chapter 15. The book has been a fantastic resource and really helped drive home the fundamentals which are needed for building real projects in Haskell.  The small Haskell exercises / projects I am doing at RC are kept [here](https://github.com/nicksanford/RC_haskell). 

I also gave a 5 minute presentation on folds and unfolds (the slides for which can be found [here](https://docs.google.com/presentation/d/1Bq_zQINCsicC3pstZ-nyOopeUWwPJHNQXV0eRW8GSho/edit#slide=id.p)).

I also worked with Omar on a [parser](https://github.com/nicksanford/haskellchemparser) to parse chemical formulas using [Parsec](https://wiki.haskell.org/Parsec), which was my first exposure to using parser combinators and monad transformers and a fantastic exercise.

I decided that the first project big project which I want to work on during my batch is a BitTorrent client in Haskell.  A possible second / alternative project is enabling the nix package manager to install packages from the bittorrent network. Both of those require learning about how bittorrent works / how DHTs work, which I have begun doing by studying a combination of specifications and reference implementations (listed below):

I have also learned about a number of miscellaneous things like how OCaml's module system works (at a high level), how rust's traits work (and how the are very similar to Haskell's typeclasses). I also learned that the state monad is not adding any capabilities that are not already present in folds.

#### Resources
BitTorrent Implementations:

1. Python: <https://github.com/jefflovejapan/drench>
2. Python: <https://github.com/borzunov/bit-torrent>
3. JS: <https://github.com/kzahel/jstorrent>
4. Erlang: This is a pretty massive file which is going to take a while to understand / untangle. <https://github.com/jlouis/etorrent>
5. <https://www.libtorrent.org/>

BitTorrent Specifications:

1. <https://www.bittorrent.org/beps/bep_0000.html>
2. <https://wiki.theory.org/index.php/Main_Page>

DHT Information Specs / Projects:

1. <https://en.wikipedia.org/wiki/Kademlia>
2. <https://stackoverflow.com/questions/1332107/how-does-dht-in-torrents-work>
3. <https://github.com/allanlw/dhtplay>
4. <https://www.bittorrent.org/beps/bep_0005.html>
5. <https://en.wikipedia.org/wiki/Magnet_URI_scheme>
6. <https://github.com/jlouis/dht>
7. <https://github.com/kevinlynx/dhtcrawler2>
8. <https://stackoverflow.com/questions/1181301/how-does-a-dht-in-a-bittorent-client-get-bootstrapped>

Nix + BitTorrent:

1. <https://github.com/NixOS/nixpkgs/blob/master/nixos/tests/bittorrent.nix>
2. <https://sourcediver.org/blog/2017/01/18/distributing-nixos-with-ipfs-part-1/>
3. <https://news.ycombinator.com/item?id=13436026>
4. <https://github.com/shlevy/nix-plugins>
