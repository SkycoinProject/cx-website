# CX Examples

## Hello World

A classic. _printStr_ is part of the core module or package, and it is automatically added to any CX program, so we don't need to explicitly include it.

Something interesting in this example (as can be noted in the [CX Playground](/editor)) is that this program will print "Hello World!" two times. What's actually happening is that CX is printing to the standard output the string, and then the program is exiting, returning a string.

```cx
package main

func main () (out str) {
     printStr("Hello World!")
}
```


## Comments

Comments in CX follow the same syntax as in many other popular languages, like C and Go. Use // to comment out one line, and /\* ... \*/ to comment out multiple lines in a source file.

```cx
package main

func main () (out i32) {
	// This won't be evaluated
	//out := divI32(5, 0) // This won't either

	out := divI32(10, 5) // The program will return 2

	/*
        Comment block
	out := subF32(3.33, 1.11)
	out := subF32(3.33, 1.11)
	out := subF32(3.33, 1.11)
	out := subF32(3.33, 1.11)
        */
}
```

## Definitions

A definition is just a global variable. This means that any function in the current module or in those modules that have imported the current module can access the definition's value.

In this example, we can see that both _printGlobals_ and _main_ are accessing the definitions _global1_ and _global2_.

```cx
package main

var global1 i32 = 5
var global2 f32 = 3.14159

func printGlobals () () {
	printI32(global1)
	printF32(global2)
}

func main () (out i32) {
	printGlobals()
	printF32(global2)
	printI32(global1)
}
```

## Functions

The example defines two functions: one for calculating a circle's area, and another for calculating a circle's perimeter. A feature in CX is that the output of a function can be explicitly defined, as in the case of _circlePerimeter_. If the output is not explicit, CX will take the last expression as the output of the function.

```cx
package main

var PI f32 = 3.14159

func circleArea (radius f32) (area f32) {
	mulF32(mulF32(radius, radius), PI)
}

func circlePerimeter (radius f32) (perimeter f32) {
	perimeter := mulF32(mulF32(2, radius), PI)
}

func main () (area f32) {
	area := circleArea(2)
	printF32(circlePerimeter(5))
}
```

## Structs

User-defined types can be created by the use of structs. In this example, a type called "Point" is created. We want a point to be defined by a name, and its x and y coordinates on a plane.

The _main_ function defines two variables of type Point, one at **line 12**, and another at **line 17**. **Lines 11, 12, and 13** try to print the values of an uninitialized instance of Point, but this won't crash the program, as CX initializes the fields to their corresponding zero value. For example, a _str_ will be initialized to an empty string, a _bool_ will be initialized to false, and an _i32_ to a 32 bit integer 0.

At **line 15** another instance of Point is created, and their x and y fields are initialized at **lines 16 and 16**. Finally, at **line 19**, we perform the addition of both fields and its sum serves as the output of the program.

```cx
package main

type Point struct {
	name str
	x i32
	y i32
}

func main () (out i32) {
	var uPoint Point
	printStr(uPoint.name)
	printI32(uPoint.x)
	printI32(uPoint.y)

	var myPoint Point
	myPoint.x = addI32(5, 10)
	myPoint.y = addI32(myPoint.x, 20)

	out = addI32(myPoint.x, myPoint.y)
}           
```

## Arrays

Three arrays are created to simulate the data of four students. They hold the ids, ages and grades. At **lines 9, 11, and 13** array reader functions are used to access the values at index 1 for each of the arrays.

```cx
package main

func main () (out str) {
	var ids []i32 = []i32{433, 561, 652, 984}
	var ages []i32 = []i32{23, 21, 26, 31}
	var grades []f32 = []f32{8.8, 9.4, 9.3, 8.3}

	printStr("Student's ID:")
	printI32(readI32A(ids, 1))
	printStr("Student's age:")
	printI32(readI32A(ages, 1))
	printStr("Student's age:")
	printF32(readF32A(grades, 1))
	printStr("done.")
}
```

## Expressions

Four functions and a main function are defined in this example. _sayHi_ demonstrates the definition of an input-less and output-less function. _sayMyName_ receives one parameter as input, but does not return anything. _returnName_ receives no parameters as input, but returns one output.

_multiReturn_ is a bit more interesting than the other functions, as it receives two input parameters, and returns four output parameters. This function takes two 32 bit integers and returns their addition, subtraction, multiplication and division. The main function calls each of these functions, and assigns its output with the third output parameter from _multiReturn_.

