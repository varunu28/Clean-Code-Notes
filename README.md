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
