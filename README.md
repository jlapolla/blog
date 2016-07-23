## Multiple Representations

Imagine an object oriented software application called TodoPlus that helps the
user organize and manipulate todo lists. *TODO_LIST* and *TODO_ITEM* are two of
the many classes defined in TodoPlus. When the user saves a particular
*TODO_LIST* object (we'll call this object *todo_list*), the following objects
are serialized and stored in a single _\*.tdp_ file on disk:

- *todo_list* itself
- All objects that *todo_list* depends on (this includes all objecst that
  *todo_list* references, both directly and indirectly)

Maria starts TodoPlus, creates a new todo list, and saves her todo list to file
*todo.tdp*. Then Maria closes TodoPlus. After Maria closes the TodoPlus
application, the state of the universe is as follows:

- There exists a single *TODO_LIST* object in the universe (we'll call this
  object *todo_list_a*)
- There is a single physical representation of *todo_list_a*: the representation
  in the *todo.tdp* file on Maria's disk

The next day, Maria starts TodoPlus, and opens her *todo.tdp* file. TodoPlus
reads *todo.tdp* and instantiates a *TODO_LIST* object (and all its
dependencies) in memory. The new object instantiated in memory represents
*todo_list_a*, the same object that Maria was working with yesterday. The state
of the universe is now:

- There exists a single *TODO_LIST* object in the universe: *todo_list_a*
- There are two physical representations of *todo_list_a*:
  - One representation in the *todo.tdp* file on Maria's disk
  - One representation memory in the running TodoPlus process

# Snippets

### entity

**entity** *(noun)*

- A name in the software text that denotes a run-time value.
  - *The entity* person1 *holds a reference to the* PERSON *object we created.*

**Discussion**

An entity is also called a **variable**.

When multiple entities denote a single reference, this does not mean that
multiple references or multiple objects exist. In the following Java code, the
two entities *a* and *b* hold the same reference (in this case, the reference
identifying the *SOME_CLASS* object instantiated on the first line). In the
Java example there is only one reference and one object.

```java
/* Java */

SOME_CLASS a = new SOME_CLASS(); // Object instantiated
SOME_CLASS b = a;
// 'a' and 'b' are aliases for the same reference
```

### reference

**reference** *(noun)*

- A run-time value that uniquely identifies an object.
  - *A constructor function creates an object and returns a reference to the
    newly created object.*

### address space

**address space** *(noun)*

- A set of addresses in memory (e.g. physical memory, virtual memory,
  non-volatile memory).
  - *The file constitutes a 5 MiB address space.*

**Discussion**

Address spaces vary depending on what level of abstraction we view them on. At
the low level, the set of all byte positions on a hard disk constitutes an
address space. On a higher abstraction level, the set of all byte positions in
a particular file on a hard disk constites an address space. In this case the
file's address space is a physical subset of the hard disk's address space. A
similar relation exists between a system's physical memory, and the virtual
memory allocated to a particular execution running on the system.

A reference is distinct from an address (especially in garbage collected
software environments). However, we still consider the set of all possible
references in an execution to constitute an address space. This is because
there is a one to one mapping between references and virtual memory addresses.
A reference identifies a single address in virtual memory, and a single address
in virtual memory is identified by a single reference.

### representation

**representation** *(noun)*

- The bits and bytes that constitute a particular object in a particular
  address space.
  - *Each execution has its own address space with its own representation of
    the object.*

### translate, translation

**translate** *(verb)*

- To create or update an object representation in a destination address space
  from the corresponding object representation in a source address space.
  - *The* LOADER *class translates objects from files into working memory.*

### persist, persistence

**persist** *(verb)*

- To translate an object from an address space in volatile memory to an address
  space in non-volatile memory, or vice-versa.
  - *The object was persisted to disk before the execution terminated.*
- *(of an object)* To exist as the same object in future executions.
  - *The object persists from yesterday's execution.*

### Single representation principle

A particular address space has at most one representation of a particular
object.

**Discussion**

This is the most common convention in software. Typically a second
representation constitutes a second object which is distinct from the first
object.

Redundancy systems such as RAID (redundant array of independent disks) and
database replication produce multiple physical address spaces, each with their
own representation of an object. These redundancy systems combine multiple
physical address spaces to present a single logical address space to the
programmer. The logical address space that the programmer interacts with obeys
single representation. Notice that the applicability of single representation
depends on the abstraction level the programmer is working in (e.g. if the
programmer is making a RAID controller, he must account for multiple
representations).

At certain points in a garbage collection cycle an address space may have
multiple representations (e.g. while copying a representation from one physical
address to another). Typically the programmer works in a higher level of
abstraction, and he may safely assume that the address space obeys single
representation.

### A persisted object maintains its identity

Assume two executions of an application, where the executions are separated by
a span of time.

During the first execution the reference entity *ref1* becomes attached to the
object *O1*. The first execution ends and *O1* is persisted.

The second excecution retrieves *O1* from persistent storage and attaches it to
the reference entity *ref2*.

*O1* in the second execution is not just an object which happens to have fields
identical to *O1* in the first execution. If we could access *ref1* and *ref2*
in a single execution, we would find that the two references denote the same
object. Across both executions, the object *O1* maintains the same object
identity.

A persistence mechanism that preserves object identity is said to exhibit
**identity integrity**.

