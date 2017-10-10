# Day5

## Java Community Keynote [KEY7694]
- IBM
- Matrix

## HotSpot AOT Internals and AOT + CDS Performance Results [CON7772]

## Optional: The Mother of All Bike Sheds [CON5985]

## Vectors with Values on the JVM [CON4826]
-

## So You Say You Want a Chatbot Revolution [CON3222]
- http://chatbotrevolution.mybluemix.net/
- two kind of Chatbot
  - Chit-chat bots
    - Neither user nor bot have a goal
    - Metrics is turing test
      - Elisa
      - Nowadays it's mostly same rate for human and chatbot in 5 minutes turing test
    - Not useful!
  - Worker bot
    - Both user and bot have goals
    - Metrics is containment
    - useful
- We don't need chatbot oftern but chatbot is useful when we are boring or we can access information quicker or safety than using smartphone
  - Talk to VA vs. Finding smartphone, unlock smartphone, open application, type something
  - In car, VA is definitely safety than smartphone
- Make bot better or learning by developing bot
  - Explain limitation
  - Give examples of what can be asked
  - Don't be afraid to say "I don't know"
    - User will be explain more easy way to understand
    - Hand off human if necessary
    - Provide button for user to response
      - Better UI than typeing a lot of text
  - Don't repeat user's request
  - Sometime conversation will fall in loop
    - To avoid it
      - some response for each node
      - do something when repeating visit of same node
  - Detect frustration and handle it
    - Take a log and analyze it using ML
  - context is important
  - user treat bot like a human
    - Say 'Oh, sorry' when user make a mistake
  - complexity of supported conversation is depend on developer

## Changes to JDK Release Model
- Showing 3 years loadmap
  - Jigsaw, Lambda was scheduled to 7
  - But it was delay even though it was almost ready
    - missing release
  - Once it missed train, we need to wait for long hours
  - More train!!
- Current JDK Release Model
  - release each 2 years
  - support long years -- 10 years
  - First two years
    - there are minor release each 6 month
      - 8u20, 40, 60, ...
      - 9.1, 9.2
      - Add new feature but change specification
  - CPU, critical patch update
- New JDK Release Model
  - Feature release every 6 month
  - YY.MM
    - 18.3, 18.9
  - LTS Release
    - Support for ordinaly release is for 6 month
    - LTS Every 3 years
      - 18.9 is first LTS
      - 21.9, 24.9, 27.9, ...
    - support for 8 years
  - Oracle JDK and OpenJDK
    - Oracle JDK is released for LTS version
      - and it supports for several years
      - 18.9, 21.9, 24.9, ...
    - OpenJDK is released for any version with GPL
      - it supports for 6 month even if it's LTS version
      - 18.3, 18.9, 19.3, ...
- Java 9 is last major release!
- OpenJDK Project
  - JDK Release
    - For feature release
  - JDK Update
    - For update to feature relase
  - Reduce overhead to maintain project for each version
- EA
  - More early access build
    - Project EA
      - For Panama, Valhalla, or something like that
- Version API
  - Same interface?
  - Different meaning for each number
    - $MAJOR.$MINOR.$SECURITY -> $YEAR.$MONTH.$VERSION
- Classfile version
  - minor version in classfile might be used for experimental feature like valhalla?
