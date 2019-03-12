# Multi Point Merge Expression Syntax

The supported syntax for the processing logic expressions is described in the following table:

.. list-table::
   :widths: 20 20 60

   * - Expression Symbol
     - Function
     - Description
   * - $
     - Variable selector
     - The format is `​${ variable }`, in which `variable` must be a measure point or an attribute of a model.
   * - if
     - Conditional statement
     - Only if the condition is true, the statement runs the code. The format of the statement is: `if (condition) { the code to be run }`.
   * - if...else
     - Conditional statement
     - If the condition is true, the statement runs a code. If the condition is false, the statement runs another code. The format of the statement is: `if (condition) { the code to be run when condition is true} else { the code to be run when condition is false}`.
   * - if...else if...else
     - Conditional statement
     - Run one of multiple codes by different conditions. The format of the statement is: `if (condition 1) { the code to be run when condition 1 is true} else if (condition 2) { the code to be run when condition 2 is true} else { the code to be run when conditions 1 and 2 are false}`.
   * - "::"
     - Namespace identifier
     - Refer to the C++ syntax. The format of the statement is: `class name (model)` :: `class member (measure point / attribute)`.
   * - "+ - * /"
     - Arithmetic operators
     -
   * - "! && |"
     - Logical operators
     -

.. note:: Multi Point Merge expressions support Scala syntax, but do not support loop statements like for and while.

## Expression Example
```scala
if(${turbine::state}=="Noraml")${turbine::wind_speed}+2*${turbine::power}
```

The function of the above expression is: If the turbine status is `Normal`, then the output is:

```
wind_speed + 2*power
```



<!--end-->