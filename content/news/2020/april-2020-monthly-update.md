---
title: "May 2020 Monthly Update"
date: 2020-06-08T19:55:54+12:00
author: Alyssa Parado
summary: Read more updates from Ring, Calva, Reagent, Fireplace, Cider, Figwheel, and Practicalli
draft: true
---

## Ring

**Apr 1-15**

Ring 2.0.0-alpha1 has been released, with the adapter and servlet
utility functions supporting both Ring 1 and Ring 2 keys by default,
though there is also an option for forcing a particular version.

Based on community feedback, the Ring 2.0 request and response headers
have been changed to be closer to the standard laid out in the Ring
1.x specifications.

I've begun setting up a project using the [JMH](https://openjdk.java.net/projects/code-tools/jmh/) benchmarking harness
to compare Ring 1 with Ring 2 performance, particularly around
building the request map. Higher level tests with ApacheBench proved
too imprecise to draw meaningful conclusions, as the variance between
results was higher than any measurable difference between Ring
versions.

I've also been reconsidering some of the ideas around adding request
and response protocols to Ring as a way of supplimenting map key
access. Initially I considered them out of scope, as they're a
performance enhancement, however they could also be used as a
compatibility mechanism for middleware that supports both Ring 1 and 2
formats.

**Apr 16-30**

I've constructed a [JMH](https://openjdk.java.net/projects/code-tools/jmh/)-based benchmarking project for Ring 1.8 and
Ring 2.0, in order to get a better idea of the relative performance of
both versions, specifically around converting to and from Java servlet
objects. I've written up the [initial findings](https://github.com/ring-clojure/ring/issues/393#issuecomment-622516908) and published them
as a comment on the Ring 2.0 GitHub issue.

I've also been fixing various smaller issues with Ring 1.8.0, and
should be ready to release 1.8.1 by the end of the day.

Work on websocket support has been on pause while performance and
compatibility concerns were investigated, but that work is now ready
to be resumed. Work on Ring 2.0 will continue over the next few
months, and further alphas will be released to allow community
feedback and testing of new features.

The first stable version of Ring 1.0 was released a little over 8 years
ago. With a little luck, Ring 2.0 will last even longer.



## Calva

**Apr 1-15**

##### Debugger

