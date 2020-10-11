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
