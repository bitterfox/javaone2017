# Day2

## Preventing Errors Before They Happen [TUT3045]
- Java's type system is too weak
  - It allows code will occur NullPointerException or UnsupportedOperationException for readonly collection
  - In addition, it also allows non-exceptional evel/vulnerable code like including SQL injectionable code.
    - To prevent it classify the data to `@Tained`(User's input, not-yet sanitaized) and `@Untained`(Sanitaized)
    - CheckerFramework checks that there is a possibility of SQL Injection and gurantiee that there is no SQL Injection
- Pluggable Type Checking
  - Run optional type checkers runs after ordinaly compiler
    - Optional type checkers can be multiple and chooseable
- Preventing NullPointerException
  - `@NonNull`, `@Nullable`
  - It actually find NPE in JUnit4
- Benefits of type systems
  - Find bugs in programs
  - Improve documentation
    - Improve maintainability
  - Aid compiler, optimizers and analysis tools
  - Possible negatives
    - Must write tye type
    - False positive
- CheckerFramework
  - https://github.com/typetools/checker-framework
  - Plugs it into OpenJDK or Oracle JDK compiler
  - Integrated Maven, Gradle, NetBeans, IntelliJ, Eclipse
  - It can detect NPE code which can not be detected by other nullness tools like FindBugs, Jlint, PMD
    - Eclipse also cannot detect
    - IntelliJ can detect some of errors but a lot of writing annotation in about a thouthount places
  - Example type systems
    - Null dereferences
    - Equality tests
    - Concurrency locking
    - Fake enumerations/ typedefs
    - String type systems
      - Regular expression
      - printf format
      - methodsignature format
      - compiler messages
    - Security type systems
      - Command injection vulnerabilities
      - Information flow privacy
  - Checkers are usable
    - type checking is familiar to programmers
    - Modular: fast, incremental, partial programmers
    - Annotations are not too verbose
    - Few false positives
  - There is `no bugs` and `no wrong annotations` in programs which satisfies the type property
    - But, it doesn't when
      - it's only for code that is checked: Native method invocation, reflection, and so on
      - the checker-self has a error
  - facilities
    - Full type systems, Generics, Qualifier defaults, pre/post conditions, warning supression
      - Flexible mecanism for Qualifier defaults
- Demo
  - Regex checker
    - `@Regex` requires valid Regex
    - `@Regex(2)` requires valid Regex with 2+ capturing groups
    - It can detect errors at `Pattern#compile` and `Matcher#group`
    - Using `RegexUtil#isRegex` to convert ordinary string to `@Regex String`
       ```
      if (RegexUtil.isRegex(regex, 2)) return;
      @Regex(2) String it = regex; // ok
      ```
      - We can declare post-condition at method
- Type Annotations
  - We cannot express following before Java 8
    - non-null
  - After Java 8, there is type annotations and we can expression
    - Type Annotation at every type appears
  - For Java 6 & 7 compatibilities, it can be wrote in comments
    - `/* @NonNull */ String hogehoge`
- Considering type checker
  - Consider following
    - What runtime exception to prevent?
    - What properties of data should always hold? (devide data in the world)
    - What operation are illegal and legal?
  - Ex)Nullness checker
    - What runtime exception to prevent?
      - NPE
    - What properties of data should always hold? (devide data in the world)
      - NoNull references always NonNull
    - What operation are illegal and legal?
      - Derefecnce @NonNull value is illegal
- Building new type checker
  - Defining a type systems
    - Defining qualifier hierarcy
    - Type introduction rules
      - `@ImplicitFor`, `@DefaultForUnannotatedCode`
    - Type rules
      - checker-specific errors
      - Extends `HogeHogeVisitor` and override `visitHogeHoge`
    - Flow-refinement
      - Detaflow Framework does this
        - Compute properties about expressions
  - Ex) Encripted checker
    - Declare new annotation with `@SubtypeOf`
    - Use `SubtypingChecker` which is generic checker and checks subtype relation
      - Or, build you own type checker
    - Use `@SuppressWarning` at where we want ignore errors
      - Or, fix codes
  - Test it using jtreg or lightweight tester
