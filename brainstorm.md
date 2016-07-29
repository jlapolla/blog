### Surrogate may solve "partial translation" problem

Consider when an execution does not have permission (for security reasons) to
view or modify the entire contents of an object. In this case, a surrogate
which provides a restricted view of the object may be the answer.

### When to use expanded subobjects

Use expanded subobject when you may have partial information. E.g. if we have a
zip code reference, then we also know the city and state. But then we cannot
express uncertainty. For instance, what if we know the state or city, but not
the specific zip code?

### Surrogate should not edit target object

Allowing the surrogate to alter the target object puts the final action
decision into the hands of the editor, not the caller. This may be a problem.

One option: instead of having the surrogate edit the object, have the object
"accept" the surrogate. Of course, this does not work when creating a new
object.

Caller initializes and passes in surrogate, callee modifies surrogate, caller
applies surrogate to existing object, or gets a new object from the surrogate.

### Translation and inheritance

In any address space, every object has a generating class. You can only
translate an object into it's generating class. You cannot translate it into a
parent class.

### Beware of immutable structures

Beware of immutable structures! Altering them (creating a new one with most of
the components the same) is a nightmare. If you need to do this, you may have a
flaw in your design (e.g. the structure should sematically be mutable). Beware
of immutable objects with references to other immutable objects.

### Surrogate and MVVM

Surrogate is the "view model" in MVVM. It can combine attributes from multiple
objects into one view.

### Surrogate update and copy-on-write

Surrogate update versus new object is related to "copy on write" idea.

### Immutability and interning

Concept of immutability resulting in implicit identity is closely related to
"string interning", "hash consing", and the "flyweight" design pattern.

### Defining **intrinsic properties**

Intrinsic properties can be computed without dereferencing mutable references.
Problem with observer model when attempting to observe extrinsic properties.
Questionable if extrinsic properties really belong on an object in the first
place.

### All references assumed shared

In general, never assume that a reference is private to your code.

### Object id is immutable

Object id is an immutable attribute, by definition.

### runtime object

A **runtime object** is a representation in an active memory space.

### Correcting definition of "reference"

Definition of **reference** is incorrect. Should be: ... identifies a runtime
object in a given address space.

### Identity integrity possible with weak references

Existence of weak references in a runtime environment allows identity integrity
to be maintained orthogonally. It is unknown whether we can accomplish this
without weak references. See artima.com/insidejvm/ed2/gc16.html.

### Object id creation

The execution that creates an object is responsible for creating an id for the
object upon translation.

### Object not honored

Authority may choose not to "honor" (i.e. reject) a new object, or an update to
an existing object.

### Agree and conflict

Representations in two address spaces may "agree" or "conflict". Conflicting
representations need to be "reconciled". Note that this is conceptually
different from synchronizing a surrogate. In the case of a surrogate, the
execution is aware of the distinction between the surrogate and the runtime
object.

### Synchronization Hell

Use as few surogates as possible. Avoid "Synchronization Hell".

Surrogates may be out of sync with the authoritative representation in their
address space. They need a synchronization mechanism.

### Pondering object-relational impedance mismatch

Object relational impedance mismatch due to the difference in the
implementation of lists in both systems?

### Domain control in object-oriented model

Impossible to do domain control in object oriented model, unless we restrict an
execution from creating certain types of objects in an authority. (I.e.
authority rejects translation when it would create a new object of a domain
control class.)

### Object reachability in the relational model

In a relational model, objects are considered reachable by default (reversal of
reference direction).

Relational the protects us from deleting a referenced object (through foreign
keys).

### One-to-many relational model

A 1:n relational model makes child sharing impossible.

### Lists that *own* their elements

Problem of editing lists of items is especially apparent when the user expects
that the list is the only place the item exists. It may well be the case when
the item is created, but the item may nevertheless end up being shared.

### Surrogate

Concept of surrogate representation (multiple conceptual representations in one
address space).

For example: we have the functional object (maintains invariant, provides
services via preconditions and postconditions). Then we have other views of the
object (e.g. the text content of a GUI control). The other views are
**surrogates**. They are similar to representations in that they describe the
object (and therefore can become unfaithful), but the are not intended to be
the object in the address space (i.e. execution is aware that they are
different runtime objects).

### Unfaithful surrogate relevancy

Objects which must be edited with commands, it matters if your temporary
representation (now called a **surrogate**) is unfaithful.

### Inherent properties

Do not store properties on an object that are not inherent to that object. For
example, the rank of a team in a league can only be computed with reference to
a particular league which contains a particular collection of teams. Also, a
team may compete in multiple leagues, and thus have a rank in each league.

### Collections of mutable objects

Consider a set of mutable objects. The uniqueness constraint cannot be
enforced. Developer is tempted to defensive copy.

### Shallow edit UI rules

For shallow edit UI: we give the user 4 options: edit person, replace with new
person, add to list, remove from list.

In general, these are the options we have available when editing any list.

### Partial translation

Partial translation: when the entire object contents should not be translated
due to confidentiality reasons.

### List editing (new vs. update)

Editing a list of people. How do we determine if the user meant to change the
name of a person, or create a new person with that name? This is related to the
"shallow edit" principle.

### Identity inference vs. parent invariant

Consider identity inference based on a parent reference. Is this identity
inference, or is it an invariant in the parent?

### Use bank account example for object-representation distinction

Bank account is a better example of object-representation separation. Use the
classic example of transferring money between accounts.

### Semantics of immutability

Immutability means that attribute equality implies reference equality.
Basically, the object ID is derived from the object's attributes, and we
consider any two objects with the same attributes to be the same object (same
object ID).

Really, this is a way to protect the clients of immutable classes. Once the
client queries the class, it can depend on the value it receives NEVER
changing. Nothing can change the attributes of the object that lie behind that
object ID, ever.

The conclusion is that immutable classes have fundamentally different semantics
compared to mutable classes.

