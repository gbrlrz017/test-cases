test-cases
==========

**What is a “test case”?**

The IEEE defines a test case as “Documentation specifying inputs, predicted results, and a set of execution conditions for a test item.” And the computer scientist and author, Glenn Meyers, says a test case is “a process of executing a program with the intent of finding an error.” A test case essentially checks that a program works as it should.

Finding and fixing _bugs_ is an important part of the programming process. A bug you didn't know existed can come back to haunt you later. For example, consider NASA's Mars Climate Orbiter which, in 1998, was tasked with looking for water on Mars (among other things). Apparently some of its programmers had been working with SI units while others had been working in English units. The result? The thrusters were 4.45 times more powerful than they should have been and this $327.6 million project was subsequently lost in space, most likely in pieces. 

No program is ever perfect (not even those that NASA writes). But a good programmer makes a habit out of making sure her or his program is as bugless as possible.

The aim of test case writing is to separate a program into small, important units that produce a quantifiable result. For example, the quality of our shell can be measure by test cases that examine functionality and usability. Test case writing should make knowing how well a program works as easy and efficient as possible. Here are some questions to test cases should address: 
1. What scenarios have been tested and which still need to be tested.
2. Which parts of the program are stable? What parts are unstable?
3. Where does the program need more work?
4. Have enough possibilities and input combinations been tested?
5. Is there appropriate error checking and error messages? (negative test cases)
6. Is the User Interface (UI) similar enough to Bash's UI? (the command prompt for example)
7. Are the required features present and working? (the assignment specifications)
8. Have the possible scenarios been documented? Are these scenarios in a working state?
9. Are the tests good enough? Are the tests finding issues in the program? 
10. What is the overall quality of the segment of the shell you just implemented? The entire shell?


**Types of Test cases:** 

- Unit Test Case: Every line of code is tested thoroughly. This is done at the development stage. It should go into the intricate details of the program to ensure that these are working correctly.
- Functional Test Case: The program is tested at the functional level. This checks if a given function or segment of code does the job it is designed to do. 
- System Test Case: The complete, integrated program is tested to check its compliance with predefined requirements. The purpose is to look for inconsistencies when the units are integrated together. 
- User Acceptance Test Case: The program is tested at the operational level. Testing here is based on scenarios the shell will encounter during its use. 

**Test Design Strategies:**

These following are useful strategies for test case writing.

- Branch Coverage Test Design: Testing is devised with decision making points in the source code in mind. A decision point is a place that may include a conditional `if` or `else` statement, a `while` loop, or some function or chunk of code where a the program changes course (_branches off_). This method is effective when all the important decision point combinations are tested.

  - Possible decision points and branching off may occur when your program decides how to handle the `child` and `parent` processes when the connectors `;`, `||`, or `&&` are used. `ls || pwd` should only execute one command or the other (the first that doesn't fail) but not both, while `ls && pwd` executes both commands as long as the first one doesn't fail, and `ls ; pwd` attempts to execute both commands. 

- Equivalence Class Partitioning: Think of an equivalence class as a set of test values that are similar enough that it is sufficient to test one or two and determine that the rest work, too. This style of testing eliminates redundancy, allowing the tester to move on to another part of testing.

  - For example, if `ls` by itself works then `pwd` and most other _bash_ commands should work by theirselves should work. You could therefore move on to an equivalence class that includes _flags_ and _arguments_ for a command, such as `cmd [flags] [args]`. 

- Boundary Value Test Design: This strategy is used when the tester is aware of bugs in the program for certain test values. The boundary around those buggy test values is tested in order to determine the extent of the problem.

  - Consider a bug with the command `ls;pwd` in your shell. It is likely an issue with your **_parsing_** method or your **_forking_** method and **_child process_** handling. Boundary test values will therefore include 
    - different spacing between commands and connectors: `ls ; pwd`, `ls; pwd`, `ls ;pwd`, and `ls    ;     pwd`. It may not seem like it, but these are likely to cause trouble. 
    - varying amounts of chained commands: `ls; pwd; ls -a dir; ….` where each chained command should be executed by a separate process. 
    - different connectors: `ls|| pwd ` or `ls -l . && pwd` 

- Function: Test each function and feature in isolation.

- Specification Based: Test functionality against existing specifications. For example, ask “What does the Bash do?” Check what _Bash_ does then run that same test on your shell.

- Risk Based: In this approach, the tester attempts to break the program in order to get an idea of what still needs work.
  
  - For example, say I am implementing `ls` (second homework assignment). I know `ls -a -l -R` works fine, but do `ls -alR` or `ls file1 -la file2 file3 -R file4` work? The latter two are more likely to find an issue. 

- Negative Test Cases: Test whether the program _does not_ do things that it _shouldn't_. The tester looks for any funny behavior in the program. These tests check whether you have to appropriate error checking and error messages.
  
  - For one of my homework assignments, I was tasked with implementing the `cd` command. Everything worked great when I had the usual `cd dir`, but when I had `cd` by itself or `cd dir1 dir2 ...` my entire shell crashed. It turns out I accidentally called `exit(1)` in my code if the user gave anything other than one argument for `cd`. 
  - Appropriate error checking for system calls and `perror` messages are important: Make a test case in which `exec*()` , `readir()`, `stat()`, `dup()` and other functions/system calls will fail.

Combining two or more of these approaches  at a time is likely to yield good results.


**How to Write Test Cases:** 

The main idea behind writing test cases is to be as effective and to the point as possible. There is no need to get overly complex as long as the essential cases are tested adequately.

- Procedure: get background, list of features to be implemented, how will users interact with program.... (use some of what have already here)
  - Get a firm understanding of the program:
get all the background you need.. sys calls etc. (insert old words here)
  - requirement specs
  - available documentation (see Bash documentation)
  - trying out Bash itself

- Decide on a test case writing strategy: 
  - Different parts may require different strategies based on different functionalities. Know which and do. Make sure the entire flow of you program is covered by your test cases.

- Decide on a test case template for documentation (IEEE 829 Standard):
  - This should be a minimally complicated part of the test case writing process. It is an important part nonetheless. Done correctly, documentation of test cases will increase efficiency by allowing focus on actually coming up with test scenarios and by being a quick reference when needed later.
	(list column titles of table)

![alt tag](https://cloud.githubusercontent.com/assets/9201839/5465285/62b87146-8549-11e4-9cfd-293e35ce1dc8.png)


Here is an example of how to fill in the test table. The program here implements a basic version of the _Bash command shell_ using `execvp`, `fork`, and `waitpid`. This was an  [assignment](https://github.com/mikeizbicki/ucr-cs100/blob/cs100-2014fall/assignments/hw/hw0-rshell/README.md).

