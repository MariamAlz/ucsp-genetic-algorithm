# UCSP

> **Course:** ENGR 605 Systems Optimization · MSc, Khalifa University

A genetic algorithm approach to the University Class Scheduling Problem (UCSP), tested on small and medium generated databases. The GA assigns each class a timeslot, a classroom and an instructor, and evolves schedules until it finds one with no conflicts.

## Approach

- **Data.** Classrooms, timeslots, instructors, courses (with maximum enrollment and eligible instructors) and student groups are stored in SQLite and loaded at start-up.
- **Encoding.** A schedule is a list of classes, one per (student group, course) pair. Each class gets a random timeslot, classroom, and one of the course's eligible instructors.
- **Constraints counted as conflicts:**
  - the classroom's seating capacity is smaller than the course's maximum number of students;
  - two classes share the same classroom in the same timeslot;
  - two classes share the same instructor in the same timeslot.
- **Fitness.** `1 / (number_of_conflicts + 1)`, so a conflict-free schedule has fitness 1.0.
- **GA settings.** Population 20, 1 elite schedule kept each generation, tournament selection (size 3), uniform crossover (each class taken from either parent with probability 0.5), and mutation rate 0.1 (a class is replaced by a randomly generated one).
- **Stopping rule.** Runs until the best schedule reaches fitness 1.0. Every generation is printed as a table, along with the current best schedule.

## Test databases

| Database | Script | Classrooms | Timeslots | Instructors | Courses | Student groups |
|---|---|---:|---:|---:|---:|---:|
| Small | `create_db_small.py` → `class_schedule_2.db` | 4 | 4 | 5 | 10 | 4 |
| Medium | `create_db_medium.py` → `class_schedule_medd.db` | 20 | 8 | 14 | 25 | 5 |

## Project structure

```
UCSP/
├── GA_UCSP.py            # Genetic algorithm, data model and table output
├── create_db_small.py    # Builds the small SQLite database
└── create_db_medium.py   # Builds the medium SQLite database
```

## Getting started

Requirements: Python 3 and `prettytable` (`sqlite3` and `random` are in the standard library).

```bash
pip install prettytable

# Build the small database and run the GA on it
python create_db_small.py
python GA_UCSP.py
```

To use the medium database, run `python create_db_medium.py` and change the file name in `DBMgr.__init__` in `GA_UCSP.py` from `class_schedule_2.db` to `class_schedule_medd.db`.

The database scripts create tables with `CREATE TABLE`, so delete an existing `.db` file before running a script again.

## Tech

Python · SQLite · PrettyTable
