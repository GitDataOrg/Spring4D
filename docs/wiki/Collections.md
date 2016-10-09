#Collections

## Introduction

Spring4D contains a group of data structures commonly referred to as "collections".  These collections are interface-based.   They include the following:

* ICollection and ICollection<T>
* IList and IList<T>
* TLinkedList<T>
* IDictionary and IDictionary<TKey, TValue>
* IStack and IStack<T>
* IQueue and IQueue<T>
* ISet and ISet<T>

All of the generic collections implement `IEnumerable<T>`, which is discussed in its own page.

The non-generic versions are all collections of [TValue](http://docwiki.embarcadero.com/Libraries/XE6/en/System.Rtti.TValue).  They also implement IEnumerable.  

Also included are read-only versions of all of the above collections.

## Descriptions

A **Collection** is a "bag" of things.  Items can be put into the bag, but there is no order, and the items cannot be indexed or sorted.   They merely are in the collection.  Items can be added or removed, but they cannot be looked up by index and the items cannot be sorted.  They can be enumerated, but there is no guarantee that they will enumerate in any given order.

A **List** is an ordered collection.  Items can be added and inserted at specific locations.  Lists can be sorted an indexed.   

A **Linked List** is the classic data structure which is a collection of nodes, which each node having a reference to the next node in the list -- that is, they are "linked" together, one after the other.  Linked lists are always ordered, and items can be insert at the beginning and end of the list, as well as before and after any given node.  

A **Dictionary** is a set of key/value pairs of any type.  The key can be used to look up its value.  The key and the value can be of any type.  

A **Stack** is a Last-in, First-out data structure. It behaves much like the plate stacks in a cafeteria where plates are pushed down into a compressed stack, and the top plate is pulled off first, and the bottom place (the one that went in first) is pulled out last.  

A **Queue** is a first in first out data structure.  It behaves like tennis balls in a tube.  You put the tennis balls in one end, and they come out the other in the same order that they went in.  

A **Set** is similar to a Delphi `set`.  Items are placed in the set in no particular order -- they are either in or out of the set.  Then, you can determine the union or the intersection of two sets.  You can determine if one set is the subset or superset of another.  