```cx
package main

func sayHi () () {
	printStr("Hi")
}

func sayMyName (name str) () {
	printStr(name)
}

func returnName () (name str) {
	name := idStr("Bart")
}

func multiReturn (num1 i32, num2 i32) (add i32, sub i32, mul i32, div i32) {
	add := addI32(num1, num2)
	sub := subI32(num1, num2)
	mul := mulI32(num1, num2)
	div := divI32(num1, num2)
}

func main () (out i32) {
	sayHi()
	sayMyName("Homer")
	printStr(returnName())
	a, s, out, d := multiReturn(20, 20)
}
```

## Go-to

_goTo_ **should not** be used, as it can create programs that are very hard to debug. CX uses the go-to operator to create other flow-control structures, such as _if_ and _for_.

Nevertheless, if you really require a go-to in your program, you can use it. The example above shows how an if-else can be constructed using two go-to expressions. _goTo_ takes three arguments as its parameters: a predicate, a number of lines the program should advance if the predicate evaluates to true, and a number of lines the program should advance if the predicate evaluates to false.

As in the case of the second _goTo_ in the example, if you give it a number of lines which exceeds the number of expressions in the function, this will only cause the function to return. Negative numbers can also be provided, and they will cause the program to re-evaluate the N expressions above.

```cx
package main

func basicIf (num i32) (num i32) {
	pred := gtI32(num, 0)
	goTo(pred, 1, 3)
	printStr("Greater than 0")
	goTo(true, 10, 0)
	printStr("Less than 0")
}

func main () (out i32) {
	basicIf(5)
}
```

## If and if/else

If and if/else statements in CX work as in any other programming language. If the condition evaluates to true, the first block of expressions will be executed; if the condition evaluates to false, the second block of expressions will be executed.

Like in Go, the condition doesn't need to be enclosed in parenthesis.

```cx
package main

func main () (out i32) {
     	if false {
		error := divI32(50, 0)
		printStr("This will never be printed")
	}

	if true {
		printStr("This will always print")
	}

	if gtI32(5, 3) {
		printStr("5 is greater than 3")
	}

	if eqStr("password123", "password123") {
		printStr("Access granted")
	} else {
		printStr("Access denied")
	}

	if lteqI32(50, 5) {
		out = idI32(100)
	} else {
		out = idI32(200)
	}
}
```

## Looping

You can use _for_ to create a loop. As in other programming languages that implement a for statement, a condition is provided which will instruct the computer to evaluate a block of expressions while the condition evaluates to true. In the case of the example above, the for loop will print the numbers from 0 to 10.

```cx
package main

func main () (out i32) {
	var out i32 = 0
	for ltI32(out, 10) {
		printI32(out)
		out = addI32(out, 1)
	}
}
```

## Evolving a function

_evolve_ is perhaps the most complicated native function in CX, not only because it is the native function that receives the greater number of parameters, but because of its purpose.

_evolve_ follows the principles of evolutionary computation. In particular, _evolve_ performs a technique called genetic programming. Genetic programming tries to find a combination of operators and arguments that will solve a problem. For example, you could instruct evolve to find a combination of operators that, when sent 10, returns 20. This might sound trivial, but genetic programming and other evolutionary algorithms can solve very complicated problems.

In this example, we have a number of inputs defined at **line 3**, and we want to find a function which maps them to the outputs defined at **line 7**. The function _solution_ could be totally empty, but we can help _evolve_ a little bit. We know that a close solution to the problem is to multiply n by n, so we add that expression.

The call to _evolve_ at **line 18** takes "solution" as its first argument. This instructs the evolutionary algorithm to evolve the _solution_ function. Next, we provide the function with a set of functions to choose from when the algorithm starts creating different combinations. This argument is actually a regular expression, so we could send it "." to match any function in the current program. However, in genetic programming it's always useful to limit the algorithm to a small set of operators. If we have a very large set of operators, the algorithm could take too long to find a solution.

The next two parameters are the set of inputs and outputs we want to create a function for. Then, the next parameter defines the number of expressions we want the target solution function to be limited to. In traditional genetic programming a limit doesn't exist, and the resulting programs are usually very large. CX's _evolve_ follows a strategy used in cartesian genetic programming (CGP). In CGP, a limit of statements or expressions is given, and this eliminates bloat in the solutions.

The last two parameters are know as the stop criteria. Evolutionary algorithms are heuristics, which means that they won't necessarily deliver the optimal solution to a problem. By using the stop criteria, we can tell _evolve_ to give us the best solution it could find after N iterations, or if the error obtained is less than certain threshold.

