# Amazon VO

### January 2022

1. Design a basic text editor like notepad. (I did it using linked list, keeping track of cursor "node" and inserting and deleting as your would normally on a linked list)
2. LRU cache - [https://leetcode.com/problems/lru-cache/](https://leetcode.com/problems/lru-cache/)
3. a) Reverse a linked list - [https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)\
   b) Reverse a linked list given L (a starting point) and R (a end point) - [https://leetcode.com/problems/reverse-linked-list-ii/](https://leetcode.com/problems/reverse-linked-list-ii/)
4. Design dealing N cards to M players and pretty printing each hand in a sorted order\
   I wrote down some basic classes for it like a "CARD", "PLAYER", "GAME" and enum for "SUIT"\
   Pretty printing you basically convert 1,11,12,13 to A,J,Q,K (i m sure there are better ways to do it)\
   For sorting, it would be a custom comparator for the class "CARD". I explained it to him a bit but didnt actually code it as we ran out of time.

### **September 2020**

**Round 1 - Bar raiser**\
Create a doggie daycare app:

1. to enter dogs need to be vaccinated
2. dogs less than 4 months and more than 10 yo go to a kessel, capacity 20
3. small dogs enter into small gods playgrounds, capacity 30
4. big dogs enter into small gods playgrounds, capacity 20
5. dogs come and go all the time\
   '''\
   class DoggieDayCare(): \
   self.kessel = 0 \
   self.small = 0 \
   self.big = 0 \
   def welcome\_doggie(dogId):   \
   pass \
   def bye\_bye\_doggie(dogId):   \
   pass\
   '''

**Round 2 - Hiring Manager**

Create a set that holds elements with an expiration date. Expired elements need to be deleted. Use dict vs queue

**Round 3 - Design Patterns**\
Subscribe / Publish\
'''\
class EventManager(): \
self.event\_dict = {}\
&#x20; def subscribe(eventName, callback):   \
pass \
def publish(eventName, param)

e = EventManager()\
e.subscribe(''category'', lambda x: print(x))\
e.publish('category', 'event1') # should print 'event 1'\
e.publish('category', 'event2') # should print 'event 2'\
'''

**Round 4 - Algos and Data Structures**\
Return all words starting with a prefix\
Ex:\
Words: \[Apple Application Banana Boore Book]\
Prefix: 'App', 'Boo'\
Return: \[Apple, Application], \[Boore, Book]

\*Interview taken in September 2020 for the Seattle EC2 team

### **Find compilation order**

Input:

target0.go: \[target1.go, target2.go, target3.go, target4.go]\
target1.go \[ target5.go, target6.go, target4.go]\
target2.go: \[target9.go, target7.go, target4.go]\
target3.go \[target6.go, target4.go]

Find the order in which a compiler will build these files

Rule:

1. If two files that are not dependent on each other then the order does not matter.
2. Before building a file, all its dependent modules should be built

Output:\
A order of compilation given the list of dependent modules

****

### **Linux Find Command**

****

implemnet linux find command as an api ,the api willl support finding files that has given size requirements and a file with a certain format like

1. find all file >5mb
2. find all xml\
   Assume file class\
   {\
   get name()\
   directorylistfile()\
   getFile()\
   create a library flexible that is flexible\
   Design clases,interfaces.



### SDE II **package dependencies** <a href="#https-leetcode.com-discuss-interview-question-1031933-amazon-onsite-sde2-package-dependencies" id="https-leetcode.com-discuss-interview-question-1031933-amazon-onsite-sde2-package-dependencies"></a>

a) You have a package repository in which there are dependencies between packages for building like package A has to be built before package B. If you are given dependencies between the packages and package name x, we have find the build order for x.\
Ex: A → {B,C}\
B → {E}\
C → {D,E,F}\
D → {}\
F → {}\
G → {C}

For package A, build order is E B F D C A (may not unique)

Given a function Set getDependencies (Package packageName) which returns a set of dependencies for a given package name, write a method List getBuildOrder(Package packageName) which returns the build order

b) How would you handle cyclic dependencies (Algo only)
