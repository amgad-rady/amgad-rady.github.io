## Report on GSoC 2020 Project
# Support Java 11/12 for jpf-core

JPF does not yet fully implement the features of Java 11 - support extends only to Java 8 - including features as simple as string concatenation. This is an especially difficult situation given that Oracle has initiated an “end of public updates process” for Java 8 (although it is interesting to note that Oracle will continue “premier support” for Java 8). It is likely that many Java users will migrate to higher versions, and unless JPF fully supports the new features of a higher version of Java it is unlikely to be used and adopted.

A crucial feature of Java 11 that needs to be urgently implemented is support for bootstrap methods that are generated and resolved at load time. These are used in Java for things as varied as string concatenation and lambda expressions. The project has made significant progress in correcting the errors in the JPF bootstrap method resolution engine and extending its features.

* 
