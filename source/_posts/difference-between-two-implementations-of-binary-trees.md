title: Difference between two implementations of binary trees
id: 33
categories:
  - 'Coding & Geek Stuff'
date: 2014-11-22 22:26:02
tags:
---

During the course of COMP2012H, we are trying to build up a microScheme language. At two places binary trees are applied, one for structured s-expression, the other for making the customized map.

The s-expression binary tree, was a cons-pair-based lisp-style binary tree, while the other implementation was a normal tree class with root node as member and node being a separate class. It turned out that quite a few differences exist between these two kinds of implementations.

For the _cons-pair-based_ one, the major characteristic is that there's no wrapping tree class. Node themselves form trees, so a parent node is responsible for the memory management of its children and grand children. Thus, in this implementation, we have node destructors that recursively delete its children.

The scenario is totally different in the second type of implementation. Thanks to the wrapping tree class, the "consciousness" of being a tree is yielded to the tree class, leaving the underlying nodes literally unaware of any details of their close relatives, and even anything about each other without a direct connection. In this sense, any member function of a node should not make any assumption about the contents of other nodes (which in fact result in very trivial methods in nodes). All it could do is about the stuff that it has, not even those it owns. In particular, the destructors for such nodes don't delete their children. Anything related to "a collection of inter-connected nodes as a tree" should be part of the tree algorithm, i.e. written as members for the wrapping tree class.

Viewed in another light, we could also see that the first type is highly recursive by nature, in order to achieve collective operation while only affinity is seen by each node. The second type is less recursive, and we could see how often a loop is used in a method, despite a tree is conceptually recursive.