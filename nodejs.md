# Exercise Tracking App - Node.js Setup & API Implementation Checklist

## Project Initialization and Configuration

### Project Setup
- [x] Create project directory
- [x] Initialize Node.js project
  - [x] Run `npm init -y` to create package.json
  - [x] Set "type": "module" for ES modules support (if desired)
  - [x] Configure package.json scripts:
    - [x] "start": "node src/server.js"
    - [x] "dev": "nodemon src/server.js"
    - [x] "test": "jest"

### Dependencies Installation
- [x] Install core dependencies
  - [x] Express: `npm install express`
  - [x] SQLite: `npm install better-sqlite3`
  - [x] Body Parser: `npm install body-parser`
  - [x] CORS: `npm install cors`
  - [x] Morgan (logging): `npm install morgan`
  - [x] Dotenv: `npm install dotenv`
- [x] Install development dependencies
  - [x] Nodemon: `npm install nodemon --save-dev`
  - [x] Jest: `npm install jest --save-dev`
  - [x] Supertest: `npm install supertest --save-dev`

### Project Structure Setup
- [x] Create source directory structure
  - [x] Create `src` main directory
  - [x] Create `src/routes` for API route definitions
  - [x] Create `src/controllers` for request handlers
  - [x] Create `src/services` for business logic
  - [x] Create `src/models` for database models
  - [x] Create `src/db` for database configuration
  - [x] Create `src/middleware` for custom middleware
  - [x] Create `src/utils` for helper functions
  - [x] Create `src/config` for configuration files
- [x] Create test directory structure
  - [x] Create `tests` main directory
  - [x] Create `tests/unit` for unit tests
  - [x] Create `tests/integration` for API tests
  - [x] Create `tests/fixtures` for test data

### Environment Configuration
- [x] Create `.env` file for development
  - [x] Add `PORT=3000` (or preferred port)
  - [x] Add `DB_PATH=./data/exercise_tracker.db`
  - [x] Add `NODE_ENV=development`
- [x] Create `.env.example` as template
  - [x] Include all keys without sensitive values
- [x] Create `.gitignore` file
  - [x] Include `.env`, `node_modules`, and other files to ignore

## Database Integration

### Database Connection Setup
- [x] Create database connection module (`src/db/database.js`)
  - [x] Import better-sqlite3
  - [x] Create connection factory function
  - [x] Set pragma settings (foreign_keys, journal_mode)
  - [x] Add proper error handling and logging
  - [x] Implement connection pooling (if needed)
  - [x] Export database instance

### Model Implementations

#### Base Model
- [x] Create base model with common CRUD operations
  - [x] Implement `findAll()` method
  - [x] Implement `findById(id)` method
  - [x] Implement `create(data)` method
  - [x] Implement `update(id, data)` method
  - [x] Implement `delete(id)` method
  - [x] Add transaction support for complex operations

#### User Model
- [x] Create User model extending base model
  - [x] Implement `findByUsername(username)` method
  - [x] Add data validation for username uniqueness
  - [x] Implement password hashing (for future authentication)
  - [x] Add method to retrieve user's workouts

#### Muscle Group Model
- [x] Create MuscleGroup model extending base model
  - [x] Implement `findWithMuscles()` method to get groups with muscles
  - [x] Add data validation for name uniqueness
  - [x] Add method to retrieve muscles in a group

#### Muscle Model
- [x] Create Muscle model extending base model
  - [x] Implement `findByMuscleGroup(muscleGroupId)` method
  - [x] Add method to retrieve exercises targeting this muscle
  - [x] Add validation for muscle group existence

#### Exercise Model
- [x] Create Exercise model extending base model
  - [x] Implement `findByCategory(category)` method
  - [x] Implement `findByMuscle(muscleId)` method
  - [x] Implement `findByMuscleGroup(muscleGroupId)` method
  - [x] Create method to manage exercise-muscle associations
  - [x] Add transaction support for exercise with muscles

#### Workout Model
- [x] Create Workout model extending base model
  - [x] Implement `findByUser(userId)` method
  - [x] Implement `findByDateRange(startDate, endDate)` method
  - [x] Create method to manage workout-exercise associations
  - [x] Add validation for user existence
  - [x] Add transaction support for workout with exercises

#### WorkoutExercise Model
- [x] Create WorkoutExercise model extending base model
  - [x] Implement `findByWorkout(workoutId)` method
  - [x] Implement `findByExercise(exerciseId)` method
  - [x] Add validation for workout and exercise existence

## Server Setup

