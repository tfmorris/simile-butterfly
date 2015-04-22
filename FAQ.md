This is a list of Frequently Asked Questions about the [[Butterfly](Butterfly.md)] Project.

Feel free to add your own questions here, we'll answer them here for others's benefit.

## Why the name Butterfly? ##

The original author of Butterfly is also the author of [Apache Cocoon](http://cocoon.apache.org/), a successful web application framework that started as a pipelined-based XML publishing system and evolved into a fully blown web application framework.

While Cocoon was more successful than its original author even imagined and pioneered several technologies (i.e. the use of an IoC-based component model, the use of event-oriented XML pipelines, the use of server-side continuations in Javascript), the modernization of client side technologies (CSS, client side XSLT, DOM) since its conception (that happened in 1998!) has displaced some its reasons to exist.

At the same time, when tasked in writing web applications, there was always something missing in other webapp frameworks that Cocoon had but it was too big and bloated to move along.

Butterfly is basically a modern metamorphosis of Cocoon where most of the bloat, inefficiencies, overdesign, over-configurability, over-componentization, duplication of effort and focus on big groups of developers is lost and some of the ideas that cocoon used are retained.

You can think of Butterfly as the webapp framework that finally came out of its cocoon ;-)

## What do you mean by 'web app modularity'? ##

One of Butterfly's main features is the notion (previously pioneered by Cocoon Blocks) of polymorphic and extensible webapp modules. Basically a web application is split into independent and reusable pieces and then 'wired' together to form the web application. Modules can indicate their dependency on another module (or on a module interface), they can implement a particular interface (and therefore be eligible to fulfill another module's dependency) or they can extend another module.

The idea behind native webapp modularity is to reduce cut/paste between your previous web applications and a new one to a bare minimum, hopefully zero.

## What did Butterfly keep of Cocoon? and what threw away? ##

First of all, Butterfly doesn't deal with XML in a special way at all, the configurations are regular Java property files and there are no built-in XML pipelining or transformation mechanisms available beyond the ones provided by the Java environment itself.

Butterfly doesn't depend on any other framework for component managing, neither Avalon, nor Spring, nor it re-invents the wheel (although it doesn't restrict you from doing so in your own modules, would you wish to do so), basically it treats modules as a servlet, allowing you to do whatever you want in there.

Butterfly, like Cocoon, takes control of the entire URL space and therefore does not constraint your URL space in any way.

Also, Butterfly retains the notion of 'sitemaps' that Cocoon had: a request manager that reacts on request parameters (mostly URL regexp matching) and dispatches to various actors... unlike Cocoon, though, Butterfly's sitemaps are simply written in java or javascript code, and both are compiled down to java bytecode. We believe this strikes a balanced between flexibility in request processing and dispatching, performance and ease of module reuse.