- Q&A
  - Can annotate library
    - Yes. there is a stub parser

## Twitter’s Quest for a Wholly Graal Runtime [CON5989]
- The goal of Twitter VM Teams is save the money
- Twitter has huge distributed systems
  - 1000+ JVM runs on 1000+ machines
    - Based on OpenJDK 8 with Graal
- Twitter uses a lot of OSS
  - Graal fits this situation
- Why Graal
  - C2 is old and very complex
  - Graal is easier to understand because it's modular designed
  - Better inlining and escape analysis
- bugs
  - #128
  - #263
  - #204
    - Heapster
  - #206
- Contributions
  - #251
- Numbers
  - Finagle-Thrift service
    - Runs 100% on Graal in production
  - Test setup
    - 1 dedicated machine per instance and all instances recieve the exact same request
    - Run with `jvmci-0.30` and `graal-vm-0.22`
      - Default tiered C1/Graal setup
  - Request per sec is mostly same between C2 and Graal
  - But Graal's Scavenge Cycles is smaller number than C2
    - about -2.7%~-2.5%
  - Memory usage increace 40MB
  - Use CPU Time decreace 11%
- Simplely, we can consider cost decreace 11%
- In JDK10, Graal will be experimental JIT compiler
- Note that Twitter uses Scala on Graal, so Java, Python, etc.. on Graal might be different result?
- Issues with Graal
  - Bootstrap issues
    - It's not actual issue on production
    - If it's issue in your env, AOT will solve
  - Use more Java Heap

## JDK 9 Hidden Gems [CON4529]
- Tools & Libraries
  - jshell(REPL tool) with Demo
    - well documentation feature(Showing signature, javadoc)
  - List#of
    - Immutable collection factories
    - The list is optimized
  - Stream#takeWhile, dropWhile
  - Optimization of String concatation
    - Using invokedynamic
- JVM/Hotspot
  - Make G1GC as a the default GC
  - CMS is deprecated and will be removed in future release(JEP 291)
  - Remove GC combination which is deprecated in JDK8(JEP 214)
  - Store interned String in CDS Achives(JEP 250)
    - String is storead shared in Class Data Sharing
    - Strings are shared across Java instances
    - Application CDS
    - Improve startup time(about 1.3 times faster, 70%) and footprint
      - 10 instance -> 10+% saving in total memory footprint
  - Ahead of Time compilication
    - Does with JIT compiled java code what AppCDS does with java class Data
      - Share the result of JIT compiler?
      - Still experimentain in JDK9, only on linux-x64
      - Improve statup performance, reduce time to peek performance, saves on footprint by sharing across instance
        - see image
  - Flight Recorder
    - Opensourced
  - Unified logging
  - and more and more features, see image
- Performance
  - Vectorization Enhancements
    - Vectorization extended to 512 bits, AVX-512
      - JIT's code generation
      - -XX:-UseSuperWord, -XX:UseAVX=2, 3, -XX:ObjectAlignmentInBytes=64
      - upto 150~180%
      - Stream is also faster
    - superunroll
    - More Vectorization
      - `Math#sqrt` is now supported and 6 times faster
  - Compact String
    - Memory usage is 20% less than JDK8
    - 5% better throughput
    - Optimized Math Libraries
      - exp, pow, sin, cos, etc.. are Optimized
      - 200 ~ 600% faster
    - Crypto Accelaration
      - SHA-250, AES CTR, etc... are optimized
        - 200%~1000% faster
    - Compression Accelaration
      - JRE's zip uses software accelaration and hardware accelaration in addition to system zlib
      - 150 ~ 300% faster
    - New Checksum Application
      - CRC32 and CRC32C
    - New Array Process Application
      - Lexicographic Array compare API
        - Array.compare
      - Benefit in big data like Cassandora HBase which uses array as a key
    - New FMA API


## Java Keynote [KEY7692]

## RDBMS to Kafka: Stories from the Message Bus Stop [CON7374]

## Shenandoah 2.0: Now That We’ve Gotten the GC Pause Times Under Control, What’s Next? [CON6108]

## Using Type Annotations to Improve Your Code [BOF3048]

## JSF 2.3 in Action [BOF2723]
