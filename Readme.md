# First Principles in Action: Breaking Problems Down at 42

This repository contains the presentation and facilitator materials for a 120-minute guided peer-to-peer workshop designed for students at 42.

## How to View the Presentation
Simply open the `index.html` file in any modern web browser. If you host this repository via **GitHub Pages**, the presentation will automatically serve as your website.

---

## The 42 Backup Challenges
If a pair of students during the workshop says, *"We don't have a problem to work on right now,"* hand them one of the following challenges based on their current Cursus level. 

These represent classic 42 "traps" where students attempt to solve complex problems by analogy, and outline how to break them down using First Principles.

### Level 1: Algorithmic Optimization (`push_swap`)
**The Original Problem (The Trap)**
"I have to sort 500 random numbers using only two stacks and a limited set of swap operations. My code is either over the operation limit or an unreadable mess. I feel like I need to invent a genius new sorting algorithm from scratch."

* **The Hunt (Hidden Assumptions):** "I need to track the exact mathematical path of all 500 numbers simultaneously." / "Stacks are rigid towers where I have to dig to the bottom."
* **The Strip (Basic Truths):** Because we have `ra` and `rra`, these aren't actually stacks; they are continuous rings. A computer can perfectly calculate the "cost" of moving any number to its correct neighbor before a single move is made.
* **The Rebuild Formula:** "Because I know *[the stacks act like rotatable rings]* and *[I can calculate the exact operation cost for every number]* are definitely true, my actual next step is to *[write a function that calculates the cheapest single number to move right now]*, rather than worrying about *[inventing a 500-number global sorting algorithm]*."
* **Micro-Action:** Write a helper function `calculate_cost(int num_pos, int target_pos)` that returns the `ra`/`rra` moves needed to align two spots, without doing any actual swapping.

### Level 2: Concurrency & Deadlocks (`philosophers`)
**The Original Problem (The Trap)**
"My philosophers are dying. If I slow down the threads, they starve. If I speed them up, they data-race. If they all grab forks at the same time, the program deadlocks and freezes forever. I am drowning in mutexes and thread timing."

* **The Hunt (Hidden Assumptions):** "I need a complex global 'manager' thread." / "Threads execute in a perfectly predictable order." / "Deadlocks are random bugs."
* **The Strip (Basic Truths):** Threads are completely chaotic. A deadlock is a strict mathematical state that *only* occurs if there is a circular wait. Forks are just boolean flags protected by a mutex.
* **The Rebuild Formula:** "Because I know *[threads are completely unpredictable]* and *[a deadlock strictly requires a circular wait]* are definitely true, my actual next step is to *[break the circular wait by forcing an asymmetry - e.g., evens grab right/left, odds grab left/right]*, rather than worrying about *[trying to perfectly time all the threads]*."
* **Micro-Action:** Create a simulation with just 2 philosophers and 2 forks with the asymmetric grabbing rule. Prove it can run indefinitely without deadlocking before adding the death timer.

### Level 3: Architecture & Parsing (`minishell`)
**The Original Problem (The Trap)**
"I'm trying to parse `echo "hello | world" | wc -l > outfile`. My code is exploding because I'm trying to handle quotes, pipes, expansions, and redirections all inside one massive `while` loop that splits the string."

* **The Hunt (Hidden Assumptions):** "I must parse, expand, and execute the command in a single left-to-right pass." / "A pipe inside quotes is the same character as a pipe outside."
* **The Strip (Basic Truths):** Parsing and Execution are two completely separate physical phases. Quotes do not exist to be printed; they simply toggle a boolean state (`in_quotes = true/false`) to tell the parser to ignore special characters.
* **The Rebuild Formula:** "Because I know *[quotes just toggle a boolean state]* and *[Parsing is completely separate from Execution]* are definitely true, my actual next step is to *[build a tokenizer that strictly labels chunks of the string (WORD, PIPE, REDIRECT)]*, rather than worrying about *[executing processes at the same time]*."
* **Micro-Action:** Build a visual `print_tokens()` function. Input `ls -l | grep "foo | bar"`, and make it strictly output `[WORD: ls]`, `[PIPE: |]`, etc., before ever writing a `fork` or `execve`.