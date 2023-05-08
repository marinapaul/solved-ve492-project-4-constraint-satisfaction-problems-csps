Download Link: https://assignmentchef.com/product/solved-ve492-project-4-constraint-satisfaction-problems-csps
<br>
A constraint satisfaction problem (CSP) is a problem whose solution is an assignment of values to variables that respects the constraints on the assignment and the variables’ domains.

CSPs are very powerful because a single fixed set of algorithms can be used to solve any problem specified as a CSP, and many problems can be intuitively specified as CSPs.

The goal of this project is to help you better understand CSPs and how they are solved. In this project you will be implementing a CSP solver and improving it with heuristic and inference algorithms.

<h2>Starter Files</h2>

For this project, your are provided with an autograder as well as many test cases that specify CSP problems. These test cases are specified within the csps directory. The tree diagram of the starter system is as follows.

.

+– BinaryCSP.py

+– Interface.py

+– StudentAutograder.py

+– Testing.py

+– csps

|          +– csp7.assignment

|          +– csp7.csp |  +– …

+– test_cases

+– backtracking3solvable1_1.solution

+– backtracking3solvable1_1.test +– …

Functions are provided in Testing to parse these files into the objects your algorithms will work with. The files follow a simple format you can understand by inspection, so if your code does not work, looking at the problems themselves and manually checking your logic is the best debugging strategy.

<h2>Data Structure</h2>

All of the necessary interface structures for assignments and CSP problems is provided for you in Interface.py, and every function you will be implementing are marked with TODO in BinaryCSP.py. While you do not need to implement these structures, it is important to understand how they work.

Almost every function you will be implementing will take in

<ul>

 <li>a ConstraintSatisfactionProblem</li>

 <li>an Assignment</li>

</ul>

The ConstraintSatisfactionProblem object serves only as a representation of the problem, and it is not intended to be changed. It holds three things:

<ul>

 <li>a dictionary from variables to their domains (varDomains)</li>

 <li>a list of binary constraints (binaryConstraints)</li>

 <li>a list of unary constraints (unaryConstraints).</li>

</ul>

An Assignment is constructed from a ConstraintSatisfactionProblem and is intended to be updated as you search for the solution. It holds

<ul>

 <li>a dictionary from variables to their domains (varDomains)</li>

 <li>a dictionary from variables to their assigned values (assignedValues).</li>

</ul>

Notice that the varDomains in Assignment is meant to be updated, while the varDomains in ConstraintSatisfactionProblem should be left alone.

A new assignment should never be created. All changes to the assignment through the recursive backtracking and the inference methods that you will be implementing are designed to be reversible. This prevents the need to create multiple assignment objects, which becomes very space-consuming.

The constraints in the CSP are represented by two classes:

<ul>

 <li>BinaryConstraint</li>

 <li>UnaryConstraint</li>

</ul>

Both of these store the variables affected and have an isSatisfied function that takes in the value(s) and returns False if the constraint is broken. You will only be working with binary constraints, as eliminateUnaryConstraints has been implemented of for you.

Two useful methods for binary constraints include affects, which takes in a variable and returns True if the constraint has any impact on the variable, and otherVariable, which takes in one variable of the binaryConstraint and returns the other variable affected.

<h1>Questions</h1>

<h2>Question 1: Recursive Backtracking</h2>

In this question you will be creating the basic <strong>recursive backtracking framework </strong>for solving a constraint satisfaction problem.

First, implement the function consistent.

def consistent(assignment, csp, var, value):

“””

Checks if a value assigned to a variable is consistent with all binary constraints in a problem.

<ul>

 <li>Do not assign value to var.</li>

 <li>Only check if this value would be consistent or not.</li>

 <li>If the other variable for a constraint is not assigned, then the newvalue is consistent with the constraint.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment</li>

 <li>csp (ConstraintSatisfactionProblem): the problem definition</li>

 <li>var (string): the variable that would be assigned</li>

 <li>value (value): the value that would be assigned to the variable</li>

</ul>

Returns:

<ul>

 <li>boolean</li>

</ul>

True if the value would be consistent with all currently assigned values, False otherwise.

