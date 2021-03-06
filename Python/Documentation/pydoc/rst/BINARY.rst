Binary arithmetic operations
****************************

The binary arithmetic operations have the conventional priority
levels.  Note that some of these operations also apply to certain non-
numeric types.  Apart from the power operator, there are only two
levels, one for multiplicative operators and one for additive
operators:

   m_expr ::= u_expr | m_expr "*" u_expr | m_expr "//" u_expr | m_expr "/" u_expr
              | m_expr "%" u_expr
   a_expr ::= m_expr | a_expr "+" m_expr | a_expr "-" m_expr

The ``*`` (multiplication) operator yields the product of its
arguments.  The arguments must either both be numbers, or one argument
must be an integer (plain or long) and the other must be a sequence.
In the former case, the numbers are converted to a common type and
then multiplied together.  In the latter case, sequence repetition is
performed; a negative repetition factor yields an empty sequence.

The ``/`` (division) and ``//`` (floor division) operators yield the
quotient of their arguments.  The numeric arguments are first
converted to a common type. Plain or long integer division yields an
integer of the same type; the result is that of mathematical division
with the 'floor' function applied to the result. Division by zero
raises the ``ZeroDivisionError`` exception.

The ``%`` (modulo) operator yields the remainder from the division of
the first argument by the second.  The numeric arguments are first
converted to a common type.  A zero right argument raises the
``ZeroDivisionError`` exception.  The arguments may be floating point
numbers, e.g., ``3.14%0.7`` equals ``0.34`` (since ``3.14`` equals
``4*0.7 + 0.34``.)  The modulo operator always yields a result with
the same sign as its second operand (or zero); the absolute value of
the result is strictly smaller than the absolute value of the second
operand [2].

The integer division and modulo operators are connected by the
following identity: ``x == (x/y)*y + (x%y)``.  Integer division and
modulo are also connected with the built-in function ``divmod()``:
``divmod(x, y) == (x/y, x%y)``.  These identities don't hold for
floating point numbers; there similar identities hold approximately
where ``x/y`` is replaced by ``floor(x/y)`` or ``floor(x/y) - 1`` [3].

In addition to performing the modulo operation on numbers, the ``%``
operator is also overloaded by string and unicode objects to perform
string formatting (also known as interpolation). The syntax for string
formatting is described in the Python Library Reference, section
*String Formatting Operations*.

Deprecated since version 2.3: The floor division operator, the modulo
operator, and the ``divmod()`` function are no longer defined for
complex numbers.  Instead, convert to a floating point number using
the ``abs()`` function if appropriate.

The ``+`` (addition) operator yields the sum of its arguments. The
arguments must either both be numbers or both sequences of the same
type.  In the former case, the numbers are converted to a common type
and then added together.  In the latter case, the sequences are
concatenated.

The ``-`` (subtraction) operator yields the difference of its
arguments.  The numeric arguments are first converted to a common
type.

Related help topics: EXPRESSIONS

