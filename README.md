# CL-Series

This is Richard C. Waters' SERIES package for Common Lisp. In a nutshell: Think "Efficient MAPCAR" SERIES translates functional-style expressions into efficient loops. Series are to loops as control structures are to GOTOs.

# Overview
From the documentation:

Series combine aspects of sequences, streams, and loops.  Like sequences,
series represent totally ordered multi-sets.  In addition, the series
functions have the same flavor as the sequence functions---namely, they
operate on whole series, rather than extracting elements to be processed by
other functions.  For instance, the series expression below computes the
sum of the positive elements in a list.

```
(collect-sum (choose-if #'plusp (scan '(1 -2 3 -4)))) => 4
```

Like streams, series can represent unbounded sets of elements and are
supported by lazy evaluation: Each element of a series is not computed
until it is needed.  For instance, the series expression below returns a
list of the first five even natural numbers and their sum.  The call on
SCAN-RANGE returns a series of all the even natural numbers.  However,
since no elements beyond the first five are ever used, no elements beyond
the first five are ever computed.

```
(let ((x (subseries (scan-range :from 0 :by 2) 0 5))) 
  (values (collect x) (collect-sum x))) => (0 2 4 6 8) and 20
```

Like sequences and unlike streams, the act of accessing the elements of a
series does not alter the series.  For instance, both users of X above
receive the same elements.

A totally ordered multi-set of elements can be represented in a loop by the
successive values of a variable.  This is extremely efficient, because it
avoids the need to store the elements as a group in any kind of data
structure.  In most situations, series expressions achieve this same high
level of efficiency, because they are automatically transformed into loops
before being evaluated or compiled.  For instance, the first expression
above is transformed into a loop like the following.

```
(let ((sum 0)) 
  (dolist (i '(1 -2  3 -4) sum) 
    (if (plusp i) (setq sum (+ sum i))))) => 4
```

A wide variety of algorithms can be expressed clearly and succinctly using
series expressions.  In particular, at least 90 percent of the loops
programmers typically write can be replaced by series expressions that are
much easier to understand and modify, and just as efficient.   From this
perspective, the key feature of series is that they are supported by a rich
set of functions.  These functions more or less correspond to the union of
the operations provided by the sequence functions, the LOOP clauses, and
the vector operations of APL.

Unfortunately, some series expressions cannot be transformed into loops.
This matters, because while transformable series expressions are much more
efficient than equivalent expressions involving sequences or streams,
non-transformable series expressions are much less efficient.  Whenever a
problem comes up that blocks the transformation of a series expression, a
warning message is issued.  Based on the information in the message, it is
usually easy to provide an efficient fix for the problem.

Fortunately, most series expressions can be transformed into loops.  In
particular, pure expressions (ones that do not store series in variables)
can always be transformed.  As a result, the best approach for programmers
to take is to simply write series expressions without worrying about
transformability.  When problems come up, they can be ignored (since they
cannot lead to the computation of incorrect results) or dealt with on an
individual basis.

# Installation

When using ASDF:
```
(asdf:operate 'asdf:load-op :series)
```