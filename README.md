Download Link: https://assignmentchef.com/product/solved-csci1913-lab-1
<br>
It’s possible to solve an equation <em>numerically, </em>by substituting numbers for its variables. It’s also possible to solve an equation <em>symbolically, </em>by using algebra. For example, to solve the equation <em>m</em>×<em>x</em>+<em>b </em>= <em>y </em>symbolically for <em>x</em>, you’d first subtract <em>b </em>from both sides, giving <em>m</em>×<em>x </em>= <em>y</em>−<em>b</em>. Then you’d divide both sides by <em>m</em>, giving <em>x </em>= (<em>y</em>−<em>b</em>) / <em>m</em>. You may assume that no variable is equal to zero.

In this laboratory exercise, you’ll write a Python program that uses algebra to solve simple equations symbolically. Your program will use Python tuples to represent equations, and Python strings to represent variables. To simplify the problem, the equations will use only the binary arithmetic operators ‘+’, ‘−’, ‘×’, and ‘/’. Also, your program need only solve for a variable that appears exactly once in an equation.

<ol>

 <li></li>

</ol>

Here’s a mathematical description of how your program must work. First, <em>L </em>→ <em>R </em>means that an equation <em>L </em>is algebraically transformed into a new equation <em>R</em>. For example:

<h1>A + B = C → B = C−A</h1>

Second, a variable is said to be <em>inside </em>an expression if it appears in that expression at least once. For example, the variable <em>x </em>is inside the expression <em>m</em>×<em>x</em>+<em>b</em>, but it isn’t inside the expression <em>u</em>−<em>v</em>. Each variable is considered to be inside itself, so that <em>x </em>is inside <em>x</em>.

Now suppose that <em>A</em>◦<em>B </em>= <em>C </em>is an equation, where <em>A</em>, <em>B</em>, and <em>C </em>are expressions, and ◦ is one of the four binary arithmetic operators. Also suppose that the variable <em>x </em>is inside either <em>A </em>or <em>B</em>. Then the following rules show how this equation can be solved for <em>x</em>.

is inside <em>A</em>

(1)

is inside <em>B </em>is inside <em>A</em>

(2)

is inside <em>B </em>is inside <em>A</em>

(3)

is inside <em>B </em>is inside <em>A</em>

(4) is inside <em>B</em>

For example, I can use the rules to solve the equation <em>m</em>×<em>x </em>+ <em>b </em>= <em>y </em>for <em>x</em>. In Rule 1, <em>A </em>is <em>m</em>×<em>x</em>, and <em>B </em>is <em>b</em>. Since <em>x </em>is inside <em>A</em>, I can transform the equation to <em>m</em>×<em>x </em>= <em>y</em>−<em>b</em>. Then in Rule 3, <em>A </em>is <em>m</em>, and <em>B </em>is <em>x</em>. Since <em>x </em>is inside <em>B</em>, I can transform the equation to <em>x </em>= (<em>y</em>−<em>b</em>)<em>/m</em>. Now <em>x </em>is alone on the left side of the equal sign, so the equation is solved. This solution used only two rules, but a more complex equation might use more rules, and it might use rules more than once.

<ol start="2">

 <li><strong> Representation.</strong></li>

</ol>

Your program must represent operators and variables as Python strings. For example, it must represent the variable <em>x </em>as the string ‘x’. It must also represent equations and expressions as Python tuples with three elements each. For example, it must represent the expression <em>a </em>+ <em>b </em>as the Python tuple (‘a’, ‘+’, ‘b’). These tuples can be nested, so that the equation <em>m</em>×<em>x </em>+ <em>b </em>= <em>y </em>is represented like this:

(((‘m’, ‘*’, ‘x’), ‘+’, ‘b’), ‘=’, ‘y’)

The asterisk ‘*’ is used as the multiplication operator. If you ignore the parentheses, commas, and quotes, then this tuple looks much like how you’d write the original equation. It’s helpful to define functions left, op, and right that return the parts of tuples that represent expressions.

def left(e):

return e[0]

def op(e):

return e[1] def right(e):

return e[2]

For example, if e is the tuple (‘a’, ‘+’, ‘b’), then left(e) returns ‘a’, op(e) returns ‘+’, and right(e) returns ‘b’.

<ol start="3">

 <li></li>

</ol>

Your program must define the following Python functions, and those functions must behave as described here. You must use the same function names as I do, but you need not use the same parameter names as I do. Your functions cannot change elements of the tuples that are passed to them as arguments, because unlike lists, tuples are immutable. <em>You will receive zero points for this assignment if you use Python lists in any way!</em>

<ul>

 <li>isInside(v, e). Test if the variable v is inside the expression e. It’s inside if (1) v equals e, or (2) v is inside the left side of e, or (3) v is inside the right side of e. This definition is recursive. Other functions in your program will need to call isInside. Hint: Don’t use the Python operator in when you write this function: it doesn’t do what you want here.</li>

 <li>solve(v, q). Solve the equation q for the variable v, and return a new equation in which v appears alone on the left side of the equal sign. For example, if you call solve like this:</li>

</ul>

solve(‘x’, (((‘m’, ‘*’, ‘x’), ‘+’, ‘b’), ‘=’, ‘y’))

then it will return this:

(‘x’, ‘=’, ((‘y’, ‘-‘, ‘b’), ‘/’, ‘m’))

The function solve really just sets things up for the function solving, which does all the work. If v is inside the left side of q, then call solving with v and q. If v is inside the right side of q, then call solving with v and a new equation like q, but with its left and right sides reversed. In either case, return the result of calling solving. If v is not inside q at all, then return None.

<ul>

 <li>solving(v, q). Whenever this function is called, the variable v must be inside the left side of q. If v is equal to the left side of q, then the equation is solved, so simply return q. Otherwise, decide which of the four transformation rules (from Section 1) must be used next to solve q. Call the function that implements that rule on v and q, then return the result.</li>

 <li>solvingAdd(v, q). Use rule 1 to transform the equation q, then call solving on the variable v and the transformed q. Return the result of calling solving.</li>

 <li>solvingSubtract(v, q). Use rule 2 to transform the equation q, then call solving on the variable v and the transformed q. Return the result of calling solving.</li>

 <li>solvingMultiply(v, q). Use rule 3 to transform the equation q, then call solving on the variable v and the transformed q. Return the result of calling solving.</li>

 <li>solvingDivide(v, q). Use rule 4 to transform the equation q, then call solving on the variable v and the transformed q. Return the result of calling solving.</li>

</ul>

The functions solvingAdd, solvingSubtract, solvingMultiply, and solvingDivide will have very similar definitions. If you can write one of these functions, then you can also write the other three. You need not write any kind of user interface that reads equations from the keyboard, or writes equations to the display. Instead, just call the function solve directly with a nested tuple that represents an equation.

<ol start="4">

 <li></li>

</ol>

The file tests1.py on Canvas contains a series of tests. Each test calls a function from your program, then prints what the function returns. Each test also has a comment that says what it should print, and how many points it is worth.

To grade your work, the TA’s will examine the results of your tests. If a test prints exactly what it should, without error, then you will receive all the points for that test. If a test does anything else, then you will receive no points for that test. Your score for this assignment will be the sum of points you receive for all the tests.