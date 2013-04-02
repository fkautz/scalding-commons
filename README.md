## scalding-commons [![Build Status](https://secure.travis-ci.org/twitter/scalding-commons.png)](http://travis-ci.org/twitter/scalding-commons)

Common extensions to the [Scalding](https://www.github.com/twitter/scalding) MapReduce DSL.

### Dfs-Datastores Integration

Scalding-Commons includes Scalding Sources for use with the [dfs-datastores](https://www.github.com/nathanmarz/dfs-datastores) project.

This library provides a `VersionedKeyValSource` that allows Scalding to write out key-value pairs of any type into a binary sequencefile. Serialization is handled with the `bijection-core` library's [Injection](https://github.com/twitter/bijection/blob/master/bijection-core/src/main/scala/com/twitter/bijection/Injection.scala) trait.

`VersionedKeyValSource` allows multiple writes to the same path,as write creates a new version. Optionally, given a [Monoid](https://github.com/twitter/algebird/blob/develop/src/main/scala/com/twitter/algebird/BaseAbstractAlgebra.scala#L23) on the value type, `VersionedKeyValSource` allows for versioned incremental updates of a key-value database.

```scala
import com.twitter.scalding.source.VersionedKeyValSource
import VersionedKeyValSource._

// ## Sink Example

// The bijection library provides implicit Injections
// from String -> Array[Byte] and Int -> Array[Byte].
val versionedSource = VersionedKeyValSource[String,Int]("path")

// creates a new version on each write
someScaldingFlow.write(versionedSource)

// because Scalding provides an implicit Monoid[Int],
// the writeIncremental method will add new integers into
// each value on every write:
someScaldingFlow.writeIncremental(versionedSource)

// ## Source Examples
//
// This Source produces the most recent set of kv pairs from the VersionedStore
// located at "path":
VersionedKeyValSource[String,Int]("path")

// This source produces version 12345:
VersionedKeyValSource[String,Int]("path", Some(12345))
```

## Maven

Current version is `0.1.5`. `groupid="com.twitter"`,  `artifact=scalding-commons_2.9.2"`.

## Authors

* Oscar Boykin <http://twitter.com/posco>
* Mike Jahr <http://twitter.com/mjahr>
* Sam Ritchie <http://twitter.com/sritchie>
* Ashu Singhal <http://twitter.com/daashu>

## License

Copyright 2012 Twitter, Inc.

Licensed under the Apache License, Version 2.0: http://www.apache.org/licenses/LICENSE-2.0
