# Clean-Code-Notes
Notes on the book Clean Code

## Names
 - If we require a comment to explain name of a variable, then it is not named correctly.
 - Pick a name that people can pronounce.
 - Avoid encodings while picking names i.e. a `string` name should not be named `nameStr` to display that it is a `string`.
 - Choose parts of speech well while picking names. `isSet` should logically return a `boolean` as it asks a question.
 - `Variables` with smaller scope should have smaller names while `variables` with longer scope should have longer names
 - It is opposite with `functions`. `Functions` with longer scope i.e. `public` should have smaller names as they are going to be called from multiple places.
 - `Functions` with smaller scope i.e. `private` should have larger names as they are going to be called from a smaller context.
 - Classes follow the same rules as that of functions in naming. 
 - Derived classes get appended with adjectives to display derivition. `Account -> SavingsAccount`.

## Functions
 - `if/else` chains in a function should be replaced with well-named predicate function.
 - Ideal function size should be 4-6 lines where additional logic should be further function calls.
 - If a function exceeds the ideal function size and has a lot of complicated logic then it should be transformed to a `class` with its own `invoke` method.
 - This `class` should be built of smaller functions that are called from `invoke` function.
 - Large functions are where classes go to hide!
 - As a part of refactoring functions in legacy code, rely on tests that check the behaviour of legacy code. Perform the refactoring in a `TDD` manner.
 - If a function is composed of multiple sections, its doing more than 1 thing. 
 - If a function manipulates more than 1 level of abstraction, its doing more than 1 thing.
 - In order for a function to do just one thing, it should not cross one level of abstraction.
 - If you are unable to extract a functionality from a function, then originally function was not doing just one thing.
 - Extract as long as you can extract no more i.e. `Extract till you drop`.

## Comments
 - Comments don't make up for bad code.
 - While adding a comment, always think if there is a way to avoid comment and make the code self explanatory.
 - Treat comment as a failure.
 - The only good comment is the comment you found a way not to write.
 - If you end up writing comment, try to ensure that the comment does not goes out of sync with the actual code in future iterations.
 - Comments can act as warning. If you want someone to not change a code or provide instructions for reuse of code, warning comments might come in handy.
 - `TODO` comments can be used to plan the future changes of the code. Make sure to scan the codebase regularly for `TODO` and put an effort to reduce `TODO` with each iteration.
 - Comments can be used to amplify changes that might seem minor to others such as `trim`, `length - 1` etc.
 - For public facing APIs, have a very clean and clear `docstring`. `Javadoc` for example.
 - Avoid redundant comments at all cost. A comment that provides no extra information in addition to reading the actual code should be removed.
 - A comment that contradicts the behavior of actual code should be considered misleading comment. They create confusion for anyone reading the code.
 - Always prefer for a scenario where you can remove a comment with a function call or a variable. 
```
// Check if customer is eligible for discount
if (customer.age > 25 && customer.salary > 20000 && customer.salary < 60000) 
                               
                               VS
                               
if (customer.isEligibleForDiscount()) 

                               OR 
                               
if (checkIfCustomerIsEligibleForDiscount(customer)
```
 - Don't have comments for things that can be taken care by a version control system. Ex: comments about author's name or date the change was added or commit hash
 - Don't have commented out code. Delete it and if required in future, you can fetch it from git based on the commit hash when it was deleted.
 - Add a comment as local to the code it describes. Don't describe global state in the comments.
 - Make sure the comments have a clear connection with the code. Your comment should not confuse the reader about the code.

## Formatting
 - Having badly formatted code gives an impression to the reader that other parts of the code will also have some hidden discrepencies.
 - Having a set rules of formatting for the project and an automated tool to enforce the formatting helps to maintain the codebase.
 - Prefer small size over larger files.
 - Consider the newspaper as an example for maintaining a file. The filename should be clear enough to give a high-level overview of what the code does. As we go down in the file the lower level details should be exposed in the linear order of file depth.
 - Use blank lines judicially to describe separation of concepts and new ideas in a code.
 - Lines of code that are tightly linked should appear together without any break such as a blank line or a comment.
 - Concepts that are closely related should be kept vertically close to each other.
 -  Variables should be declared as close to their usage as possible.
 -  Variables used for control of a loop should be declared as a part of their loop whenever possible.
 -  Instance variables should be defined at the starting of the class. Anyone reading the class definition should understand what variables does this class use before going to the functions the class describes.
 -  If one function calls another function then they should be vertically close to each other. Preferrabely the caller function should be above the function getting called.
 -  Functions with same **conceptual affinity** should be vertically close to each other. Conceptual affinity can mean functions calling each other, overloaded constructors or functions performing operations of similar behavior.
 -  Use hortizontal space to associate things that are closely related and disassociate things that are weakly related.

