Naming and binding
******************

*Names* refer to objects.  Names are introduced by name binding
operations. Each occurrence of a name in the program text refers to
the *binding* of that name established in the innermost function block
containing the use.

A *block* is a piece of Python program text that is executed as a
unit. The following are blocks: a module, a function body, and a class
definition. Each command typed interactively is a block.  A script
file (a file given as standard input to the interpreter or specified
on the interpreter command line the first argument) is a code block.
A script command (a command specified on the interpreter command line
with the '**-c**' option) is a code block.  The file read by the
built-in function ``execfile()`` is a code block.  The string argument
passed to the built-in function ``eval()`` and to the ``exec``
statement is a code block. The expression read and evaluated by the
built-in function ``input()`` is a code block.

A code block is executed in an *execution frame*.  A frame contains
some administrative information (used for debugging) and determines
where and how execution continues after the code block's execution has
completed.

A *scope* defines the visibility of a name within a block.  If a local
variable is defined in a block, its scope includes that block.  If the
definition occurs in a function block, the scope extends to any blocks
contained within the defining one, unless a contained block introduces
a different binding for the name.  The scope of names defined in a
class block is limited to the class block; it does not extend to the
code blocks of methods -- this includes generator expressions since
they are implemented using a function scope.  This means that the
following will fail:

   class A:
       a = 42
       b = list(a + i for i in range(10))

When a name is used in a code block, it is resolved using the nearest
enclosing scope.  The set of all such scopes visible to a code block
is called the block's *environment*.

If a name is bound in a block, it is a local variable of that block.
If a name is bound at the module level, it is a global variable.  (The
variables of the module code block are local and global.)  If a
variable is used in a code block but not defined there, it is a *free
variable*.

When a name is not found at all, a ``NameError`` exception is raised.
If the name refers to a local variable that has not been bound, a
``UnboundLocalError`` exception is raised.  ``UnboundLocalError`` is a
subclass of ``NameError``.

The following constructs bind names: formal parameters to functions,
``import`` statements, class and function definitions (these bind the
class or function name in the defining block), and targets that are
identifiers if occurring in an assignment, ``for`` loop header, in the
second position of an ``except`` clause header or after ``as`` in a
``with`` statement.  The ``import`` statement of the form ``from ...
import *`` binds all names defined in the imported module, except
those beginning with an underscore.  This form may only be used at the
module level.

A target occurring in a ``del`` statement is also considered bound for
this purpose (though the actual semantics are to unbind the name).  It
is illegal to unbind a name that is referenced by an enclosing scope;
the compiler will report a ``SyntaxError``.

Each assignment or import statement occurs within a block defined by a
class or function definition or at the module level (the top-level
code block).

If a name binding operation occurs anywhere within a code block, all
uses of the name within the block are treated as references to the
current block.  This can lead to errors when a name is used within a
block before it is bound. This rule is subtle.  Python lacks
declarations and allows name binding operations to occur anywhere
within a code block.  The local variables of a code block can be
determined by scanning the entire text of the block for name binding
operations.

If the global statement occurs within a block, all uses of the name
specified in the statement refer to the binding of that name in the
top-level namespace. Names are resolved in the top-level namespace by
searching the global namespace, i.e. the namespace of the module
containing the code block, and the builtins namespace, the namespace
of the module ``__builtin__``.  The global namespace is searched
first.  If the name is not found there, the builtins namespace is
searched.  The global statement must precede all uses of the name.

The builtins namespace associated with the execution of a code block
is actually found by looking up the name ``__builtins__`` in its
global namespace; this should be a dictionary or a module (in the
latter case the module's dictionary is used).  By default, when in the
``__main__`` module, ``__builtins__`` is the built-in module
``__builtin__`` (note: no 's'); when in any other module,
``__builtins__`` is an alias for the dictionary of the ``__builtin__``
module itself.  ``__builtins__`` can be set to a user-created
dictionary to create a weak form of restricted execution.

**CPython implementation detail:** Users should not touch
``__builtins__``; it is strictly an implementation detail.  Users
wanting to override values in the builtins namespace should ``import``
the ``__builtin__`` (no 's') module and modify its attributes
appropriately.

The namespace for a module is automatically created the first time a
module is imported.  The main module for a script is always called
``__main__``.

The ``global`` statement has the same scope as a name binding
operation in the same block.  If the nearest enclosing scope for a
free variable contains a global statement, the free variable is
treated as a global.

A class definition is an executable statement that may use and define
names. These references follow the normal rules for name resolution.
The namespace of the class definition becomes the attribute dictionary
of the class.  Names defined at the class scope are not visible in
methods.


Interaction with dynamic features
=================================

There are several cases where Python statements are illegal when used
in conjunction with nested scopes that contain free variables.

If a variable is referenced in an enclosing scope, it is illegal to
delete the name.  An error will be reported at compile time.

If the wild card form of import --- ``import *`` --- is used in a
function and the function contains or is a nested block with free
variables, the compiler will raise a ``SyntaxError``.

If ``exec`` is used in a function and the function contains or is a
nested block with free variables, the compiler will raise a
``SyntaxError`` unless the exec explicitly specifies the local
namespace for the ``exec``.  (In other words, ``exec obj`` would be
illegal, but ``exec obj in ns`` would be legal.)

The ``eval()``, ``execfile()``, and ``input()`` functions and the
``exec`` statement do not have access to the full environment for
resolving names.  Names may be resolved in the local and global
namespaces of the caller.  Free variables are not resolved in the
nearest enclosing namespace, but in the global namespace. [1] The
``exec`` statement and the ``eval()`` and ``execfile()`` functions
have optional arguments to override the global and local namespace.
If only one namespace is specified, it is used for both.

Related help topics: global, ASSIGNMENT, DELETION, DYNAMICFEATURES