The debugger has been released! [Documentation for the debugger](https://calva.io/debugger/) was written and published that covers the existing functionality and lists current and upcoming features.

Since then, I've added step over, step in, and step out functionality as well as annotations in the editor to show the value of the current form as execution moves to each breakpoint. I've updated the docs to reflect these changes as well as made some general improvements to the docs.

I'm currently working on polishing the latest changes and then will release them along with the updates to the docs.

##### Docs

The [Calva docs](https://calva.io/) have a new look and a new home! We switched to using mkdocs for building our docs and are using the material theme.


**Apr 16-30**

##### Debugger

I've added a command to instrument functions for debugging as an alternative and more convenient method than adding `#dbg` reader tags to the code file. I've also added editor decorations to visually indicate which functions are instrumented. The instrumented function definitions as well as the usages gain a top and bottom border and a background. clj-kondo is used [as a JVM library](https://github.com/borkdude/clj-kondo/blob/master/doc/jvm.md#api) to get the var definitions and usages, and [this data](https://github.com/borkdude/clj-kondo/tree/master/analysis) is matched up with cider-nrepl's [`debug-instrumented-defs`](https://docs.cider.mx/cider-nrepl/nrepl-api/ops.html#_debug_instrumented_defs) response data to create the decorations on document change.

The instrument command and decorations features were released and I've started working on displaying structured values, for locals, in the VS Code debugger side pane (currently non-primitives are shown as strings). This should allow a user to "drill down" into a structured value like a map or collection.

## Reagent

**Apr 1-30**

Reagent 1.0.0-alpha1 is out and contains implementation of both
creating React function components from Reagent components
and options. The default is still to create class components
by default, to ensure compatibility, but an option can be used
make function components the default.

After the last update, I had pretty much everything ready, but
I wanted to change the API a bit before first release.
The first implementation of
options had one problem related to caching, as different
options could result in different results but the cache was shared.
This was one of the reasons I wanted to create "Compiler" object
which is created using the options. This object can hold the cache
(or unique key for caches) and is implemented as a reified
protocol. This object currently holds the implementation for a few
Reagent internal functions, most importantly `as-element`, and in
future the function or macro creating this object from options map,
can generate different code for this (and other) methods.

Implementing and getting all the tests passing with this approach
took quite long, but I finally got everything working. After
finishing the code, I added some new documentation pages
and a new example showing making Function components default
and using Hooks with Reagent, and created the first release for testing.

Based on the comments from users, I've already added
two more "shortcuts" to Reagent: `:f>` to create
Function Reagent component (using the default
options), and `:r>` to use React
components without properties conversion done by
`:>` and `adapt-react-class`.

Additionally I've followed latest developments in ClojureScript
compiler, and added new test environment for Reagent, using the
new `:bundle` compilation target. This target generates
JS tooling compatible output. This output can be passed to
Webpack, or for testing, Karma.


## Fireplace

**May 1-15**

[Handle `:Require` exceptions](https://github.com/tpope/vim-fireplace/issues/374)
Spike out a async `:Eval` via an interface to background a blocking eval
  with a key binding.  This is a substantial retooling that also allows among
  other things sending text to stdin. Needs a bit more polish to push to
  master but should be done by the second update.
  
 
 ## Cider

**May 1-15**

##### nREPL:

* Implemented an initial version of the built-in code completion middleware
* Started work on the built-in lookup middleware
* Reviewed and merged dynamic middleware loading functionality (some really great work by Shen Tian)
* Provided default print functions via `nrepl.util.print`.
* Reviewed and merged a couple of other bug fixes by Shen Tian.
* Small documentation updates

##### cider-nrepl:

* Normalized the interface of several ops by making the params they accept consistent

##### Piggieback:

* Cut Piggieback 0.5 with pprint support
* Wrote some design documentation

##### CIDER:

* Added support for the new nREPL completions op
* Fixed a couple of long-standing bugs related to streamed printing in the REPL (https://github.com/clojure-emacs/cider/issues/2628 and https://github.com/clojure-emacs/cider/issues/1971)
* Make it possible to configure the print buffer size that nREPL uses via CIDER
* Reviewed and merged some inspector improvements
* Added a couple of small command (e.g. `cider-repl-toggle-clojure-font-lock`) and made many documentation improvements
* Dropped support for Nashorn

##### inf-clojure:

* Worked with Dan Sutton on simplifying inf-clojure and making it more robust (probably I'll cut a new major release by the end of the funding round)


**May 16-31**

##### nREPL:

* Implemented an initial version of the build-in lookup middleware
* Started work on some best practices documentation for middleware authors
* Started working on adding some extra language/runtime metadata to nREPL that might make it easier for client to
support multiple nREPL implementation (related to https://github.com/clojure-emacs/cider/issues/2848).

##### Orchard:

* Merged a fix for #86 that fixes the resolution order in the info function
* Cut a new release (0.5.9)
* Spent a lot of time debugging an issue with the new release (https://github.com/clojure-emacs/orchard/issues/91)
* Cut a new release (0.5.10)

##### cider-nrepl:

* Cut a new release (0.25)

##### CIDER:

* Many small documentation improvements (including some new page navigation)
* Improved the behavior of commands based on `cider-jump` (https://github.com/clojure-emacs/cider/issues/2850)
* Fixed eldoc on Emacs 28.1
* Discussed ideas on how to make CIDER somewhat Clojure-agnostic (https://github.com/clojure-emacs/cider/issues/2848)
* Cut a new CIDER release (0.25)

##### Misc:

* Fixed a deployment issue with the clojuredocs-edn-export tool (it generates data that orchard uses)


## Figwheel

**May 16-31**

##### Thanks!

This last couple of weeks have been fun and productive. I'm grateful to
have had the time to work on the new `:bundle` target and simplify how
figwheel integrates with the larger NPM eco-system. As a result, the
NPM workflow in Figwheel has been vastly improved.

The following `dev.cljs.edn` config is enough to get you started with
the new bundle target and webpack.

```clojure
^{:auto-bundle :webpack}
{:main example.core}
```

Thanks to everyone who contributed to Clojurists Together and to those
great early adopters of the new `:bundle` target who've helped me
understand what folks are needing.

If you use these new features in Figwheel please reach out with any
comments/questions on the `#figwheel-main` Slack channel as I'd like
to really understand what folks are running into.

##### Overview

I spent the last two weeks continuing to improve the `:bundle` target
support in [figwheel-main](https://figwheel.org).

You can see this work reflected it the `com.bhaumsn/figwheel-main
0.2.7` release and the newly updated [documentation for working with
NPM](https://figwheel.org/docs/npm.html). 

If you head over to that page you will see a vastly improved document
that details the improvements. You will also see that I spent a
weekend updating the look of the [Figwheel
website](https://figwheel.org). The site is still a work in progress
as I only implemented about half of [Lubov
Soltan](https://lubovsoltan.com/)'s design.

To ensure that I wasn't doing anything that would complicate working
with React Native and the new `:bundle` target, I spent some time
looking at
[react-native-figwheel-bridge](https://github.com/bhauman/react-native-figwheel-bridge). I
ended up spending some time updating it so that it works with the new
ClojureScript compiler. In addition I refactored RNFB and added
support for `expo`. During that work I formulated a decent plan to
bring React Native support directly into figwheel-main.

I also spent some time on Devcards and removed its dependency on a
global `js/React` object. It now works much better when using NPM
dependencies for React.

##### Itemized

* Added a notion of `:final-output-to` so that figwheel can
  potentially know where the final output bundle is. This allows us
  to bootstrap an environment without having to create a host
  page.
* Added templating keywords that can be interpolated in the
  `:bundle-cmd`. So if you put `:output-to`, `:final-output-to`,
  `:final-output-path`, of `:final-output-filename` in a `:bundle-cmd`
  the will be replaced with the appropriate file strings.
* Fixed the default REPL host page so that it works with bundling
* Fixed [Extra-Mains](https://figwheel.org/docs/extra_mains.html) functionality so that it works with bundling
* Fixed [Auto-Testing](https://figwheel.org/docs/testing.html#auto-testing) so that it works with bundling
* Added bundle command logging so that the bundle command that is executed gets logged to the console
* Added bundle failure logging so that the output of a failed bundle command is logged to the console
* Added the new Figwheel logo and new website style to [figwheel.org](https://figwheel.org)
* Extensively updated the Figwheel [NPM documentation](https://figwheel.org/docs/npm.html)
* Made it so that `:log-level :debug` will always reveal the stacktrace of a logged Error
* added support for the new `--install-deps` **cljs.main** cli option
* added a validation error for when the `:output-to` is not in the
  `:output-dir` when using the `:bundle` target (bundling wont work otherwise)
* added the `:auto-bundle :webpack` figwheel option which will fill in
  all the defaults to get you up and running with NPM and webpack quickly
* added `:parcel` support to the `:auto-bundle` config option
* added `:bundle-freq` figwheel option which controls how often the
  bundler is called, the value can be either `:once`,`:always` or
  `:smart`. The `:smart` setting re-bundles when the `:output-to` or
  the `npm_deps.js` file for a build changes.
* debugged and reported couple big bugs with `:install-deps` in the
  ClojureScript compiler
* removed [Devcards](https://github.com/bhauman/devcards) dependence on `js/React` global

##### Future work

* launching a Webpack watch process
* React Native support
* Devcards support

During the course of this work my Figwheel to-do list has grown quite
large so stay tuned, there are more changes to come.




## Practicalli

**May 1-15**

Unfortunately illness caught up with me towards the end of this period, but otherwise I have been busy with the weeekly broadcasts and improvements to the Practicalli Clojure book, updating the install guides to Clojure CLI tools and repl driven development sections.

##### Practicalli Study Group
The weekly live broadcasts continue, finalizing the data science series and starting a new series on `clojure.spec`

##### Visualising data science
Concluded the [series of 7 live broadcasts on Visualising data science](https://www.youtube.com/playlist?list=PLpr9V-R8ZxiDUXIR2z8Y8wvhpoPyl0t_D).

The series started with extraction of data from various sources and how to transform data into more relevant structures for the task required.  Initially using ascii-graph to visualize data in the REPL before moving on to create a professional looking dashboard usings Oz and Bulma CSS framework.

The dashboard project was developed around the continually changing coronavirus data available in the UK. The broadcasts has several examples of how to deal with things when the data format changes (which it did several times).

The later broadcasts includes tips and examples on how to refactor a project as it grows.

##### Introduction to clojure.spec.alpha
Started a new video series covering how to use spec in the REPL and with Clojure projects

Initially covering the foundation functions of the `clojure.spec.alpha` library and why its called alpha.  [practicalli/leveraging-spec](https://github.com/practicalli/leveraging-spec) project was created, covering Clojure predicates, spec/conform, spec/valid?, literal values (Clojure sets), the spec registry, fully qualified namespaces, map literal syntax, spec/def and spec/explain.

Started a spec for an online bank account that will be used in further episodes of the series.


##### Practicalli Clojure
Continued to migrate the book to Clojure CLI tools and deps.edn projects. The overall book content design is being refactored.

Update install guide to use Java 11 and added more editor options to the install guides, including NeoVim Conjure and Atom.io Chlorine.

Updated the [practicalli/clojure-deps-edn](https://github.com/practicalli/clojure-deps-edn) repository with also contains a collection of commonly used aliases.  This repository greatly simplifies the installation of Clojure CLI tooling.

Started creating transcripts for videos.  Creating transcripts first helps increase the quality of videos created and reduces the amount of effort required post processing videos.

Added basic introduction to spec which will be expanded as Practicalli study group video series continues


##### Practicalli Spacemacs
Supported the community with issues on Spacemacs gitter and #spacemacs channel of Clojurians Slack.

##### Practicalli Clojure WebApps
Refactor overall book content design for Practicalli Clojure WebApps - plan to extend the scope of the book.

**May 16-31**

Unfortunately I was ill for most of this period, so not as much achieved this time.  I am (and always intended to) extending the work passed the Clojurists Together sponsor period.

##### Summary
Continuing the weekly broadcasts on Clojure spec, a topic that will be added to the Practicalli Clojure book.

Added GitHub issues / PR shields for each book on the website to make tracking and contributing more convenient.

Pages on how to use Magit forge to list and create issues and pull requests as well as fork repositories on GitHib, all from within Spacemacs (Emacs).


##### Practicalli Website
Added [Shields](https://github.com/badges/shields) for each book with links to content ideas and pull requests on the respective repositories.  Aids in the tracking of book progress and providing another way for others to contribute.

Added YouTube playlists to the Practicalli website to make specific video content easier to find.

Updated the Creative commons license notice on the front pages of all books and GitHub README files, ensuring compliance with the [Software Freedom Conservancy](https://sfconservancy.org/).

Add favicon to each book website


##### Practicalli Study Group
Continuing the new series on `clojure.spec.alpha`, this time comparing function definition validation with `:pre` / `:post` conditions and spec `fdef`.

* [072 - Clojure Spec - Part 2 validation with :pre & :post and spec fdef](https://www.youtube.com/watch?v=fOv_z6E30l0&list=PLpr9V-R8ZxiBWGAuncfBRYhZtY5-Bp75s&index=3)


##### Practicalli Spacemacs
Several updates on using the Magit client

Guide on using Magit Forge to list and create issues and pull requests on GitHub, including forking remote repositories on GitHub.
https://practicalli.github.io/spacemacs/source-control/magit/forge.html

Updated and tested the Magit Forge configuration page
https://practicalli.github.io/spacemacs/source-control/forge-configuration.html

Add page on how to staging changes with Magit, including the scope of changes that can be selected, from multiple files, hunks or individual lines.

Video to how to update an existing pull request added to the contributing section, to complement the existing video showing how to contribute a pull request to Spacemacs (which can be used for any other project).

[Practicalli Spacemacs playlist](https://www.youtube.com/playlist?list=PLpr9V-R8ZxiCHMl2_dn1Fovcd34Oz45su) updated with related Spacemacs videos from jr0cket channel.

Supported the community with issues on Spacemacs gitter and #spacemacs channel of Clojurians Slack.


##### Practicalli Clojure WebApps
Add the [high level plan to extend the book contents](http://practicalli.github.io/clojure-webapps/content-plan.html) to the Clojure WebApps book, along with project ideas to implement.

Updated the cover of the book to use the new Practicalli ClojureWebApps book banner

Improvements to various content sections based on feedback from the community.
* Updated webapps overview with additional project links
* Completely revised the introduction to sets and hash-maps
* Improved descriptions for the ring introduction, creating a project, creating a webserver, defining handlers, compojure defroutes and using the let function
* Clarified use of Heroku for deploying applications

