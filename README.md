# Code exercise

## Edits
* v2 of the problem changes task IDs to be represented as a UUID/GUID;


## Problem
Suppose you have an application where users create lists of tasks to be completed, where all tasks are ordered.

There can be 0 or more tasks in a list, and tasks can be nested to an arbitrary depth. For example:
* Go shopping
    - Pickup milk
    - Pickup bread
* Gym
* Cook dinner

Since the database backing the application stores tasks independently each task must be stored along
 with the IDs of its previous, next and parent tasks (where necessary).
 
Task IDs are represented as UUIDs/GUIDs (or their string representation for the purpose of this exercise).
IDs are not necessarily ordered by creation time - i.e. tasks can be created out of order.
  
The previous example could therefore be stored as:

[**Code block 1**]
```json
[
  {
    "ID": "2e0dc336-35db-4815-9f7c-5bceb1475806",
    "description": "Go shopping",
    "nextID": "26c166e0-0009-4a8c-bb56-d0bf43bef972"
  },
  {
    "ID": "26c166e0-0009-4a8c-bb56-d0bf43bef972",
    "description": "Gym",
    "previousID": "2e0dc336-35db-4815-9f7c-5bceb1475806",
    "nextID": "cca0e3a1-2de1-4b20-923c-ad53a8dcb298"
  },
  {
    "ID": "848fe9e5-b545-4963-8f45-335c24f6b22d",
    "description": "Pickup bread",
    "previousID": "74af3911-c4d0-492e-b1d2-aff36d2d785f",
    "parentID": "2e0dc336-35db-4815-9f7c-5bceb1475806"
  },
  {
    "ID": "74af3911-c4d0-492e-b1d2-aff36d2d785f",
    "description": "Pickup milk",
    "nextID": "848fe9e5-b545-4963-8f45-335c24f6b22d",
    "parentID": "2e0dc336-35db-4815-9f7c-5bceb1475806"
  },
  {
    "ID": "cca0e3a1-2de1-4b20-923c-ad53a8dcb298",
    "description": "Cook dinner",
    "previousID": "26c166e0-0009-4a8c-bb56-d0bf43bef972"
  }
]
```

This format allows the application to fetch tasks from the database and re-order them to be correctly displayed to the user.
To correctly be displayed to the user the tasks must be ordered into a hierarchical data structure.
Using the previous example again the ordered tasks would be represented by the following JSON:

[**Code block 2**]
```json
[
  {
    "task": {
      "ID": "2e0dc336-35db-4815-9f7c-5bceb1475806",
      "description": "Go shopping",
      "nextID": "26c166e0-0009-4a8c-bb56-d0bf43bef972"
    },
    "subTasks": [
      {
        "task": {
          "ID": "74af3911-c4d0-492e-b1d2-aff36d2d785f",
          "description": "Pickup milk",
          "nextID": "848fe9e5-b545-4963-8f45-335c24f6b22d",
          "parentID": "2e0dc336-35db-4815-9f7c-5bceb1475806"
        },
        "subTasks": []
      },
      {
        "task": {
          "ID": "848fe9e5-b545-4963-8f45-335c24f6b22d",
          "description": "Pickup bread",
          "previousID": "74af3911-c4d0-492e-b1d2-aff36d2d785f",
          "parentID": "2e0dc336-35db-4815-9f7c-5bceb1475806"
        },
        "subTasks": []
      }
    ]
  },
  {
    "task": {
      "ID": "26c166e0-0009-4a8c-bb56-d0bf43bef972",
      "description": "Gym",
      "previousID": "2e0dc336-35db-4815-9f7c-5bceb1475806",
      "nextID": "cca0e3a1-2de1-4b20-923c-ad53a8dcb298"
    },
    "subTasks": []
  },
  {
    "task": {
      "ID": "cca0e3a1-2de1-4b20-923c-ad53a8dcb298",
      "description": "Cook dinner",
      "previousID": "26c166e0-0009-4a8c-bb56-d0bf43bef972"
    },
    "subTasks": []
  }
]
```

## Task
Write a program that reads in a JSON blob representing tasks as they are stored in the database 
and writes the JSON representation of the ordered tasks to another JSON file.

This program should be runnable from the command line (after compilation if necessary) and take two arguments:

1. File path of input JSON (a flat list of tasks as shown above in `Code block 1`); and
2. File path where the structured output will be written to (as shown above in `Code block 2`).

For example:
```bash
$ node solution.js ./examples/example_1_input.json ./answer.json
``` 
 
### Deliverables
1. Solution source code;
2. Instructions (`README.md` preferred) for how to compile and run your solution; and
3. Any artifacts such as test cases or extra examples deemed relevant.

These artifacts should be submitted either via:

1. Email of a zip file containing all necessary code;
2. Link to shared folder in cloud storage service (Dropbox / Google Drive / etc.); or
3. Link to github/gitlab repo (if private and on github please invite `loughlin@crewmanfour.com`).

### Constraints

#### Error checking
The solution can assume the input JSON is well-formed and that all task references are valid (e.g. there are no duplicate IDs).

#### Programming language
Suggested programming languages include:
* JavaScript / TypeScript;
* Python (2/3);
* F#;
* Scala;
* Clojure;
* Java;
* Kotlin;
* Rust;
* Swift;
* OCaml; or
* Haskell.

**NOTE** For any compiled language please ensure the README includes sufficiently detailed build (from source code) and run instructions. 
Using a package manager is okay. 
Any solution which includes using binary blobs/jars/etc will not be tested.

#### Environment
Your solution will be tested within a Linux or MacOS environment, so please ensure some degree of cross platform compatibility if working outside of these OSs.
