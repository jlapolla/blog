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