## Objects & Data Structures
 - A class should not expose the implementation details and its variables to the user. It should rather provide an abstraction to the underlying implementation so that it holds the flexibility to change the type and implementation details in the future
 ```
 class Car {
   public double getCurrentFuelCapacity();
   public double getTotalFuelCapacity();
 }
               VS
 class Car {
   public double getPercentageFuelCapacity();
 }
 ```
  - **Objects** hide their data behind abstractions and expose functions that operate on that data. **Data Structures** expose their data and have no meaningful operations.
  - Procedural code makes it easy to add new functions without changing data structure. OO code makes it easy to add new data structures without changing existing functions.
  - **Law of Demeter** states that a module should not know internal details of the objects it manipulates.
  - **Train Wreck** code is when a code is represented as a chain of function calls on one another.
```
String output = ctct.getOptions().getScratchDir().getAbsolutePath();

              VS
 
 Options options = ctxt.getOptions();
 File scratchDir = options.getScratchDir();
 String output = scratchDir.getAbsolutePath();
```
 - Avoid hybrids which are half objects and half data structures i.e. they do concrete operations but also allow their users to access and mutate the data through public getters and setters.
 - **Data Transfer Objects(DTO)** is a class with public variables and no functions. 
 - **Beans** have private variables manipulated by getters and setters.
 - **Active Record** is a special form of DTO. They contain public variables with methods such as `save`, `find`. They can be a direct translations of database tables. Make sure to not add any business logic in an active record as doing so will turn them into a hybrid. 
 
## Error Handling
 - Use exceptions than returning error code. It makes the caller code more cleaner as the code doesn't need to assert for exact error codes and have to handle the exception explicitly.
 - In error handling, the `try-catch-finally` follows a pattern where each step under `try` block can result in failure and stop execution and the execution resumes in the `catch` block.
 - The `catch` block should leave the program in a consistent state and revert any changes done by failing `try` block.
 - Try to write tests that force exceptions and then add business logic to make your tests pass.
 - Avoid checked exceptions as they force us to make changes to multiple levels from where the actual function throwing the exception is called.
 - Every exception that is thrown should provide context in which the exception occurs and the source of code that resulted in exception.
 - Exceptions thrown by third party library should be converted to an exception that user can understand. This can be done by creating a wrapper for the library. The wrapper will convert the exceptions thrown by library to exceptions that make more sense to users of your code.
 - If you have a case where you don't abort the program in case of an exception but rather change the behavior then its better to abstract the exception and have a normal flow for the actual use case.
 - **Special Case Pattern** is when you create a class or configure an object so that it handles a special case for you. This can be used when the client code doesn't needs to deal with an exception that results in crashing of your program but expects a different behavior in case of exception.
 - Don't return a **null**. Always prefer throwing an exception or a special case objects. For example if a returned list doesn't exists then rather than returning a null, return an empty list.
 - Don't pass **null**. Prefer throwing a well formed exception when you receive a null parameter.
 - Clean code has an attribute of separating error handling to an extent that it doesn't clutters the actual business logic and makes the code readable without jumping around the exception handling code.

## Boundaries
 - When using a boundary interface like `Map`, usage of the interface should be encapsulated rather than allowing the users to directly manipulate it by passing the `Map` to them.
 - When using third party APIs, write tests that describe the desired usage of these APIs. These tests are called **learning tests**.
 - **Learning tests** are useful when we are figuring out the correct usage of an API and understand how to best use it to fulfil our use case. They also help us in recognizing any breaking changes that might come up with API version update.
 - While using code that is out of our control, keep clear boundaries on the way our code depends on the third party code so that future changes in third party APIs do not create huge rework for our system.
 - While using third party code, prefer to use **adapter** pattern in order to reduce dependency on the third party code. This helps us in creating a *plug and play* environment rather than being tightly coupled to the third party code.

