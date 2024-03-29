# Useful Questions
Useful questions should be general in nature such that they apply to anything in their respective domain.

## For solving math problems
 - see George Polya "how to solve it"

## For owning a piece of software
 - What is this thing?
 - Why does it exist?
 - What does it do?
 - What is it made out of?
 - How does it do what it does?
 - How do I start it up?
 - How do I make it do stuff?
 - How can I see what it’s doing?
 - How does it get built?
 - How do I deploy it to QA?
 - How do I release it to production?
 - How can I tell the current version running in production?
 - How can I see how it behaves in production?
 - What is the future of this thing?
 - What don't we love about this thing?

## For finding problems
 - Is this a problem?
 - What is the problem?
   - What is the difference between things as desired and things as perceived?
 - What type of problem is it?
 - Why is this a problem?
 - Who thinks this is a problem?
 - How big is this problem?
 - How can we determine the nature of the problem?
 - Who has the problem?
   - Who must we make happy?
 - How many different definitions of the problem are there? 
 - Does the person who have the problem actually want it solved?
 - Why does the person who has the problem want or not want it solved?
 - How problematic is this?
 - When did this become a problem?
 - For how long has this been a problem?
 - How long have we known about this problem?
 - Who is willing to tolerate this problem?
 - For how long are we willing to tolerate this problem?
 - Why are we willing to tolerate this problem?
 - How did this become a problem?
 - What can be done about this problem?
 - Where did this problem come from?
 - Where do we find this problem?
   - Problem Space                   
     - Problems your customers have that are inherent in their domain
   - Problem-Problem Space			        
     - Problems your customers have that are not inherent in their domain, but are generated from problems in the problem space
   - Problem-Solution Space			       
     - Your solutions to your customers problems that arise from their domain
   - Solution-Problem Space		 	      
     - Problems generated by your solutions to your customers problems
   - Solution-Problem-Problem Space		
     - Problems generated by not solving the problems generated by your solutions to your customers problems

## For when you're debugging

- What question can you ask that will eliminate half the possible answers? Ask that first.
  - Ask broad questions before asking specific ones. For example ask "have you checked the logs?" Before asking "have you checked the dev tools console log?"
- Have you checked the logs?
  - Application Logs
  - System Logs
  - Web Server Logs
  - Database Logs
  - Browser Dev Tools Console/Network Tabs
- Are there any error helpful error codes you can lookup?
- Have you stepped through it in a debugger?
  - Have you walked through the stacktrace in the debugger?
  - If you can't place a breakpoint directly can you manually add a breakpoint on a specific exception and then subsequently walk up the stacktrace to get to the actual problem?
- Have you tried rubber ducking it?
- Have you sought out and compared it deeply with a working/known good example?
- An error message says XYZ missing/not found/doesn't exist
  - have you checked that it does or doesn't actually exist? The computer is usually telling you it doesn't exist or can't be found because it doesn't actually exist and therefore cannot be found.
  - have you tried doing a clean build?
  - have you actually even deployed the code?
- Is there a typo somewhere?
  - did you (very) carefully check capitilization?
    - setup vs. Setup vs. SetUp #datapoint 
    - Is there a naming convention that expects capitilization other than what you have used?
  - has something been renamed and something is still pointing to the old name?
  - have you copy pasted something from one place where it was spelled incorrectly and then subsequently typed it out correctly manually at a later stage and hence things don't match somewhere?
- Is it a permissions issue?
  - Does it work if you run it as a different user?
  - Does something else own a lock because the resources is in use by another process?
    - once a schema script refused to run because I had the very screen open in edit mode that used the table so a row was locked and the schema script failed with cryptic errors. #datapoint 
  - Does your user own the necessary locks?/have they been granted the necessary role?
- Are you being tricked by your assumptions?
  - Has something cached an output and that's why you are still seeing the old behaviour?
  - Are you assuming you have a recent code change because it has been merged to master, but actually you have not updated your branch?
  - Are there actually two different things/classes/objects/scripts/files with the same name and you don't realize they are actually different?
    - I was troubleshooting an issue and assumed the Optional<> I was seeing was Java 8's optional  when in fact it was actually a class called Optional that served exactly the same purpose as Java 8's Optional but it was written years before Java 8 came out. #datapoint
- What do the docs have to say about the expected behavior?
- Could it be a problem with inheritance?
  - Is there a method overriding the behaviour you're currently looking at?
  - Is the behavior you are observing actually located in the superclass or somewhere else in the inheritance hierarchy?
- Do you know when the error was introduced?
  - Has someone (or perhaps another system) made a change recently either to configuration or code that could be affecting the behavior you are seeing?
    - is a new code path being executed that wasn't previously being executed because of this change?
  - Have you checked the history of the file in the version control system to see how it has changed throughout time and if that might be of any relevance?
  - If you are not sure when the error was introduced have you tried using binary search on the commit history to locate exactly which commit introduced the error by checkout out the commit, building it and testing the behavior then iterating that process until you find it?
- Is it a timing/ordering issue?
- Is the file symlinked?
- Is it a path/build path/classpath issue?
  - Have you looked at the file that defines the path/build path/.classpath and check is every folder/resource that you expect to be declared there actually declared?
  - If a particular file is missing from the path/build path/classpath, which folder is that file in? Is that folder listed in the path/build path/classpath?
- Help! There is a NullPointerException which nothing on this line could possibly generate! What's going on?
  - Is a null Integer being passed into a method signature of myMethod(int someInt); and being auto-unboxed at runtime causing an unexpected NullPointerException because int by definition cannot be null? Nasty.
- Could it be an environmental difference?
  - Could there be different versions of a part of the techstack that are behaving differently? 
  - If dev and prod use different webservers or databases i.e Tomcat for dev but WebLogic for prod or MySql for dev and Oracle for prod could there be a difference in the way an API or spec is implemented by the different vendors?
    - Case in point Tomcat's expression language implementation allows method signatures of both public boolean isSomething(); and public Boolean isSomething(); but WebLogic only allows public boolean isSomething();
- Is a resource at capacity?
  - out of memory?
  - out of disk?
  - out of bandwidth?
  - too many simultaneous connections to a server/disk?
- Is a resource unavailable?
  - are there any 3rd party services that are relied on but unable to connect to?
  - Is there a server that is expected to be running or a network location that is expected to be available?
- Could there be a bug in a library/3rd party code?
  - Is the source code available to look at?
- Could there be a possible race condition?
- Have you tried turning it on an off again?
  - bounce the server?
  - bounce the database?
