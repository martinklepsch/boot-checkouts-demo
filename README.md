## Boot Checkouts Demo

With `boot` 2.6.0 the `checkouts` task got deprecated in favor of a more integrated [environment parameter](https://github.com/boot-clj/boot/wiki/Boot-Environment).

## An example

Let's say we want to develop `clojure.java-time` simultaneously while working on a bigger project.
First we need to depend on `clojure.java-time` as usual. To now also refresh the jar whenever it changes
we add the same dependency coordinate to the checkouts option.

```
(set-env! :dependencies '[[clojure.java-time "0.3.1-SNAPSHOT"]]
          :checkouts '[[clojure.java-time "0.3.1-SNAPSHOT"]])
```

> This is theoretically also possible using command line options, but there's currently an [outstanding bug](https://github.com/boot-clj/boot/issues/636) with it that needs fixing.

Now with the above `set-env!` call in our `build.boot` we can just run `boot repl`.

In the example below I defined a function `java-time/foo` inside
`java_time.clj`. At first it prints "test". Then I change the function
inside my `clojure.java-time` (git-)checkout and run `lein install` to
get an updated jar into my Maven repository. Boot will pick up the new
jar and add it to the classpath. The only thing users are required to
do is reloading namespaces.

Once you've done that the new code should be loaded. Of course the
reloading of namespaces can be simplified with something like
`clojure.tools.namepsace`.

```
Adding checkout dependency clojure.java-time...
nREPL server started on port 55194 on host 127.0.0.1 - nrepl://127.0.0.1:55194
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.8.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_112-b16
        Exit: Control+D or (exit) or (quit)
    Commands: (user/help)
        Docs: (doc function-name-here)
              (find-doc "part-of-name-here")
Find by Name: (find-name "part-of-name-here")
      Source: (source function-name-here)
     Javadoc: (javadoc java-object-or-class-here)
    Examples from clojuredocs.org: [clojuredocs or cdoc]
              (user/clojuredocs name-here)
              (user/clojuredocs "ns-here" "name-here")
boot.user=> (require 'java-time)
nil
boot.user=> (java-time/foo)
test
nil
boot.user=> (java-time/foo)
test
nil
boot.user=> (require 'java-time :reload-all)
nil
boot.user=> (java-time/foo)
testing some more
nil
boot.user=> Bye for now!
```

### Still not working?

Consider opening an issue with questions or feedback.
