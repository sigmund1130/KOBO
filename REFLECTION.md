# CA1 Reflection

## 1. Fractional numbers

The fractional-number decision is at `src/scanner.rs:118`. The scanner accepts a dot as part of a number only when `peek()` is `.` and `peek_next()` is an ASCII digit. With `5.`, the integer part is scanned first as NUMBER `5`; because no digit follows the dot, the condition is false and the dot remains for the next scan. The scanner then reports the dot as a character that is not part of any token. This follows section 1.4 of the language specification: a Kobo number is one or more digits, optionally followed by a dot and one or more digits. The input `5.` lacks the required digit after its dot, while `0.5` and `12.75` satisfy the rule. The lookahead preserves valid decimal literals without incorrectly consuming a stray dot.

## 2. Line numbers and EOF

The line counter changes at `src/scanner.rs:83` when the scanner encounters a newline outside a string. It also changes at `src/scanner.rs:97` while scanning a string literal that spans a newline. These increments ensure later tokens and errors report the correct source line. EOF is handled at `src/scanner.rs:37` and `src/scanner.rs:41`: instead of using the current line after whitespace has been consumed, the scanner takes the line from the last real token, using line 1 for an empty file. Therefore, a file that ends with two blank lines has the same EOF line number as the same file without those blank lines. This matches section 6.1, which specifies that trailing editor newlines must not change the token stream or EOF location.

## 3. Development history

The current scanner builds successfully and the supplied phase-1 suite reports 14/14 passing tests. The repository currently has one completed scanner commit. I do not have genuine earlier personal commits showing a failing test and a subsequent correction, so I cannot truthfully give the before-and-after commit hashes or quote an earlier incorrect line. I have therefore not invented that evidence. The implementation was checked against the stated scanner rules, including lookahead for fractional numbers, multi-line strings, comments, line-number handling, keywords, and EOF token placement.