“””

<ul>

 <li>This function indicates whether a given value would be possible to assign to a variable without violating any of its constraints.</li>

 <li>You only need to consider the constraints in binaryConstraints that affect this variable and have the other affected variable already assigned. Once this is done, implement recursiveBacktracking</li>

</ul>

def recursiveBacktracking(assignment, csp, orderValuesMethod, selectVariableMethod, inferenceMethod):

“””

Recursive backtracking algorithm.

<ul>

 <li>A new assignment should not be created.</li>

 <li>The assignment passed in should have its domains updated with inferences.* In the case that a recursive call returns failure or a variable assignment is incorrect, the inferences made along the way should be reversed.</li>

 <li>See maintainArcConsistency and forwardChecking for the format of inferences.</li>

</ul>

Examples of the functions to be passed in:

<ul>

 <li>orderValuesMethod: orderValues, leastConstrainingValuesHeuristic</li>

 <li>selectVariableMethod: chooseFirstVariable, minimumRemainingValuesHeuristic* inferenceMethod: noInferences, maintainArcConsistency, forwardChecking</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): a partial assignment to expand upon</li>

 <li>csp (ConstraintSatisfactionProblem): the problem definition</li>

 <li>orderValuesMethod (function&lt;assignment, csp, variable&gt; returns list&lt;value&gt;):</li>

</ul>

a function to decide the next value to try

<ul>

 <li>selectVariableMethod (function&lt;assignment, csp&gt; returns variable):a function to decide which variable to assign next</li>

 <li>inferenceMethod (function&lt;assignment, csp, variable, value&gt; returnsset&lt;variable, value&gt;): a function to specify what type of inferences to use</li>

</ul>

Returns:

<ul>

 <li>Assignment</li>

</ul>

A completed and consistent assignment. None if no solution exists. “””

The algorithm is provided as follows, see also Slide 26 of Lecture 12, CSP I.

<ul>

 <li>Ignore for now the parameter inferenceMethod. • This function is designed to take in a problem definition and a partial assignment. When finished, the assignment should either be a complete solution to the CSP or indicate failure.</li>

</ul>

<h2>Question 2: Variable Selection</h2>

While the recursive backtracking method eventually finds a solution for a constraint satisfaction problem, this basic solution will take a very long time for larger problems.

Fortunately, there are heuristics that can be used to make it faster, and one place to include heuristics is in selecting which variable to consider next.

Implement minimumRemainingValueHeuristic.

def minimumRemainingValuesHeuristic(assignment, csp): “””

Selects the next variable to try to give a value to in an assignment. * Uses minimum remaining values heuristic to pick a variable. Use degree heuristic for breaking ties.

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand* csp (ConstraintSatisfactionProblem): the problem description</li>

</ul>

Returns:

<ul>

 <li>the next variable to assign</li>

</ul>

“””

<ul>

 <li>This follows the minimum remaining value heuristic (see Slide 41 of CSP I) to select a variable with the fewest options left and uses the degree heuristic to break ties. • The degree heuristic chooses the variable that is involved in the largest number of constraints on other unassigned variables.</li>

</ul>

<h2>Question 3: Value Ordering</h2>

Another way to use heuristics is to optimize a constraint satisfaction problem solver is to attempt values in a different order.

Implement leastConstrainingValuesHeuristic.

def leastConstrainingValuesHeuristic(assignment, csp, var): “””

Creates an ordered list of the remaining values left for a given variable.

<ul>

 <li>Values should be attempted in the order returned.</li>

 <li>The least constraining value should be at the front of the list.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand</li>

 <li>csp (ConstraintSatisfactionProblem): the problem description* var (string): the variable to be assigned the values</li>

</ul>

Returns:

<ul>

 <li>list&lt;values&gt;a list of the possible values ordered by the least constraining value heuristic</li>

</ul>

“””

<ul>

 <li>This takes in a variable and determines the order in which the possible values should be attempted according to the least constraining values heuristic (see Slide 42 of CSP I), which prefers values that eliminate the fewest possibilities from other variables.</li>

 <li>Your code should be able to solve small CSPs. To test and debug, use py and the functions in Testing.py.</li>

