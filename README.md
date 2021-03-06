# cljs-react-reload

> Write reloadable Reactjs classes in ClojureScript

This library was created to answer the question how do I maintain
local state for React components that I am reloading live. This
library will be very helpful for folks who use Figwheel and write
React.js classes.

To understand what it does it is probably best to [read it](https://github.com/bhauman/cljs-react-reload/blob/master/src/cljs_react_reload/core.clj)

@dan_abramov's React hotloader prompted me to write this.

## Usage

[![Clojars Project](https://clojars.org/cljs-react-reload/latest-version.svg)](https://clojars.org/lein-figwheel)

Require the `cljs-react-reload.core` macros

```clojure
(ns example.core
  (:require-macros
   [cljs-react-reload.core :refer [defonce-react-class def-react-class]))]
```

Then use the `defonce-react-class` to define a React class that gets
patched on reload.

```clojure
(defonce-react-class Counter
  #js {:getInitialState (fn [] #js {:count 0})
       :render
       (fn []
         (this-as this
           (sablono.core/html
             [:div [:h1 (str "Count")]
                [:button
                  {:onClick
                    #(.setState
                      this
                      #js{ :count (inc (.. this -state -count))})} "inc"]])))})
```

Now you can work on the various React lifecycle hooks and see that
their behavior changes while the local state of the component doesn't.

The `def-react-class` is provided so that you can easiliy **redefine** the
React class.