### Express Application Setup
- [x] Create main server file (`src/server.js`)
  - [x] Import required dependencies
  - [x] Initialize Express application
  - [x] Configure middleware (json, urlencoded, cors)
  - [x] Set up logging with Morgan
  - [x] Import and mount all routes
  - [x] Add error handling middleware
  - [x] Configure server to listen on specified port

### API Utilities and Middleware
- [x] Create response formatter middleware
  - [x] Define standard format: `{ success, data, error, meta }`
  - [x] Add timestamps and request IDs to responses
- [x] Create error handling middleware
  - [x] Define custom error classes (NotFoundError, ValidationError, etc.)
  - [x] Map errors to appropriate HTTP status codes
  - [x] Format error responses consistently
- [x] Create validation middleware
  - [x] Implement request validation for different endpoints
  - [x] Return detailed validation errors
- [x] Implement pagination middleware
  - [x] Support `page` and `limit` parameters
  - [x] Add pagination metadata to responses

## API Routes and Controllers Implementation

### User API Endpoints

#### User Routes
- [ ] Create user routes file (`src/routes/users.js`)
  - [ ] Define route for GET /api/users
  - [ ] Define route for GET /api/users/:id
  - [ ] Define route for POST /api/users
  - [ ] Define route for PUT /api/users/:id
  - [ ] Define route for DELETE /api/users/:id
  - [ ] Define route for GET /api/users/:id/workouts
  - [ ] Export router