## Unit Testing
 - Laws of Test Driven Development(TDD):
   - You may not write production code until you have a failing test.
   - You may not write more of a unit test than is sufficient to fail & not compiling is failing
   - You may not write more pf production code than it is sufficient to pass failing test.
 - Test code is as important as production code. It must be kept as clean as production code.
 - Having a clean test suite provides you with a flexibility to change the architecture/refactor your code without thinking of side effects as there are tests to protect you.
 - Having a domain specific testing language(**DSTL**) helps in building a clean test. This **DSTL** can be used for test setup, custom assertion etc.
 - Number of asserts in a test should be minimized, preferabely one.
 - Test one concept only per test.
 - Clean test should be **FIRST**:
   - **F**ast
   - **I**ndependent
   - **R**epeatable
   - **S**elf Validating
   - **T**imely 

## Classes
 - A class should have *public static constants* on the top, followed by *private static constants* and then *private instance variables*.
 - Public functions should follow the variables. Private functions called by the public functions should come right after the public functions.
 - Classes should be smaller in size. A size of a class is measured by the count of its responsibilities.
 - **Single Responsibility Principle(SRP)** states that a class should have one responsibility and only one reason for change. We want a system to have multiple small classes, each having single responsibility and single reason for change. All these classes work with each other to achieve the desired system behaviour.
 - Classes should have small number of instance variables and each method should manipulate one or more of these methods. A class in which each variable is used by each method is maximally **Cohesive**.
 - We should aim for maximum cohesion and assume low cohesion as an opportunity to break a class into two or more classes.
 - Breaking larger function into smaller functions often gives us an opportunity to split a class into smaller classes with higher cohesion.
 - **Open Closed Principle(OCP)** states that classes should be open for extention but closed for modification. In an ideal system adding a new feature should be done by extending current functionality rather than modifying the existing ones.
 - A client class should have an interface as its dependencies rather than a concrete implementation.
 - **Dependency Inversion Principle(DIP)** states that our classes should depend upon abstractions and not concrete details. This promotes loose coupling among client classes and concrete implementations.

## Systems
 - Software systems should separate the startup process from the runtime logic that takes over after the startup process.
 - One approach for construction of startup dependencies is to move all the construction logic and wiring to main function. All other modules assume that all dependencies are already fulfiled without knowing any internals of the main function.
 - When we have a use case where a construction is handled by business logic then we can make use of an **ABSTRACT FACTORY** pattern.
 - **Dependency Injection** is a powerful mechanism for separating dependency construction from its use.
 - **Aspect oriented programming(AOP)** helps in creating a decoupled architecutre. Each component of the system will have an aspect related to system such as persistence.
 - **AspectJ** is an extention of Java language that provides first class support for aspects as modularity constructs.
 - Separation of concerns helps in testing the architecture to a great extent.
 - Systems need domain specific languages(DSL). A DSL allows all levels of abstractions and all domains in the application to be expresed as *POJOs*, from high level policy to low-level details.

## Emergence
 - Kent Beck's four rules for simple design. A design is simple if it follows the below rules:
   - Runs all the tests.
   - Contains no duplicates.
   - Expresses the intent of programmers.
   - Minimizes number of classes and methods.
 - **Template method pattern** helps in removing higher level duplication.

## Concurrency
 - Concurrency is a decoupling strategy. It decouples *what gets done* from *when it gets done*.
 - Common myths associated with concurrency:
   - Concurrency always improves performance.
   - Design does not change when writing concurrent programs.
   - Understanding concurrency issues is not important when working with a container such as **Web or EJB container**.
 - Some important points about concurrency:
   - Concurrency incurs some overhead.
   - Correct concurrency is complex.
   - Concurrency bugs aren't often repeatable.
   - Concurrency often requires a fundamental change in design strategy.
 - Concurrency defense principles:
   - Keep your concurrency related code separate from other code.