If you run the example above, you should get a list of errors printed on the screen. These errors will decrease as the evolution algorithm progresses. When either the error threshold or the number of iterations is reached, the evolutionary algorithm will stop and the program will print what the evolved solution function evaluates to when sent an argument of 30.

```cx
package main

var inps []f64 = []f64{
	-10.0, -9.0, -8.0, -7.0, -6.0, -5.0, -4.0, -3.0, -2.0, -1.0,
	0.0, 1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0, 10.0}

var outs []f64 = []f64{
	90.0, 72.0, 56.0, 42.0, 30.0, 20.0, 12.0, 6.0, 2.0, 0.0, 0.0,
	2.0, 6.0, 12.0, 20.0, 30.0, 42.0, 56.0, 72.0, 90.0, 110.0}

func solution (n f64) (out f64) {
	square = mulF64(n, n)
}

func main () (out f64) {
	:dStack false;
	:dProgram true;
	evolve("solution", "addF64|mulF64|subF64", inps, outs, 5, 300, f32ToF64(0.1))

	printStr("Extrapolating solution")
	printF64(solution(f32ToF64(30.0)))
}
```

## Meta-programming commands

This example is not meant to be run as a program. It's actually a simulation of a REPL session. The reason behind this is that meta-programming commands (unlike meta-programming functions) are most useful when used in an interactive environment.

In this example, three meta-programming commands are used: dStack, dProgram, and step. The program that was loaded into the REPL is the example from [evolving a function](evolving-a-function). _:dStack_ is used for telling the REPL that it should start/stop printing the call stack each time the program advances. :dProgram is like :dStack, but it tells the REPL to print the program's abstract syntax tree. The "d" at the start of these commands stands for "debug", and this tells us that both of these commands are used for debugging purposes only.

:step takes an integer as its argument, and tells CX to run forwards or backwards the number of steps indicated by this integer. It is worth noting that giving :step a negative number is not the same as, for example, using _goTo_ to get back to a previous expression in a function. :step will "forget" anything that happened in the last N steps, if N is negative. If N is positive, it's the equivalent of a normal execution of a CX program.

```
* :dStack false;
* :dProgram true;

Program
0.- Module: main
	Definitions
		0.- Definition: inps []f64
		1.- Definition: outs []f64
	Functions
		0.- Function: solution (n f64) (out f64)
			0.- Expression: square = mulF64(n f64, n f64)
			1.- Expression: var_8 = addF64(n f64, square )
			2.- Expression: var_300 = addF64(square , square )
			3.- Expression: var_303 = mulF64(square , var_8 )
			4.- Expression: out = addF64(n f64, square )
		1.- Function: main () (out f64)
			0.- Expression: nonAssign_0 = f32ToF64(0.1 f32)
			1.- Expression: nonAssign_2 = printStr("Extrapolating solution" str)
			2.- Expression: nonAssign_3 = f32ToF64(30 f32)
			3.- Expression: nonAssign_4 = solution(nonAssign_3 )
			4.- Expression: nonAssign_5 = printF64(nonAssign_4 )

* :step 100;
Evolving function 'solution'
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
5.238095238095238
0
Finished evolving function 'solution'
Extrapolating solution
930
```

## Meta-programming functions

In this example we can see one of the special features of CX: meta-programming functions. Unlike meta-programming commands, which unleash their true potential only in the REPL, meta-programming functions can be used in CX expressions and modify a program's structure in runtime.

We can see that at **line 8**, a division by 0 is defined, but the program won't crash because we are removing this expression by calling _remExpr("main", 1)_ at **line 7**, i.e., we are telling CX to remove expression 1 (the second one, as expressions are zero-indexed) from the "main" function.

At **line 9**, we instruct CX to add an _printStr_ expression. This call to _addExpr_ would normally crash the program, as it's adding an argument-less call to the function. However, we then ask CX to add whatever it wants as an argument to the expression we just added, at **line 10**. The "." (a regular expression) we are sending to _exprAff_ is telling CX to apply any affordance it wants.

As a more elaborated example of affordances and meta-programming functions, check the [robot simulator](robot-simulator).

```cx
package main

var greeting str = "Meta hello."
var farewell str = "Meta good-bye."

func main () () {
	remExpr("toBeRemoved")
	:tag toBeRemoved;
	divI32(3, 0)
	addExpr("newPrint", "printStr")
	affExpr("newPrint", ".", 0)
}
```