#### User Controller
- [ ] Create user controller (`src/controllers/userController.js`)
  - [ ] Implement `getUsers(req, res, next)` function
    - [ ] Support filtering by username
    - [ ] Implement pagination
    - [ ] Return total count in response
  - [ ] Implement `getUserById(req, res, next)` function
    - [ ] Handle 404 for non-existent users
    - [ ] Support including workouts with `?include=workouts`
  - [ ] Implement `createUser(req, res, next)` function
    - [ ] Validate required fields
    - [ ] Check for username uniqueness
    - [ ] Return created user with ID and timestamp
  - [ ] Implement `updateUser(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Prevent changing to existing username
    - [ ] Return updated user
  - [ ] Implement `deleteUser(req, res, next)` function
    - [ ] Check for associated workouts
    - [ ] Return success/failure status
  - [ ] Implement `getUserWorkouts(req, res, next)` function
    - [ ] Validate user exists
    - [ ] Return paginated workouts for user

### Muscle Group API Endpoints

#### Muscle Group Routes
- [ ] Create muscle group routes file (`src/routes/muscleGroups.js`)
  - [ ] Define route for GET /api/muscle-groups
  - [ ] Define route for GET /api/muscle-groups/:id
  - [ ] Define route for POST /api/muscle-groups
  - [ ] Define route for PUT /api/muscle-groups/:id
  - [ ] Define route for DELETE /api/muscle-groups/:id
  - [ ] Define route for GET /api/muscle-groups/:id/muscles
  - [ ] Export router

#### Muscle Group Controller
- [ ] Create muscle group controller (`src/controllers/muscleGroupController.js`)
  - [ ] Implement `getMuscleGroups(req, res, next)` function
    - [ ] Support including muscles with `?include=muscles`
  - [ ] Implement `getMuscleGroupById(req, res, next)` function
    - [ ] Handle 404 for non-existent muscle groups
  - [ ] Implement `createMuscleGroup(req, res, next)` function
    - [ ] Validate required fields (name, unique)
    - [ ] Return created muscle group with ID
  - [ ] Implement `updateMuscleGroup(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Prevent changing to existing name
    - [ ] Return updated muscle group
  - [ ] Implement `deleteMuscleGroup(req, res, next)` function
    - [ ] Check for associated muscles
    - [ ] Return success/failure status
  - [ ] Implement `getMuscleGroupMuscles(req, res, next)` function
    - [ ] Validate muscle group exists
    - [ ] Support including exercises with `?include=exercises`

### Muscle API Endpoints

#### Muscle Routes
- [ ] Create muscle routes file (`src/routes/muscles.js`)
  - [ ] Define route for GET /api/muscles
  - [ ] Define route for GET /api/muscles/:id
  - [ ] Define route for POST /api/muscles
  - [ ] Define route for PUT /api/muscles/:id
  - [ ] Define route for DELETE /api/muscles/:id
  - [ ] Define route for GET /api/muscles/:id/exercises
  - [ ] Export router

#### Muscle Controller
- [ ] Create muscle controller (`src/controllers/muscleController.js`)
  - [ ] Implement `getMuscles(req, res, next)` function
    - [ ] Support filtering by muscleGroupId
    - [ ] Support including exercises with `?include=exercises`
  - [ ] Implement `getMuscleById(req, res, next)` function
    - [ ] Handle 404 for non-existent muscles
    - [ ] Include muscle group information
  - [ ] Implement `createMuscle(req, res, next)` function
    - [ ] Validate required fields (name, unique, muscleGroupId)
    - [ ] Verify muscleGroupId exists
    - [ ] Return created muscle with ID
  - [ ] Implement `updateMuscle(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Verify muscleGroupId exists if changing
    - [ ] Return updated muscle
  - [ ] Implement `deleteMuscle(req, res, next)` function
    - [ ] Check for associated exercises
    - [ ] Return success/failure status
  - [ ] Implement `getMuscleExercises(req, res, next)` function
    - [ ] Validate muscle exists
    - [ ] Support filtering by isPrimary: `?primary=true`

### Exercise API Endpoints

#### Exercise Routes
- [ ] Create exercise routes file (`src/routes/exercises.js`)
  - [ ] Define route for GET /api/exercises
  - [ ] Define route for GET /api/exercises/:id
  - [ ] Define route for POST /api/exercises
  - [ ] Define route for PUT /api/exercises/:id
  - [ ] Define route for DELETE /api/exercises/:id
  - [ ] Define route for GET /api/exercises/:id/muscles
  - [ ] Export router

#### Exercise Controller
- [ ] Create exercise controller (`src/controllers/exerciseController.js`)
  - [ ] Implement `getExercises(req, res, next)` function
    - [ ] Support multiple filters (category, muscleId, muscleGroupId)
    - [ ] Implement pagination
    - [ ] Return total count in response
  - [ ] Implement `getExerciseById(req, res, next)` function
    - [ ] Handle 404 for non-existent exercises
    - [ ] Include primary and secondary muscles
  - [ ] Implement `createExercise(req, res, next)` function
    - [ ] Validate required fields (name, unique, category)
    - [ ] Process associated muscles array
    - [ ] Use transaction for exercise and muscle associations
    - [ ] Return created exercise with muscle data
  - [ ] Implement `updateExercise(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Update exercise and associated muscles
    - [ ] Use transaction for consistency
    - [ ] Return updated exercise with muscle data
  - [ ] Implement `deleteExercise(req, res, next)` function
    - [ ] Check for associated workouts
    - [ ] Delete entries in ExerciseMuscles
    - [ ] Return success/failure status
  - [ ] Implement `getExerciseMuscles(req, res, next)` function
    - [ ] Validate exercise exists
    - [ ] Separate by primary and secondary muscles

### Workout API Endpoints

#### Workout Routes
- [ ] Create workout routes file (`src/routes/workouts.js`)
  - [ ] Define route for GET /api/workouts
  - [ ] Define route for GET /api/workouts/:id
  - [ ] Define route for POST /api/workouts
  - [ ] Define route for PUT /api/workouts/:id
  - [ ] Define route for DELETE /api/workouts/:id
  - [ ] Define route for GET /api/workouts/:id/exercises
  - [ ] Export router

#### Workout Controller
- [ ] Create workout controller (`src/controllers/workoutController.js`)
  - [ ] Implement `getWorkouts(req, res, next)` function
    - [ ] Support filters (userId, startDate, endDate)
    - [ ] Implement pagination
    - [ ] Return total count in response
  - [ ] Implement `getWorkoutById(req, res, next)` function
    - [ ] Handle 404 for non-existent workouts
    - [ ] Include user details
    - [ ] Support including exercises with `?include=exercises`
  - [ ] Implement `createWorkout(req, res, next)` function
    - [ ] Validate required fields (userId, date)
    - [ ] Process associated exercises array
    - [ ] Use transaction to insert workout and exercise associations
    - [ ] Return created workout with exercise data
  - [ ] Implement `updateWorkout(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Update workout and associated exercises
    - [ ] Use transaction for consistency
    - [ ] Return updated workout with exercise data
  - [ ] Implement `deleteWorkout(req, res, next)` function
    - [ ] Delete associated entries in WorkoutExercises
    - [ ] Return success/failure status
  - [ ] Implement `getWorkoutExercises(req, res, next)` function
    - [ ] Validate workout exists
    - [ ] Include sets, reps, weight, duration, distance data

### Workout Exercise API Endpoints

#### Workout Exercise Routes
- [ ] Create workout exercise routes file (`src/routes/workoutExercises.js`)
  - [ ] Define route for GET /api/workout-exercises
  - [ ] Define route for GET /api/workout-exercises/:id
  - [ ] Define route for POST /api/workout-exercises
  - [ ] Define route for PUT /api/workout-exercises/:id
  - [ ] Define route for DELETE /api/workout-exercises/:id
  - [ ] Export router

#### Workout Exercise Controller
- [ ] Create workout exercise controller (`src/controllers/workoutExerciseController.js`)
  - [ ] Implement `getWorkoutExercises(req, res, next)` function
    - [ ] Support filters (workoutId, exerciseId)
    - [ ] Return matching workout exercises
  - [ ] Implement `getWorkoutExerciseById(req, res, next)` function
    - [ ] Handle 404 for non-existent entries
    - [ ] Include exercise and workout information
  - [ ] Implement `createWorkoutExercise(req, res, next)` function
    - [ ] Validate required fields (workoutId, exerciseId)
    - [ ] Verify workoutId and exerciseId exist
    - [ ] Return created association with IDs
  - [ ] Implement `updateWorkoutExercise(req, res, next)` function
    - [ ] Validate input fields
    - [ ] Update sets, reps, weight, duration, distance
    - [ ] Return updated workout exercise
  - [ ] Implement `deleteWorkoutExercise(req, res, next)` function
    - [ ] Simple deletion by ID
    - [ ] Return success/failure status

## API Route Registration
- [ ] Create main routes index file (`src/routes/index.js`)
  - [ ] Import all route modules
  - [ ] Define base path for each route (/api/...)
  - [ ] Export combined router
- [ ] Mount routes in server.js
  - [ ] Import main routes index
  - [ ] Mount routes with app.use()

## Testing

### Test Setup
- [ ] Create test database setup
  - [ ] Create in-memory SQLite for testing
  - [ ] Create test database initialization script
  - [ ] Add test data fixtures
- [ ] Configure test environment
  - [ ] Create jest.config.js
  - [ ] Set up test environment variables

### Unit Tests
- [ ] Create unit tests for models
  - [ ] Test User model methods
  - [ ] Test MuscleGroup model methods
  - [ ] Test Muscle model methods
  - [ ] Test Exercise model methods
  - [ ] Test Workout model methods
  - [ ] Test WorkoutExercise model methods
- [ ] Create unit tests for services
  - [ ] Test business logic functions
  - [ ] Test validation functions

### Integration Tests
- [ ] Create integration tests for User endpoints
  - [ ] Test all CRUD operations
  - [ ] Test filtering and pagination
  - [ ] Test relationships (user workouts)
- [ ] Create integration tests for MuscleGroup endpoints
  - [ ] Test all CRUD operations
  - [ ] Test relationships (muscle groups → muscles)
- [ ] Create integration tests for Muscle endpoints
  - [ ] Test all CRUD operations
  - [ ] Test filtering and relationships
- [ ] Create integration tests for Exercise endpoints
  - [ ] Test all CRUD operations
  - [ ] Test multiple filter parameters
  - [ ] Test muscle associations
- [ ] Create integration tests for Workout endpoints
  - [ ] Test all CRUD operations
  - [ ] Test date range filtering
  - [ ] Test exercise associations
- [ ] Create integration tests for WorkoutExercise endpoints
  - [ ] Test all CRUD operations
  - [ ] Test filtering and relationships

## Documentation

### Code Documentation
- [ ] Add JSDoc comments to all functions
  - [ ] Document parameters and return values
  - [ ] Add meaningful descriptions
  - [ ] Document exceptions and errors
- [ ] Add README.md with setup instructions
  - [ ] Include installation steps
  - [ ] Document environment variables
  - [ ] Include usage examples
  - [ ] Add API documentation links

### API Documentation
- [ ] Create API documentation
  - [ ] Document all endpoints with examples
  - [ ] Include request/response formats
  - [ ] Document query parameters and filters
  - [ ] Include error codes and messages
  - [ ] Add authentication documentation (for future implementation)

## Security Measures
- [ ] Implement basic security measures
  - [ ] Add helmet middleware for security headers
  - [ ] Implement rate limiting
  - [ ] Add input sanitization
  - [ ] Set up CORS properly
  - [ ] Validate all user inputs

## Deployment Preparation
- [ ] Create production configuration
  - [ ] Set up production environment variables
  - [ ] Configure logging for production
  - [ ] Add compression middleware
  - [ ] Set up error handling for production
- [ ] Create start scripts
  - [ ] Add production start script
  - [ ] Create database initialization script
  - [ ] Add database backup script