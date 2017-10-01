# Day1

## Meet Apache NetBeans [CON6160]
- Absent

## Breaking up the Monolith with Jigsaw: An Architect’s View [CON4400]
- https://github.com/apache/incubator-netbeans
- https://netbeans.apache.org/
- Sven Reimers
- Martin Klahn
- Introduction to Jigsaw
  - The idea is "Reliable configuration", "Strong encapsulation"
  - Modular Descriptor(`exports`, `opens`, `requires`, etc...)
    - requires static?
      - it's compile-time dependencies, dependencies checking will pass at runtime even if the library is not provided
  - Difference between ordinary library managimant system(like Maven, Gradle, etc)?
    - Jigsaw will check consistent of dependencies at compile-time
  - What's happen if there is multiple implementation for service
    - Java will resolve all of implementations
  - Versioning is not supported at this moment
- Why modularity Matters
  - Software will be big, large.
		- Software sould be split into multiple module and constructed from these module.
    - Encapsulation, Complexity
- Live coding with Apache netBeans 9 and Jigsaw
   - NetBeans don't complete any class which is not `required` even if it's in same project.
   - Demo of service(SPI) with Jigsaw


## Event Sourcing with JVM Languages [CON5074]
- Absent

## The Road to Contributing to the Java Technology Community by Developing a Tool [CON6501]
- Why contribute to Java Technology and Community?
  - Because Love
- Heapstats
- How to contribute to the Java Community
  - Contribute to the Java Platform
    - Contribute to Java Platform is difficult because there is a high technical barrier
    - But there is a some rewords
  - Join local JUG
    - In addition, there is a virtual Java User Group
    - https://
  - Developing a software/Tool
    - Duke's Choice Award
    - How and what tool should we develop?
- The lesson and the reason of our tool Development
  - Lessons
    - Find an itch (need) you feel everyday
    - Start from small
    - Find associates
  - History of Heapstats
    - He working in NTT OSS Center as a technical supports of OpenJDK, fixing bug or crash issues
    - Sometime there is no any log, so it's need to enable logging and wait apeearing issue again <- itch
    - Develop a tool which is collecting the logs
    - Prototype is developed in 2011
    - Refining for productino use like documantation in 2012
    - Host it at icedtea community as a OSS in 2013
    - Talk it in conference at local JUG in 2013
    - Talk it in JavaOne in 2014
    - Yoshio Terada nominate it as a candidate of Duke's choise award but it failed in 2014
    - He keeping developing and spread in 2015-2016 like Develop, talk in local JUG, Develop, talk in local JUG, and so on
    - OpenJDK <-> Heapstats
      - Understand OpenJDK's code to develop Heapstats
      - Fix OpenJDK's code
    - It's nominated again and finally it got Duke's choice award in 2016
  - The most important thing is "beleaving your software"


## Feeding Nine Million Java Developers: How and What? [CON6088]

## Adopt Programs: How to Participate in the Future of Java [CON7623]

## Development Horror Stories [CON3977]
