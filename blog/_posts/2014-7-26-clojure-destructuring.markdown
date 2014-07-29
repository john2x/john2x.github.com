---
title: Clojure Destructuring Tutorial and Cheat Sheet
layout: post
category: blog
---

When I try to write or read some Clojure code, every now and then I get stuck on some destructuring forms. It's like a broken record. One moment I'm in the zone, then this thing hits me and I have to stop what I'm doing to try and grok what I'm looking at.

So I decided I'd write a little tutorial/cheatsheet for Clojure destructuring, both as an attempt to really grok it (I absorb stuff more quickly if I write it down), and as future reference for myself and others.

Below is the whole thing, copied from the original [gist][gist]. I'm planning on adding more (elaborate) examples and a section for [compojure's own destructuring forms][compojure]. If you want to bookmark the cheat sheet, I recommend the [gist][gist] since it has proper syntax highlighting and will be updated first.

[gist]: https://gist.github.com/john2x/e1dca953548bfdfb9844
[compojure]: https://github.com/weavejester/compojure/wiki/Destructuring-Syntax

* * *

Simply put, destructuring in Clojure is a way extract values from a datastructure and bind them to symbols, without having to explicitly traverse the datstructure. It allows for elegant and concise Clojure code.

Vectors
-------

**Syntax:** `[symbol another-symbol] ["value" "another-value"]`


{% highlight clojure %}
(def my-vector [:a :b :c :d])
(def my-nested-vector [:a :b :c :d [:x :y :z]])

(let [[a b c d] my-vector]
  (println a b c d))
;; => :a :b :c :d

(let [[a _ _ d [x y z]] my-nested-vector]
  (println a d x y z))
;; => :a :d :x :y :z
{% endhighlight %}

You don't have to match the full vector.

{% highlight clojure %}
(let [[a b c] my-vector]
  (println a b c))
;; => :a :b :c
{% endhighlight %}

You can use `& rest` to bind the remaining part of the vector to `rest`.

{% highlight clojure %}
(let [[a b & rest] my-vector]
  (println a b rest))
;; => :a :b (:c :d)
{% endhighlight %}

When a destructuring form "exceeds" a vector (i.e. there not enough items in the vector to bind to), the excess symbols will be bound to `nil`.

{% highlight clojure %}
(let [[a b c d e f g] my-vector]
  (println a b c d e f g))
;; => :a :b :c :d nil nil nil
{% endhighlight %}

You can use `:as some-symbol` as the *last two items* in the destructuring form to bind the whole vector to `some-symbol`

{% highlight clojure %}
(let [[:as all] my-vector]
  (println all))
;; => [:a :b :c :d]

(let [[a :as all] my-vector]
  (println a all))
;; => :a [:a :b :c :d]

(let [[a _ _ _ [x y z :as nested] :as all] my-nested-vector]
  (println a x y z nested all))
;; => :a :x :y :z [:x :y :z] [:a :b :c :d [:x :y :z]]
{% endhighlight %}

You can use both `& rest` and `:as some-symbol`.

{% highlight clojure %}
(let [[a b & rest :as all] my-vector]
  (println a b rest all))
;; => :a :b (:c :d) [:a :b :c :d]
{% endhighlight %}

### Optional arguments for functions

With destructuring and the `& rest` form, you can specify optional arguments to functions.

{% highlight clojure %}
(defn foo [a b & [x y z]]
  (println a b x y z))
(foo :a :b) ;; => :a :b nil nil nil
(foo :a :b :x) ;; => :a :b :x nil nil
(foo :a :b :x :y :z) ;; => :a :b :x :y :z
{% endhighlight %}

Maps
----

**Syntax:** `{symbol :key, another-symbol :another-key} {:key "value" :another-key "another-value"}`

{% highlight clojure %}
(def my-hashmap {:a "A" :b "B" :c "C" :d "D"})
(def my-nested-hashmap {:a "A" :b "B" :c "C" :d "D" :q {:x "X" :y "Y" :z "Z"}})

(let [{a :a d :d} my-hashmap]
  (println a d))
;; => A D

(let [{a :a, b :b, {x :x, y :y} :q} my-nested-hashmap]
  (println a b x y))
;; => A B X Y
{% endhighlight %}

Similar to vectors, if a key is not found in the map, the symbol will be bound to `nil`.

{% highlight clojure %}
(let [{a :a, not-found :not-found, b :b} my-hashmap]
  (println a not-found b))
;; => A nil B
{% endhighlight %}

You can provide an optional default value for these missing keys with the `:or` keyword and a map of default values.

{% highlight clojure %}
(let [{a :a, not-found :not-found, b :b, :or {not-found ":)"}} my-hashmap]
  (println a not-found b))
;; => A :) B
{% endhighlight %}

The `:as some-symbol` form is also available for maps, but unlike vectors it can be specified anywhere (but still preferred to be the last two pairs).

{% highlight clojure %}
(let [{a :a, b :b, :as all} my-hashmap]
  (println a b all))
;; => A B {:a A :b B :c C :d D}
{% endhighlight %}

And combining `:as` and `:or` keywords (again, `:as` preferred to be the last).

{% highlight clojure %}
(let [{a :a, b :b, not-found :not-found, :or {not-found ":)"}, :as all} my-hashmap]
  (println a b not-found all))
;; => A B :) {:a A :b B :c C :d D}
{% endhighlight %}

There is no `& rest` for maps.

### Shortcuts

Having to specify `{symbol :symbol}` for each key is repetitive and verbose (it's almost always going to be the symbol equivalent of the key), so shortcuts are provided so you only have to type the symbol once.

Here are all the previous examples using the `:keys` keyword followed by a vector of symbols:

{% highlight clojure %}
(let [{:keys [a d]} my-hashmap]
  (println a d))
;; => A D

(let [{:keys [a b], {:keys [x y]} :q} my-nested-hashmap]
  (println a b x y))
;; => A B X Y

(let [{:keys [a not-found b]} my-hashmap]
  (println a not-found b))
;; => A nil B

(let [{:keys [a not-found b], :or {not-found ":)"}} my-hashmap]
  (println a not-found b))
;; => A :) B

(let [{:keys [a b], :as all} my-hashmap]
  (println a b all))
;; => A B {:a A :b B :c C :d D}

(let [{:keys [a b not-found], :or {not-found ":)"}, :as all} my-hashmap]
  (println a b not-found all))
;; => A B :) {:a A :b B :c C :d D}
{% endhighlight %}

There are also `:strs` and `:syms` alternatives, for when your map has strings or symbols for keys (instead of keywords), respectively.

{% highlight clojure %}
(let [{:strs [a d]} {"a" "A", "b" "B", "c" "C", "d" "D"}]
  (println a d))
;; => A D

(let [{:syms [a d]} {'a "A", 'b "B", 'c "C", 'd "D"}]
  (println a d))
;; => A D
{% endhighlight %}

### Keyword arguments for function

Map destructuring also works with lists (but not vectors).

{% highlight clojure %}
(let [{:keys [a b]} '("X", "Y", :a "A", :b "B")]
  (println a b))
;; => A B
{% endhighlight %}

This allows your functions to have optional keyword arguments.

{% highlight clojure %}
(defn foo [a b & {:keys [x y]}]
  (println a b x y))
(foo "A" "B")  ;; => A B nil nil
(foo "A" "B" :x "X")  ;; => A B X nil
(foo "A" "B" :x "X" :y "Y")  ;; => A B X Y
{% endhighlight %}
