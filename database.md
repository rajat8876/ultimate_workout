# Exercise Tracking App - Database Setup Checklist

## Database Initialization

- [x] Install SQLite3 or better-sqlite3 package
- [x] Create database file in data directory
- [x] Set PRAGMA foreign_keys = ON
- [x] Set PRAGMA journal_mode = WAL
- [x] Create database connection wrapper

## Table Creation

### Users Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add username field (TEXT, NOT NULL, UNIQUE)
- [x] Add password_hash field (TEXT, NOT NULL)
- [x] Add created_at field (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- [x] Add optional email, first_name, last_name fields
- [x] Create index on username for faster lookups

### MuscleGroups Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add name field (TEXT, NOT NULL, UNIQUE)
- [x] Add description field (TEXT)

### Muscles Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add name field (TEXT, NOT NULL, UNIQUE)
- [x] Add muscle_group_id field (INTEGER, NOT NULL)
- [x] Add FOREIGN KEY constraint to MuscleGroups(id)
- [x] Add ON DELETE CASCADE for automatic deletion
- [x] Create index on muscle_group_id for faster joins

### Exercises Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add name field (TEXT, NOT NULL, UNIQUE)
- [x] Add category field (TEXT, NOT NULL)
- [x] Add description field (TEXT)
- [x] Add difficulty_level field (TEXT)
- [x] Add equipment_needed field (TEXT)
- [x] Create index on category for filtering

### ExerciseMuscles Junction Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add exercise_id field (INTEGER, NOT NULL)
- [x] Add muscle_id field (INTEGER, NOT NULL)
- [x] Add is_primary field (BOOLEAN, NOT NULL, DEFAULT 0)
- [x] Add FOREIGN KEY constraint to Exercises(id) with CASCADE
- [x] Add FOREIGN KEY constraint to Muscles(id) with CASCADE
- [x] Add UNIQUE constraint on exercise_id + muscle_id combination
- [x] Create index on exercise_id for faster queries
- [x] Create index on muscle_id for faster queries

### Workouts Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add user_id field (INTEGER, NOT NULL)
- [x] Add date field (DATE, NOT NULL)
- [x] Add notes field (TEXT)
- [x] Add created_at field (DATETIME, DEFAULT CURRENT_TIMESTAMP)
- [x] Add FOREIGN KEY constraint to Users(id) with CASCADE
- [x] Create index on user_id for filtering
- [x] Create index on date for filtering
- [x] Create compound index on user_id + date for common queries

### WorkoutExercises Junction Table
- [x] Create with id as INTEGER PRIMARY KEY AUTOINCREMENT
- [x] Add workout_id field (INTEGER, NOT NULL)
- [x] Add exercise_id field (INTEGER, NOT NULL)
- [x] Add sets field (INTEGER)
- [x] Add reps field (INTEGER)
- [x] Add weight field (REAL)
- [x] Add duration field (INTEGER) - in seconds
- [x] Add distance field (REAL)
- [x] Add notes field (TEXT)
- [x] Add FOREIGN KEY constraint to Workouts(id) with CASCADE
- [x] Add FOREIGN KEY constraint to Exercises(id) with RESTRICT
- [x] Create index on workout_id for faster queries

## Initial Data Population

### MuscleGroups Data
- [x] Insert Chest group with description
- [x] Insert Back group with description
- [x] Insert Arms group with description
- [x] Insert Shoulders group with description
- [x] Insert Legs group with description
- [x] Insert Core group with description

### Muscles Data
- [x] Insert 3-4 chest muscles (e.g., Pectoralis Major, Pectoralis Minor)
- [x] Insert 4-5 back muscles (e.g., Latissimus Dorsi, Trapezius, Rhomboids)
- [x] Insert 5-6 arm muscles (e.g., Biceps, Triceps, Forearm muscles)
- [x] Insert 3-4 shoulder muscles (e.g., Deltoids, Rotator Cuff)
- [x] Insert 5-6 leg muscles (e.g., Quadriceps, Hamstrings, Glutes, Calves)
- [x] Insert 3-4 core muscles (e.g., Rectus Abdominis, Obliques)

### Exercises Data
- [ ] Insert 10-15 strength exercises with descriptions
  - [ ] Include compound movements (Bench Press, Squat, Deadlift)
  - [ ] Include isolation exercises (Bicep Curl, Tricep Extension)
  - [ ] Include bodyweight exercises (Push-up, Pull-up)
- [ ] Insert 5-10 cardio exercises (Running, Cycling, Swimming)
- [ ] Insert 5-10 flexibility exercises (Various stretches)
- [ ] Include variety of difficulty levels (Beginner, Intermediate, Advanced)
- [ ] Include equipment needs information

### ExerciseMuscles Associations
- [x] Link each chest exercise to appropriate chest muscles
- [x] Link each back exercise to appropriate back muscles
- [x] Link each arm exercise to appropriate arm muscles
- [x] Link each shoulder exercise to appropriate shoulder muscles
- [x] Link each leg exercise to appropriate leg muscles
- [x] Link each core exercise to appropriate core muscles
- [x] Mark primary target muscles with is_primary = 1
- [x] Include secondary muscle targets with is_primary = 0
- [x] Ensure compound exercises target multiple muscle groups

## Verification
- [ ] Verify all tables created with correct structure
- [ ] Verify foreign key constraints are working
- [ ] Verify indices are properly created
- [ ] Verify initial data is properly inserted
- [ ] Test basic queries for performance
- [ ] Ensure script is idempotent (can run multiple times without errors)