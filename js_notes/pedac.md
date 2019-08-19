# The PEDAC Problem Solving Process
- Understand the **Problem**
- **Example**/Test Cases
- **Data** Structure and Algorithm
- **Code**

## PEDAC
|Objective           |Step                  | Description
|:-------------------|:---------------------|:----------
|Process the Problem |Understand the Problem| Indentify expected input/output. Make requirements explicit. Identify rules. Mental model of problem.
|                    |Examples/Text Cases   | Validate understanding of the problem
|                    |Data Structure        | How do we represent this data?
|                    |Algorithm             | How do we convert our input to output? 
|Code with Intent    |Code                  | How do we implement this algo with code?

### Small Code Challenge Problems
  - 20 - 45 minutes
  - typical solutions: 10 - 40 lines of code
  - **used extensively in interviews for a reason**
    - mastery of a language
    - logic / reasoning
    - communications
  - not a skill that you "acquire and file away", but needs a lot of practice

### Understand the Problem
  - sometime requirements are explicit
    - take notes
    - the "odd words" problem
  - **sometimes requirements are not so explicit and need to be modeled**
    - These problems are often left somewhat ambiguous in interviews **intentionally**. Ask questions to clarify.
    - examples need to be described in computational words
    - "diamond of stars"
  - implicit knowledge needs to be captured
    - convert to explicit rules, in computational language
    - "what century is that"
  - identifying and defining important terms and concepts
    - "queen attack"
    - same row; same column; especially same diagonal
  - **ask questions to verify your understanding!**

### Examples / Test Cases
  - Input / Output
  - Test cases serve two purposes:
    - help you understand the problem
    - allow you to verify your solution
  - **Happy paths**
    - combination of requirements; the "obvious" result
  - **Edge cases**
    - focus on input
    - emptiness (nil/null, "", [], {})
    - boundary conditions
    - repetition / duplication
    - upper case / lower case
    - data types
  - **Failures / Bad Input**
    - raise exceptions / report errors
    - return a special value (nil/null, 0, "", [], etc.)
  - **ask questions to verify your understanding!**

### Data Structure
  - input data
  - rules/requirements as data
    - hash/object

  - string, array, hash/object, number
    - string
      - concact, strip, reverse, etc.
      - Regular Expression! split, replace, match...
    - array
      - need to walk through it (iteration)
      - index
      - abstractions!!
        - map
        - reduce
        - select/filter
        - all
        - ...
    - hash/object
      - lookup table / dictionary
      - partition data for more efficient access downstream
      - digest
    - number
      - math operations
      - number as string may have advantage over numbers
  - compound data structures
    - array of arrays
    - hash with array/object values, etc.

### Algorithm
  - algorithms have to be described in the language of chosen data structure!
    - "then solve it" doesn't count
  - have to be **really fluent** with
    - String / Regex
    - Array
      - Ruby: Enumerable
      - JavaScript: Higher-Order Functions
    - Hash / Object
      - Creation (default values)
      - Access (default values)
      - Iteration
  - verify your algorithm with your examples / test cases

### Interview Tips
- This is about _derisking_.
- We check our building blocks and run code often to derisk our problem solving process.