</ul>

<h2>Question 4 (4 points): Forward Checking</h2>

While heuristics help to determine what to attempt next, there are times when a particular search path is doomed to fail long before all of the values have been tried.

Inferences (see Slides 28-36 of CSP I) are a way to identify impossible assignments early on by looking at how a new value assignment affects other variables.

The pseudocode of recursiveBacktracking that integrates inferences is given.

recursive Backtracking with inferences

1: <strong>function </strong>BACKTRACKING-SEARCH(<em>csp</em>) <strong>returns </strong>a solution, or failure

2:                <strong>return </strong>[BACKTRACK({},<em>csp</em>)]

3: <strong>end function</strong>

4: <strong>function </strong>BACKTRACKING(<em>assignment</em>,<em>csp</em>) <strong>returns </strong>a solution, or failure

5:              <strong>if </strong><em>assignment </em>is complete <strong>then</strong>

6:                    <strong>return </strong>[<em>assignment</em>]

7:            <strong>end if</strong>

8:                  var ← SELECT-UNASSIGNED-VARIABLE(<em>csp</em>)

9:                   <strong>for </strong>each <em>value </em>in ORDER-DOMAIN-VALUES(<em>var</em>,<em>assignment</em>,<em>csp</em>) <strong>do</strong>

10:                    <strong>if </strong><em>value </em>is consistent with <em>assignment </em><strong>then</strong>

11:                            add {<em>var </em>= <em>value</em>} to <em>assignment</em>

12:                              <em>inferences </em>← INFERENCE(<em>csp,var,value</em>)

13:                            <strong>if </strong><em>inferences </em>6= <em>failure </em><strong>then</strong>

14:                                     <em>result </em>← BACKTRACK(<em>assignment</em>,<em>csp</em>)

15:                                  <strong>if </strong><em>result </em>6= <em>failure </em><strong>then</strong>

16:                                        <strong>return </strong>[<em>result</em>]

17:                                <strong>end if</strong>

18:                                   add <em>inferences </em>to <em>assignment</em>

19:                         <strong>end if</strong>

20:                  <strong>end if</strong>

21:                      remove {<em>var</em>=<em>value</em>} and <em>inferences </em>from <em>assignment</em>

22:          <strong>end for</strong>

23:           <strong>return </strong>[<em>failure</em>]

24: <strong>end function</strong>

Each inference made by an algorithm involves one possible value being removed from one variable. It should be noted that when these inferences are made they must be kept track of so that they can later be reversed if a particular assignment fails. They are stored as a set of tuples (variable, value).

First, update recursiveBacktracking to deal with the parameter inferenceMethod.

Then, implement forwardChecking.

def forwardChecking(assignment, csp, var, value):

“””

Implements the forward checking algorithm.

<ul>

 <li>Each inference should take the form of (variable, value) where the value</li>

</ul>

is being removed from the domain of variable.

<ul>

 <li>This format is important so that the inferences can be reversed if theyresult in a conflicting partial assignment.</li>

 <li>If the algorithm reveals an inconsistency, any inferences made should bereversed before ending the function.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand</li>

 <li>csp (ConstraintSatisfactionProblem): the problem description</li>

 <li>var (string): the variable that has just been assigned a value</li>

 <li>value (string): the value that has just been assigned</li>

</ul>

Returns:

<ul>

 <li>set&lt; tuple&lt;variable, value&gt; &gt;the inferences made in this call or None if inconsistent assignment “””</li>

</ul>

This is a very basic inferencemaking function.

<ul>

 <li>When a value is assigned, all variables connected to the assigned variable by a binary constraint are considered.</li>

 <li>If any value in those variables is inconsistent with that constraint and the newly assigned value, then the inconsistent value is removed.</li>

 <li>You can test again your code by passing forwardChecking to inferenceMethod. <strong>It should be faster than the previous version in Question 3.</strong></li>

</ul>

<h2>Question 5: Maintaining Arc Consistency</h2>

There are other methods for making inferences than can detect inconsistencies earlier than forward checking. One of these is the Maintaining Arc Consistency algorithm, or the MAC algorithm.

