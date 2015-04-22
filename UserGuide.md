## Introduction ##

Depending on your operating system, Butterfly can be built and started with the following:

```
Windows
butterfly.bat

Mac or UNIX
./butterfly
```

on the command line from within the root Butterfly directory.  Note that you need to have Java installed and available in your path.

Once Butterfly is running point your browser at

```
http://127.0.0.1:8080/
```

in your browser, and you'll see a Butterfly welcome page.

## Modules ##

Butterfly's framework wires together modules to produce functionality.  Modules should do one thing and can be extended by other modules to create new behaviors.  All modules can be found in the directory specified by the `butterfly.modules.path` property, by default defined as the `modules` directory in the Butterfly source, but can contain a comma-separated list of paths on your disk where Butterfly will look for modules.

A module is simple to create.  The module is any subdirectory placed in the `butterfly.modules.path` that contains an `MOD-INF` subdirectory with a  `module.properties` file inside that contains information on the module; more on its below.  The module may also have a Javascript controller and additional Java sources for further functionality, as well as other templates and files related to the module's display and functioning; these are covered in the section on module contents.  One of the goals of modules is to make them easy to share, in part by wrapping all necessary parts in one directory.

All properly structured modules will be loaded by Butterfly on startup; to know which module corresponds to which URL, or mount point, Butterfly examines the `modules.properties` file, as specified by the `butterfly.modules.wirings` property, by default defined as the `modules.properties` file found in the root Butterfly source directory.  Again, more on its later.

The layout concerning just modules, then, resembles the following:

```
butterfly/
 |- modules.properties
 +- modules/
   +- example-module/
     +- (files)
     +- MOD-INF/
       +- module.properties
       +- controller.js (optional)
       +- src/ (optional)
       +- lib/ (optional)
       +- classes/ (optional)
```

### Module Properties ###

A module's properties, `MOD-INF/module.properties`, is laid out in a `key = value` format.  Available keys and sample values for the `interesting` module are:

```
description = Interesting Foo Module
templating = true
templating.macros = macros.vm
requires = skin 
implements = foo
controller = org.example.InterestingFooModuleController
extends = boring
```

and for the `entry` module:

```
description = Starting Portal
templating = true
requires = skin, foo
```

`description` and `templating` are required; the rest are optional according to the module's features.  If templating is enabled, Velocity templates in the module's contents will be interpreted; see the later section on module contents for more.

### Module Wiring ###

Once modules have been written, they can be assigned to local URLs and a map drawn out for how the modules relate.  You must mount a module to the root URL `/` and define that module's values for its `requires` key.  From above:

```
entry = /
entry.skin = pretty
entry.foo = interesting

interesting = /foo/
interesting.skin = pretty

pretty = /skin/
```

Note in the first line that the left hand side is the name of the module, which is the name of the directory that contains the module.  The right hand side is set to the URL you want to assign to that module.  Requirements of the module, such as `entry.skin`, do not point to URLs directly but to the module that will handle that requirement.  Parsing this wiring and modules set up should reveal that the following is part of the structure of this instance's `modules` directory:

```
modules/
  +- entry/
  | +- MOD-INF/
  +- foo/
  | +- interesting/
  |   +- MOD-INF/
  +- skins/
    +- pretty/
      +- MOD-INF/
```

Note that the `foo` and `skins` directories do not have their own `MOD-INF` directories.  Note also that there could be several other modules scattered throughout the directory; they simply aren't wired for use in the example wiring configuration.

### Module Contents ###

In addition to any static content (such as HTML, CSS, or image files), a module can contain templates, template macros, JAR files, and the source for a controller, which can be either Javascript or Java.

#### Templates ####

Butterfly templates can be HTML or Velocity templates.  They provide a path mechanism to avoid embedding potentially volatile URL fragments in your files.  For instance, you can write the following in your index.html file:

<pre>
<link href="[#skin#]/styling.css" type="text/css" rel="stylesheet" /><br>
</pre>

instead of specifying exactly where your module's skin requirement can be found on the web server.  This way, should you ever choose to change skins or change a skin's mount point, you won't have to make the changes in every dependent module's files.  The same abstract path feature is available in Velocity templates.

#### Controller ####

A module's controller defines how the module actually operates.  Much like the overall module wiring tells Butterfly which module to use when a certain URL is accessed, the controller specifies what the module should do when its own URLs are accessed.  The controller can be written in Javascript or in Java (if in Java, the controller property in `module.properties` needs to specify the Java class used as a controller).

A Javascript controller ''must'' provide a `process(path, request, response)` function.  This function will describe what to do when a given ''path'' (relative to the module itself, not to the root of the web server) is passed in.  The ''request'' and ''response'' variables are related to the HTTP request and response objects.

Within the Javascript controller, the `butterfly` global variable is provided.  <span>More on butterfly's methods and properties needed...</span>

A Javascript controller ''may'' provide an `init()` function for initializing the module in script.


## Engine ##

The Butterfly engine makes use of properties found in the servlet container's `butterfly.properties` file.  This is found by default in the `engine/src/main/webapp/WEB-INF` directory in Butterfly's root source directory.

The properties utilized by Butterfly are:
butterfly.home
where is the Butterfly engine found

butterfly.url
where on the web will Butterfly be found - particularly useful when Butterfly is being proxied

butterfly.modules.path
which directory should be used for finding and loading modules

butterfly.modules.wirings
which file should be used to specify module wiring

butterfly.locale.language
butterfly.locale.country
butterfly.locale.variant
for localizing your instance of Butterfly

butterfly.timeZone
which timezone is the server in

butterfly.cache.size
butterfly.cache.policy
butterfly.cache.tuner.sleeptime
cache-related properties

butterfly.resource.loader.class
butterfly.resource.loader.cache
butterfly.resource.loader.modificationCheckInterval
butterfly.resource.loader.description
class loading properties```