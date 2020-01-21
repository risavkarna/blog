## Mathematics' Anarchitecture & Processes

In the  [previous post](https://risav.dev/mathematics-architecture-and-processes-ck5ic92qa03jzqks1l2gv7lob)  in this series, we made an outline of mathematical areas of programmer interests that we will explore together in 2020. 

A major portion of the post was about how first-order and/or propositional logic is isomorphic with categories and types. Since  [propositions are types](https://youtu.be/IOiZatlZtGU), you can have such constructs as constraint types or process types and perhaps a protocol for interacting with other types. A simple way forward would be to simply use the commonly held concept of mathematical operators and the fact that these operators will always be 'operating' on n - tuples with elements of various types. It is fun to work on the operators themselves rather than with numbers.

Operators that preserve structure or map to a specific one are more interesting than the stochastic ones for now. At this point in the realm of what we call transformers and mutators. You will see that these are generalized nicknames for natural transformations and functors. We will be using these to build our basic data structures like multi-dimensional matrices, preferably in  [rasdaman](http://www.rasdaman.org/)  on  [PostgreSQL](https://www.postgresql.org/). These matrices encode graphs and hypergraphs which we will be using in our collaborative system ( [CoSys](https://github.com/risavkarna/cosys/blob/master/README.md) ).

We have introduced the co-planetary platform and nep.work in a separate post. We end the series on a more abstract note by talking about our internal and external language and about the sinks, links & sources.

The whole point is to make a map. And then to make a smaller map to navigate the previous map, ad infinitum. This structure is 12 tiered, 6 tiered, 4 tiered, 3 tiered, 2 tiered and 1 tiered in a 1D plane. In 2D, it is 12x6, 12x7, 12x7x2 and 6x6. In 3D, it is 3x3x3 and 3n+-1 x 3n+-1 x 3n+-1 dimensional. All kinds of data can be mapped on to this single monolithic immutable structure. The projection of such data, however, requires specialized DBMS and algorithms. For example, we will be using  [Druid](https://druid.apache.org/) for time series and stream data. For document storage, we will be using MongoDB or Mongo-like DB APIs. For key: value stores, we will lean towards  [memcached](https://memcached.org/)  or  [redis](https://redis.io/). This is not to be confused with the  [Apache Ignite](https://ignite.apache.org/)  read layer of our persistence model for mathematics, and anything else for that matter. 

Yes, we are still strictly only talking about mathematics and applied mathematics. We will shift focus towards 'pure' mathematics and forget the engineering concerns later when we define types and categories. We still have the responsibility to build the matrices and hypergraphs promised before. For our purpose of finding mathematical foundations and mathematical structures and founding a mathematical software based on those findings, we will need steps 7 through 12. 

In the next post in this series, we will cover the origin of all of these ideas and the  [one ring](https://lotr.fandom.com/wiki/One_Ring)  to rule them all. 

>Three Rings for the Elven-kings under the sky,

> Seven for the Dwarf-lords in their halls of stone,

> Nine for Mortal Men doomed to die,

> One for the Dark Lord on his dark throne

> In the Land of Mordor where the Shadows lie.

> One Ring to rule them all, One Ring to find them,

> One Ring to bring them all and in the darkness bind them

> In the Land of Mordor where the Shadows lie

> This, my dev, is our API