## Factorial

The _factorial_ function defined above shows how one can use recursion in CX. If we give the compiler/interpreter the :dStack meta-programming command, we'd see how CX calls _factorial_ several times, and how each of these calls are waiting for the next to return.

```cx
package main

func factorial (num i32) (fact i32) {
	if eqI32(num, 1) {
		fact := idI32(1)
	} else {
		fact := mulI32(num, factorial(subI32(num, 1)))
	}
}

func main () (out i32) {
	factorial(6)
}
```

## Robot simulator

This example tries to illustrate most of the concepts shown in the previous examples.

```cx
package main

var goNorth str = "going north."
var goSouth str = "going south."
var goWest str = "going west."
var goEast str = "going east."

func robotAct (row i32, col i32, action str) (row i32, col i32) {
	printStr(action)
	if eqStr(action, "going north.") {
		row = addI32(row, -1)
	}
	if eqStr(action, "going south.") {
		row = addI32(row, 1)
	}
	if eqStr(action, "going east.") {
		col = addI32(col, 1)
	}
	if eqStr(action, "going west.") {
		col = addI32(col, -1)
	}
}

func robot (row i32, col i32) (row i32, col i32) {
	remArg("robotAct")
	affExpr("robotAct", "goNorth|goSouth|goWest|goEast", 0)
	:tag robotAct;
	row, col = robotAct(row, col, "")
}

func map2Dto1D (r i32, c i32, w i32) (i i32) {
	i = addI32(mulI32(w, r), c)
}

func map1Dto2D (i i32, w i32) (r i32, c i32) {
	r = divI32(i, W)
	c = modI32(i, W)
}

func robotObjects (row i32, col i32, width i32, wallMap []bool, wormholeMap []bool) () {
	remObjects()
	if readBoolA(wallMap, map2Dto1D(addI32(row, -1), col, width)) {
		addObject("northWall")
	}
	if readBoolA(wallMap, map2Dto1D(addI32(row, 1), col, width)) {
		addObject("southWall")
	}
	if readBoolA(wallMap, map2Dto1D(row, addI32(col, 1), width)) {
		addObject("eastWall")
	}
	if readBoolA(wallMap, map2Dto1D(row, addI32(col, -1), width)) {
		addObject("westWall")
	}

	if readBoolA(wormholeMap, map2Dto1D(addI32(row, -1), col, width)) {
		addObject("northWormhole")
	}
	if readBoolA(wormholeMap, map2Dto1D(addI32(row, 1), col, width)) {
		addObject("southWormhole")
	}
	if readBoolA(wormholeMap, map2Dto1D(row, addI32(col, 1), width)) {
		addObject("eastWormhole")
	}
	if readBoolA(wormholeMap, map2Dto1D(row, addI32(col, -1), width)) {
		addObject("westWormhole")
	}
}

func main () (out str) {

	setClauses("
          aff(robotAct, goNorth, X, R) :- X = northWall, R = false.
	  aff(robotAct, goSouth, X, R) :- X = southWall, R = false.
	  aff(robotAct, goWest, X, R) :- X = westWall, R = false.
	  aff(robotAct, goEast, X, R) :- X = eastWall, R = false.

	  aff(robotAct, goNorth, X, R) :- X = northWormhole, R = true.
	  aff(robotAct, goSouth, X, R) :- X = southWormhole, R = true.
	  aff(robotAct, goWest, X, R) :- X = westWormhole, R = true.
	  aff(robotAct, goEast, X, R) :- X = eastWormhole, R = true.
        ")

	setQuery("aff(%s, %s, %s, R).")

	var wallMap []bool = []bool{
		true, true,  true,  true,  true,
		true, false, true, false, true,
		true, false, true, false, true,
		true, false, false, false, true,
		true, true,  true,  true,  true}

	var wormholeMap []bool = []bool{
		false, false, false, false, false,
		false, false, false, false, false,
		false, false, false, false, false,
		false, false, false, false, false,
		false, false, false, false, false}

	var width i32 = 5
	var row i32 = 1
	var col i32 = 1

	var counter i32
	for ltI32(counter, 6) {
		wallMap = writeBoolA(wallMap, map2Dto1D(row, col, width), true)
		wormholeMap = writeBoolA(wormholeMap, map2Dto1D(row, col, width), false)
		robotObjects(row, col, width, wallMap, wormholeMap)
		row, col := robot(row, col)
		counter = addI32(counter, 1)
	}

	printStr("done.")
}
```
