# Support Java 11/12 for jpf-core

JPF does not yet fully implement the features of Java 11 - support extends only to Java 8 - including features as simple as string concatenation. This is an especially difficult situation given that Oracle has initiated an “end of public updates process” for Java 8 (although it is interesting to note that Oracle will continue “premier support” for Java 8). It is likely that many Java users will migrate to higher versions, and unless JPF fully supports the new features of a higher version of Java it is unlikely to be used and adopted.

A crucial feature of Java 11 that needs to be urgently implemented is support for bootstrap methods that are generated and resolved at load time. These are used in Java for things as varied as string concatenation and lambda expressions. The project has made significant progress in correcting the errors in the JPF bootstrap method resolution engine and extending its features.

## Pull Requests

### Pull Request javapathfinder/jpf-core#230

This is related to issue javapathfinder/jpf-core#229. The code now properly uses the bootstrap method ```makeConcatWithConstants``` to concatenate strings. This change fixed errors present in the original code not covered by existing tests, like an error where a string concatenation with more than three variables (e.g. ```a + b + c``` for three variables ```a```, ```b```, and ```c```) would fail. There remain limitations to this approach, as the representations of object in the JVM and JPF virtual machine are different, hence using a ```makeConcatWithConstants``` callsite with an identical type signature as the one the JVM constructs is not possible. There is also a remaining issue with JPF calling the ```toString``` method during string concatenation, as the JVM does (note: add a reference here).

### Pull Request javapathfinder/jpf-core#237

This pull request is related to issue javapathfinder/jpf-core#236. This pull request fixed a subtle bug where the JPF operand stack semantics did not follow the JVM semantics whereby a value of type ```Long``` or ```Double``` is pushed to and popped from the stack as two words. Fixing this bug made roughly 30 of 50 failing tests pass, as the bug had far-reaching implications. Fixing this bug showed that the bootstrap method handling for lambda expressions is correct, at least for lambda expressions that use the ```LambdaMetafactory.metafactory```.

### Pull Request javapathfinder/jpf-core#254

This pull request is related to issue javapathfinder/jpf-core#248. This fixed an error with the method dispatch semantics in JPF as described in the issue.

## Outstanding Issues

* javapathfinder/jpf-core#255

* javapathfinder/jpf-core#255

## Local Branches and Ancillary Code

https://github.com/amgad-rady/pyshellcommand

Python scripts to disassemble, regex search, and compute statistics on the Java standard library jars.

https://github.com/amgad-rady/jpf-core/

My fork of ```jpf-core``` which contains the exploratory branches I used to debug and formulate solutions.
