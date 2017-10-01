# Day1

## Meet Apache NetBeans [CON6160]

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
- Live coding with Apache netBeans 9 and Jigsaw

## Event Sourcing with JVM Languages [CON5074]

## Everything You Ever Wanted to Know About Java and Didn’t Know Whom to Ask [CON7613]

## Feeding Nine Million Java Developers: How and What? [CON6088]

## Adopt Programs: How to Participate in the Future of Java [CON7623]

## Development Horror Stories [CON3977]
