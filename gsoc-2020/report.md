# Support Java 11/12 for jpf-core

Java PathFinder (JPF) is an open source model checker for the Java programming language. JPF automatically checks Java bytecode for subtle bugs due to randomization and concurrency that are typically missed by conventional testing techniques.

JPF does not yet fully implement the features of Java 11 - support extends only to Java 8 - including features as simple as string concatenation. This is an especially difficult situation given that Oracle has initiated an “end of public updates process” for Java 8 (although it is interesting to note that Oracle will continue “premier support” for Java 8). It is likely that many Java users will migrate to higher versions, and unless JPF fully supports the new features of a higher version of Java it is unlikely to be used and adopted.

A crucial feature of Java 11 that needs to be urgently implemented is support for bootstrap methods that are generated and resolved at load time. These are used in Java for things as varied as string concatenation and lambda expressions. The project has made significant progress in correcting the errors in the JPF bootstrap method resolution engine and extending its features.

## Pull Requests

### Pull Request [#230](https://github.com/javapathfinder/jpf-core/pull/230)

This is related to issue [#229.](https://github.com/javapathfinder/jpf-core/pull/237) This change fixed errors present in the original code not covered by existing tests, like an error where a string concatenation with more than three variables (e.g. ```a + b + c``` for three variables ```a```, ```b```, and ```c```) would fail. There were likely other errors not explicitly exposed as string concatenation previously used an error-prone *ad hoc* approach.

String concatenation in JPF has been rewritten to emulate the string concatenation using bootstrap methods that is used by the Java virtual machine. The method ```makeConcatWithConstants``` in the library ```StringConcatFactory``` (link [here](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/invoke/StringConcatFactory.html)) is invoked in the JVM using reflection, with the arguments drawn from the [Model Java Interface](https://github.com/javapathfinder/jpf-core/wiki/Model-Java-Interface) environment. Although it might seem as though this must be done in JPF rather than the host JVM, ```makeConcatWithConstants``` is a pure function, hence calling it in the host JVM is valid since it will not change the state of the system under test. 

### Pull Request [#237](https://github.com/javapathfinder/jpf-core/pull/237)

This pull request is related to issue [#236.](https://github.com/javapathfinder/jpf-core/issues/236) This pull request fixed a subtle bug where the JPF operand stack semantics did not follow the JVM semantics whereby a value of type ```Long``` or ```Double``` is pushed to and popped from the stack as two words. Fixing this bug made roughly 30 of 50 failing tests pass, as the bug had far-reaching implications. Fixing this bug also showed that the bootstrap method handling for lambda expressions is correct, at least for lambda expressions that use the ```LambdaMetafactory.metafactory```.

### Pull Request [#254](https://github.com/javapathfinder/jpf-core/pull/254)

This pull request is related to issue [#248](https://github.com/javapathfinder/jpf-core/issues/248). This fixed an error with the method dispatch semantics in JPF as described in the issue.

## Outstanding Issues

* [#255](https://github.com/javapathfinder/jpf-core/issues/255)

* [#258](https://github.com/javapathfinder/jpf-core/issues/258)

## Local Branches and Ancillary Code

<https://github.com/amgad-rady/pyshellcommand>

Python scripts to disassemble, regex search, and compute statistics on the Java standard library jars.

<https://github.com/amgad-rady/jpf-core/>

My fork of ```jpf-core``` which contains the exploratory branches I used to debug and formulate solutions.
