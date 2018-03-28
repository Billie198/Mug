Matty's Ultimate Gem
====================

[![Build Status](https://secure.travis-ci.org/phluid61/mug.png)](http://travis-ci.org/phluid61/mug)
[![Gem Version](https://badge.fury.io/rb/mug.png)](http://badge.fury.io/rb/mug)
[![Hound-CI](https://img.shields.io/badge/style%20guide-hound--ci-a873d1.svg)](https://houndci.com/)

alias
-----

### Module

#### `alias_singleton_method(new_name, old_name) => self`

Makes _new_name_ a new copy of the instance method _old_name_.  This can be used to retain access to instance methods that are overridden.

#### Examples

```ruby
require 'mug/alias'

module Mod
  def self.foo
    1
  end
  alias_singleton_method :bar, :foo
  def self.foo
    2
  end
end
Mod.foo #=> 2
Mod.bar #=> 1
```

and-or
------

### Object

#### `obj.and default`<br>`obj.and default {|o| block }`

Returns either _obj_ or _default_, depending on the falsiness of _obj_.

If a block is given, _obj_ is yielded to it; if it returns truthy, _default_ is returned, otherwise _obj_ is returned.

#### `obj.or default`<br>`obj.or default {|o| block }`

Returns either _obj_ or _default_, depending on the truthiness of _obj_.

If a block is given, _obj_ is yielded to it; if it returns truthy, _obj_ is returned, otherwise _default_ is returned.

#### `obj.and_then {|o| block }`

Calls _block_ if _obj_ is truthy.

Returns _obj_.

#### `obj.or_then {|o| block }`

Calls _block_ if _obj_ is falsey.

Returns _obj_.

#### Examples

```ruby
require 'mug/and-or'

data_store.get_env_hash.or(default_hash).do_something

get_a_list.and(default_list, &:empty?).do_something

try_thing.and_then {|result| log "got #{result.inspect}" }
try_thing.or_then { log "failed" }
```

apply
-----

### Proc

#### `proc.apply *args`

Curries this Proc and partially applies parameters.
If a sufficient number of arguments are supplied, it passes the
supplied arguments to the original proc and returns the result.
Otherwise, returns another curried proc that takes the rest of
arguments.

### Method

#### `meth.curry`<br>`meth.curry n`

Returns a curried proc. If the optional arity argument is given,
it determines the number of arguments. A curried proc receives
some arguments. If a sufficient number of arguments are supplied,
it passes the supplied arguments to the original proc and returns
the result. Otherwise, returns another curried proc that takes the
rest of arguments.

#### `meth.apply *args`

Curries this Method and partially applies parameters.
If a sufficient number of arguments are supplied, it passes the
supplied arguments to the original proc and returns the result.
Otherwise, returns another curried proc that takes the rest of
arguments.

array/delete_all
---------------

### Array

#### `array.delete_all {|item| block } => array`<br>`array.delete_all => Enumerator`

Deletes every element of `self` for which block evaluates to `true`.

Returns an array of the deleted elements.

If no block is given, an Enumerator is returned instead.

See \#delete\_if, \#reject!

[Feature #13777](https://bugs.ruby-lang.org/issues/13777)


array/extend
------------

### Array

#### `array.extend!(size=0, obj=nil)`<br>`array.extend!(array)`<br>`array.extend!(size) {|index| block }`

Extend this Array.

In the first form, when a _size_ and an optional _obj_ are sent,
the array is extended with _size_ copies of _obj_. Take notice that
all elements will reference the same object _obj_.

The second form appends a copy of the array passed as a parameter
(the array is generated by calling `#to_ary` on the parameter).
See also: `#concat`, `#+`

In the last form, the array is extended by the given size. Each new
element in the array is created by passing the element's index to the
given block and storing the return value.

#### `array.extend(size=0, obj=nil)`<br>`array.extend(array)`<br>`array.extend(size) {|index| block }`

See `#extend!`

array/minus
-----------

### Array

#### `array.minus(ary, remainder: false)`

Subtract elements from this array.

This is similar to `Array#-` except that elements from this array are
removed only once per instance in _ary_.

If _remainder_ is given and true, returns a second array which is
all elements in _ary_ that were not present in this array.

#### `array ^ other`

Get the elements unique to one of two arrays.

Duplicates in either array are included only once.

array/samples
-------------

### Array

#### `array.samples(min: int, max: int, random: rng) => new_ary`

Choose a random subset of elements from the array.

The elements are chosen by using random and unique indices into the
array in order to ensure that an element doesn't repeat itself
unless the array already contained duplicate elements.

The optional _min_ and _max_ arguments restrict the size of the
returned array.  _min_ must be >= 0, and _max_ must be >= _min_.
(Both values are clamped to the size of the array.)

If the array is empty, always returns an empty array.

The optional _random_ argument will be used as the random number
generator.

array/to_proc
-------------

### Array

#### `array.to_proc => proc {|x| ...}`

Returns a Proc that accepts a single argument.

The Proc's parameter is used as an index into this array.

bittest
-------

### Integer

#### `int.and?(other, test: :any)`

Tests common bits in _this_ AND _other_.

test:

* `:any` => true if any bits are set
* `:all` => true if all bits are set

#### `int.and_any?(other)`

True if _this_ AND _other_ is non-zero.

i.e. if any set bits in _other_ are set in _this_.

#### `int.and_all?(other)`

True if _this_ AND _other_ is _other_.

i.e. if all set bits in _other_ are set in _this_.

#### `int.or?(other)`

True if _this_ OR _other_ is non-zero.

#### `int.xor?(other)`

True if _this_ XOR _other_ is non-zero.

bool
----

### Kernel

#### `Bool(obj)`

Returns the truthiness of _obj_, as either _True_ or _False_.

This is functionally equivalent to calling `!!obj`

### Object

#### `obj.to_b`

Converts _obj_ to a boolean using "typical" C-like conversion rules.

The following values all become false:

* `0`, `0.0`, etc. (any numeric zero)
* `Float::NAN`
* `""`
* `[]`, `{}`, etc. (any Enumerable or Enumerator with no elements)
* any Exception

All others values become true.

#### `obj.to_bool`

Returns the truthiness of _obj_, as either _True_ or _False_.

This is functionally equivalent to calling `!!obj`

### Examples

```ruby
require 'mug/bool'

Bool(obj) #=> !!obj
obj.to_bool #=> !!obj
obj.to_b #=> C-like truthiness
```

clamp
-----

Clamps a number to a range.

### Numeric

#### `num.clamp lower, higher => new_num`

Clamps _num_ so that _lower_ <= _new\_num_ <= _higher_.

Returns _lower_ when _num_ < _lower_, _higher_ when _num_ > _higher_, otherwise
_num_ itself.

Raises an exception if _lower_ > _higher_

#### `num.clamp range => new_num`

Effectively calls range#bound

### Range

#### `rng.bound val => new_val`

Bounds val so that _first_ <= _new\_val_ <= _last_.

Returns _first_ when _val_ < _first_, _last_ when _val_ > _last_, otherwise
_val_ itself.

Raises an exception if _val_ >= _end_ and the range is exclusive.


enumerable/any-and-all
----------------------

### Enumerable

#### `enum.any_and_all? {|obj| block }`

Passes each element of the collection to the given block. The method returns `true` if the
block contains elements that never return `false` or `nil`. If the block is not given, Ruby
adds an implicit block of `{ |obj| obj }` which will cause `any_and_all?` to return `true`
when none of the collection members are `false` or `nil`.


enumerable/counts
-----------------

Returns counts of objects in enumerables.

### Enumerable

#### `enum.counts`

Returns a hash of `item=>count` showing how many
of each _item_ are in this Enumerable.

#### `enum.counts_by {|item| block }`

Passes each element in turn to the block, and returns a
hash of `result=>count`.

If no block is given, an enumerator is returned.

### Examples

```ruby
require 'mug/enumerable/counts'

%w(a b b).counts                   #=> {'a'=>1, 'b'=>2}
%w(a b b).counts_by{|o| o.upcase } #=> {'A'=>1, 'B'=>2}
```


enumerable/hash-like
--------------------

Makes Enumerables quack a bit more like a hash.  A lot of these *will not work* on indeterminate or
infinite sequences.

### Enumerable

#### `each_pair {| key, value | block } => hsh`<br>`each_pair => an_enumerator`

Calls `block` once for each key in the enum, passing the key-value pair as parameters.
If no block is given, an enumerator is returned instead.

#### `each_key {| key | block } => hsh`<br>`each_key => an_enumerator`

Calls `block` once for each key in the enum, passing the key as a parameter.
If no block is given, an enumerator is returned instead.

#### `fetch(key [, default] ) => obj`<br>`fetch(key) { |key| block } => obj`

Returns a value from the enum for the given key. If the key can't be
found, there are several options: With no other arguments, it will
raise a `KeyError` exception; if `default` is given, then that will
be returned; if the optional code block is specified, then that will
be run and its result returned.

#### `fetch_values(key, ...) => array`<br>`fetch_values(key, ...) { |key| block } => array`

Returns an array containing the values associated with the given keys
but also raises `KeyError` when one of keys can't be found.
Also see `#values_at` and `#fetch`.

#### `key?(key) => true or false`

Returns `true` if the given key is present in this enum.

#### `keys => array`

Returns a new array populated with the keys from this enum.

#### `length => integer`

Returns the number of key-value pairs in the hash.

#### `member?(key) => true or false`

Returns true if the given key is present in this enum.

#### `slice(*keys) => a_hash`

Returns a hash containing only the given keys and their values.

#### `transform_keys {|key| block } => new_hash`<br>`transform_keys => an_enumerator`

Returns a new hash with the results of running the block once for every key.
If no block is given, an enumerator is returned instead.

#### `transform_values {|value| block } => new_hash`<br>`transform_values => an_enumerator`

Returns a new hash with the results of running the block once for every value.
If no block is given, an enumerator is returned instead.

#### `value?(value) => true or false`

Returns `true` if the given value is present for some key.

#### `values => array`

Returns a new array populated with the values from this enum.

#### `values_at(key, ...) => array`

Return an array containing the values associated with the given keys.


fragile-method-chain
--------------------

Defines a fragile method chain.  If any method call in the chain returns a falsy value, the chain aborts.

```ruby
require 'mug/fragile-method-chain'

# Similar to: a.b && a.b.c
# except that a.b is not called twice
a._?.b.c._!

# Also works with #[] method
nested_hash._?[:a][:b][:c]._!
```

hash/map
--------

### Hash

#### `hsh.map_values {|v| block }`

Returns a new hash which is a copy of _hsh_ but each value is replaced by the result of running it through _block_.

```ruby
require 'mug/hash/map'

{'a'=>1, 'b'=>2}.map_values { |v| v*2 } #=> {'a'=>2, 'b'=>4}
{'a'=>1, 'b'=>2}.map_values { "cat" }   #=> {'a'=>"cat", 'b'=>"cat"}
```

#### `hsh.map_keys {|k| block }`

Returns a new hash which is a copy of _hsh_ but each key is replaced by the result of running it through _block_.

If _block_ returns duplicate keys, they will be overwritten in the resulting hash.

```ruby
require 'mug/hash/map'

{'a'=>1, 'b'=>2}.map_keys { |k| k*2 } #=> {'aa'=>1, 'bb'=>2}
{'a'=>1, 'b'=>2}.map_keys { "cat" }   #=> {'cat'=>2}
```

#### `hsh.map_pairs {|k, v| block }`

Returns a new hash which is a copy of _hsh_ but each key-value pair is replaced by the result of running it through _block_.

If _block_ returns duplicate keys, they will be overwritten in the resulting hash.

```ruby
require 'mug/hash/map'

{'a'=>1, 'b'=>2}.map_pairs { |k,v| [k*2, v+1] } #=> {'aa'=>2, 'bb'=>3}
{'a'=>1, 'b'=>2}.map_pairs { ["cat","dog"] }   #=> {'cat'=>'dog'}
```

hash/merge
----------

### Hash

#### `hsh.merge_left other_hash => new_hash`

Returns a new hash containing the contents of _other_hash_ and the
contents of _hsh_. The value for each duplicate key is the value in
_hsh_ when it exists.

#### `hsh.merge_right other_hash => new_hash`

Returns a new hash containing the contents of _other_hash_ and the
contents of _hsh_. The value for each duplicate key is the value in
_other_hash_ when it exists.

#### `hsh.merge_left! other_hash => hsh`

Adds the contents of _other_hash_ to _hsh_. Entries with duplicate
keys are overwritten with the values from _other_hash_ if the
values in _hsh_ are _nil_.

#### `hsh.merge_right! other_hash => hsh`

Adds the contents of _other_hash_ to _hsh_. Entries with duplicate
keys are overwritten with the values from _other_hash_ unless the
values in _other_hash_ are _nil_.

hash/operations
---------------

### Hash

#### `hsh | other_hsh`


Returns a new Hash, whose value is the same as this
one, with any extras in _other_hash_ added in.

Useful for default options.

```ruby
require 'mug/hash/operations'

def foo options={}
  options | {b: 2, c: 2}
end
foo a: 1, b: 1 # => {:a=>1, :b=>1, :c=>2}
```

#### `hsh + other_hsh`

Adds the contents of _other_hash_ to _hsh_.
Entries with duplicate keys are overwritten with the values from _other_hash_

```ruby
require 'mug/hash/operations'

a = {a: 1, b: 1}
b = {b: 2, c: 2}
a + b # => {:a=>1, :b=>2, :c=>2}
b + a # => {:a=>1, :b=>1, :c=>2}
```

#### `hsh << o`

Appends stuff to the hash.

* If _o_ is a Hash, this is identical to calling #merge!
* If _o_ is an Array with two elements, it is interpreted as [key,value]
* If _o_ can be converted to a hash with #to_h, this is identical to calling #merge!
* Otherwise an ArgumentError is raised.

```ruby
require 'mug/hash/operations'

h = {}
h << {:a=>0}       # h = {:a=>0}
h << {:b=>2,:c=>3} # h = {:a=>0,:b=>2,:c=>3}
h << [:a,1]        # h = {:a=>1,:b=>2,:c=>3}
```


hash/when
---------

Use a Hash like a case statement.

```ruby
case key
when /foo/ then "FOO"
when /bar/ then "BAR"
else "DEFAULT"
end
```

becomes:

```ruby
h = {
  /foo/ => "FOO",
  /bar/ => "BAR",
}
h.default = "DEFAULT"
h.when key
```


iterator/for
------------

### Object

#### `obj.iter_for(meth, *args)`

Creates an Iterator object, which is a subclass of Enumerator that recursively invokes a method on an object.

Initially the receiving object is _obj_.  After each iteration, the receiving object is replaced with the result of the previous iteration.

```ruby
require 'mug/iterator/for'

0.iter_for(:next).take(5) #=> [0,1,2,3,4]
0.iter_for(:+,2).take(5) #=> [0,2,4,6,8]
```

iterator/method
---------------

### Method

#### `meth.to_iter(*args)`

Creates an Iterator object, which is a subclass of Enumerator that recursively invokes _meth_ on an object.

Initially the receiving object is the object on which _meth_ is defined.  After each iteration, the receiving object is replaced with the result of the previous iteration.

```ruby
require 'mug/iterator/method'

0.method(:next).to_iter.take(5) #=> [0,1,2,3,4]
0.method(:+).to_iter(2).take(5) #=> [0,2,4,6,8]
```

loop-with
---------

### Kernel

#### `loop_with_index(offset=0) {|i| block }`

Repeatedly executes the block, yielding the current iteration
count, which starts from _offset_. If no block is given, returns
an Enumerator.

#### `loop_with_object(obj) {|o| block }`

Repeatedly executes the block, yielding an arbitrary object, _obj_.

### Examples

```ruby
require 'mug/loop-with'

loop_with_index do |i|
  p i
  break
end

arr = loop_with_object([]) do |a|
  s = gets.chomp
  throw StopIteration if s.empty?
  a << s
end
```

main
----

### Kernel

#### `__main__`<br>`__main__ { block }`

Compares the calling source filename with `$PROGRAM_NAME` (`$0`).

Returns a falsey value if the calling context is not in the 'main' file.

If called without a block, and the calling context is in the 'main' file,
returns `true`.

If called with a block, and the calling context is in the 'main' file, the
block is executed and the result is returned.

### Examples

```ruby
require 'mug/main'

if __main__
  puts "the main file"
end

__main__ do
  puts "also the main file"
end

# More advanced example:

if __main__ { ARGV[0] == 'foo' }
  do_foo
end

```

matchdata/each
--------------

### MatchData

#### `md.each`<br>`md.each { |str| block }`

Iterates over each capture group in the MatchData object,
including `$&` (the entire matched string), yielding the
captured string.

#### `md.each_capture`<br>`md.each_capture {|key, str| block }`

Iterates over each capture group in the MatchData object,
yielding the capture position and captured string.

The capture positions are either all Strings or all Integers,
depending on whether the original Regexp had named capture
groups or not.

#### `md.each_named_capture`<br>`md.each_named_capture {|name, str| block }`

Iterates over each named capture group in the MatchData object,
yielding the capture name and string.

#### `md.each_positional_capture(include_names: false)`<br>`md.each_positional_capture(include_names: false) {|name, str| block }`

Iterates over each positional capture group in the MatchData object,
yielding the capture position and string.

If `include_names` is given and true, treats named captures
as positional captures.

WARNING: if mixing named and positional captures, no positional
captures will be available using this method!

matchdata/hash
--------------

### MatchData

#### `md.to_h`

Returns a Hash object of capture position => captured string.

The capture positions are either all Strings or all Integers,
depending on whether the original Regexp had named capture
groups or not.

#### `md.named_captures`

Returns a Hash object of capture name => captured string.

#### `md.positional_captures(include_names: false)`

Returns a Hash object of capture position => captured string.

If `include_names` is given and true, treats named captures
as positional captures.

WARNING: if mixing named and positional captures, no positional
captures will be available using this method!

maybe
-----

### Object

#### `obj.maybe`<br>`obj.maybe { block }`

Invokes a method on _obj_ iff _obj_ is truthy, otherwise returns _obj_.

When a block is given, the block is invoked in the scope of _obj_ (i.e. `self` in the block refers to _obj_).

When no block is given, _maybe_ returns an object to conditionally delegates methods to _obj_.

```ruby
require 'mug/maybe'

# Equivalent to: a && a.b && a.b.c
# except that a and b are only invoked once

# (block form)
a.maybe{ b.maybe{ c } }

# (delegator form)
a.maybe.b.maybe.c
```

negativity
----------

### Numeric

#### `num.negative?`

If _num_ is negative (i.e. < 0), returns itself, otherwise returns _nil_.

#### `num.positive?`

If _num_ is positive (i.e. > 0), returns itself, otherwise returns _nil_.

#### `num.nonnegative?`

If _num_ is nonnegative (i.e. >= 0), returns itself, otherwise returns _nil_.

#### `num.nonpositive?`

If _num_ is nonpositive (i.e. <= 0), returns itself, otherwise returns _nil_.

### Examples

```ruby
require 'mug/negativity'

if i.negative?
  puts "#{i} = 0 - #{-i}"
end

n.positive? or raise('not enough items')

x.nonnegative? || -x

arr.map{|i| i.nonpositive? }.compact
```

not
--------

### Kernel

#### `obj.not`<br>`obj.not {|o| block }`<br>`obj.not(*a)`<br>`obj.not(*a) {|o| block }`

Negate a predicate.

### Examples

```ruby
require 'mug/not'

false.not        #=> true
true.not         #=> false

[].not &:empty?  #=> false
[1].not :empty?  #=> true

[1,-2,3].not(:all?) {|e| e > 0 } #=> true
```


rexproc
-------

### Regexp

#### `rex.to_proc`

Returns a proc that accepts one argument, that matches against this regexp object.

```ruby
require 'mug/rexproc'

%w[foo bar baz].select &/\Ab/ #=> ["bar", "baz"]
%w[foo bar baz].reject &/\Ab/ #=> ["foo"]
%w[foo bar baz].find &/\Ab/ #=> "bar"
```

self
----

### Object

#### `obj.self`<br>`obj.self {|o| block }`<br>`obj.itself`<br>`obj.itself {|o| block }`

When a block is given, yields _obj_ to the block and returns the resulting value.

When no block is given, simply returns _obj_.

> Note: this is different from `#tap` because `obj.tap{nil}` returns _obj_, but `obj.self{nil}` returns _nil_.

```ruby
require 'mug/self'

1.self #=> 1
obj.self #=> obj
2.self{|i| i*3 } #=> 6
[1,1,2,2,3].group_by(&:self) #=> {1=>[1,1], 2=>[2,2], 3=>[3]}

1.itself #=> 1
obj.itself #=> obj
2.itself{|i| i*3 } #=> 6
[1,1,2,2,3].group_by(&:itself) #=> {1=>[1,1], 2=>[2,2], 3=>[3]}
```

#### `obj.revapply(*args) {|*list| block }`<br>`obj.cede(*args) {|*list| block }`<br>`obj.revapply(*args)`<br>`obj.cede(*args)`

When a block is given, yields _obj_ and any _args_ to the block and returns the resulting value.

When no block is given, returns an Enumerator.

tau
---

Defines the true circle constant.

```ruby
Math::TAU #= 6.283185307179586..
```

Additionally it expands the BigDecimal/BigMath module:

```ruby
require 'bigdecimal'
require 'bigdecimal/math'
include BigMath

puts TAU(15)
```

See http://tauday.com to find out what it's all about.

time
----

### Time

#### `t.to_now`

Returns the number of seconds since the time represented by
this Time object.

```ruby
start = Time.now
#...
duration = start.to_now
```

#### `t.from_now`

Returns the number of seconds until the time represented by
this Time object.

```ruby
target = Time.new 2117, 1, 1, 0, 0, 0
sleep target.from_now
```

#### `Time.until t`

Returns the number of seconds until `t`.

#### `Time.since t`

Returns the number of seconds since `t`.

to\_h
----

**Removed**

> Note: for Ruy 2.1, `Enumerable#to_h` [is already defined](http://ruby-doc.org/core-2.1.0/Enumerable.html#method-i-to_h).

> Note: for Ruby <2.0, it is advisable to instead use the [*to\_h* gem](https://rubygems.org/gems/to_h).

top
---

### Enumerable

#### `enum.top(n=1)`<br>`enum.top(n=1) {|a,b| block }`

Get the top _n_ items, in order from top to bottom.

Returns an _Array_ even when _n_ is 1.

See: `Enumerable#sort`

#### `enum.top_by(n=1) {|item| block }`

Get the top _n_ items, in order from top to bottom, ordered
by mapping the values through the given block.

Returns an _Array_ even when _n_ is 1. Values that are tied
after mapping are returned in the initial order.

If no block is given, an enumerator is returned instead.

See: `Enumerable#sort_by`


#### `enum.bottom(n=1)`<br>`enum.bottom(n=1) {|a,b| block }`

Get the bottom _n_ items, in order from bottom to top.

Returns an _Array_ even when _n_ is 1.

See: `Enumerable#sort`

#### `enum.bottom_by(n=1) {|item| item }`

Get the bottom _n_ items, in order from bottom to top, ordered
by mapping the values through the given block.

Returns an _Array_ even when _n_ is 1. Values that are tied
after mapping are returned in the initial order.

If no block is given, an enumerator is returned instead.

See: `Enumerable#sort_by`

