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

### address space

**address space** *(noun)*

- A set of addresses in memory (e.g. physical memory, virtual memory,
  non-volatile memory).
  - *The file constitutes a 5 MiB address space.*

### representation

**representation** *(noun)*

- The bits and bytes that constitute a particular object in a particular
  address space.
  - *Each execution has its own representation of the object.*

### translate, translation

**translate** *(verb)*

- To create or update an object representation in a destination address space
  from the corresponding object representation in a source address space.
  - *The class translates objects from files into working memory.*

### persist, persistence

**persist** *(verb)*

- To translate an object between an address space in non-volatile memory and an
  address space in volatile memory (the volatile address space may be either
  the source or destination of the translation).
  - *The object was persisted to disk before the execution terminated.*
- *(of an object)* To exist as the same object in future executions.
  - *This object persists from yesterday's execution.*

### Single representation principle

A particular address space has at most one representation of a particular
object.

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