It works like the AC-3 algorithm (see Slide 35 of CSP I). We recall a version of AC-3 where inferences are saved.

The difference between MAC and AC-3 are as follows.

<ul>

 <li>While AC-3 is called as a preprocessing step, which explains why all arcs inserted in the queue, MAC is called during the search.</li>

 <li>After a variable <em>X<sub>i </sub></em>is assigned a value, MAC performs the same operations as AC-3, but starts with only the arcs (<em>X<sub>j</sub>,X<sub>i</sub></em>) for all <em>X<sub>j </sub></em>that are unassigned variables that are neighbors of <em>X<sub>i</sub></em>.</li>

</ul>

First, implement revise.

def revise(assignment, csp, var1, var2, constraint):

“””

Helper function to maintainArcConsistency and AC3.

<ul>

 <li>Remove values from var2 domain if constraint cannot be satisfied.* Each inference should take the form of (variable, value) where the value is being removed from the domain of variable.</li>

 <li>This format is important so that the inferences can be reversed if theyresult in a conflicting partial assignment.</li>

 <li>If the algorithm reveals an inconsistency, any inferences made should bereversed before ending the function.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand</li>

 <li>csp (ConstraintSatisfactionProblem): the problem description</li>

 <li>var1 (string): the variable with consistent values</li>

 <li>var2 (string): the variable that should have inconsistent values removed</li>

 <li>constraint (BinaryConstraint): the constraint connecting var1 and var2</li>

</ul>

Returns:

<ul>

 <li>set&lt;tuple&lt;variable, value&gt;&gt;the inferences made in this call or None if inconsistent assignment</li>

</ul>

“””

This is a helper function that is responsible for determining inconsistent values in a variable.

Then implement maintainArcConsistency.

def maintainArcConsistency(assignment, csp, var, value):

“””

Implements the maintaining arc consistency algorithm.

<ul>

 <li>Inferences take the form of (variable, value) where the valueis being removed from the domain of variable.</li>

 <li>This format is important so that the inferences can be reversed if theyresult in a conflicting partial assignment.</li>

 <li>If the algorithm reveals an inconsistency, and inferences made should bereversed before ending the function.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand</li>

 <li>csp (ConstraintSatisfactionProblem): the problem description</li>

 <li>var (string): the variable that has just been assigned a value</li>

 <li>value (string): the value that has just been assigned</li>

</ul>

Returns:

<ul>

 <li>set&lt;&lt;variable, value&gt;&gt;the inferences made in this call or None if inconsistent assignment “””</li>

</ul>

The MAC algorithm starts off very similar to forward checking in that it removes inconsistent values from variables connected to the newly assigned variable.

The difference is that is uses a <strong>queue </strong>to propagate these changes to other related variables.

<h2>Question 6: Preprocessing</h2>

Another step to making a constraint satisfaction solver more efficient is to perform preprocessing. This can eliminate impossible values before the recursive backtracking even starts.

One method to do this is to use the AC-3 algorithm.

Implement AC3.

def AC3(assignment, csp):

“””

AC3 algorithm for constraint propagation.

<ul>

 <li>Used as a pre-processing step to reduce the problem before runningrecursive backtracking.</li>

</ul>

Args:

<ul>

 <li>assignment (Assignment): the partial assignment to expand* csp (ConstraintSatisfactionProblem): the problem description</li>

</ul>

Returns:

<ul>

 <li>Assignmentthe updated assignment after inferences are made or None if an inconsistent assignment</li>

</ul>

“””

Note it does not need to track the inferences that are made, because if the assignment fails at any point then there is no prior state to back up to. This means that there is no solution to the CSP.

<h1>Grading</h1>

Submit BinaryCSP.py in a zip file to online judge.

<strong>Submission </strong>The BinaryCSP.py is where your entire CSP implementation will reside. You should submit this file (and only this one) with your code and comments to online judge.

<strong>Honor Code </strong>We will be checking your code against other submissions in the class for logical redundancy. If you copy someone else’s code from online and submit it with minor changes, we will know. These cheat detectors are quite hard to fool, so please don’t try.