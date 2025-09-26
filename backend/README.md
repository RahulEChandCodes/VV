# VidyaVichara Backend API

## Setup Instructions

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (running locally or connection string)

### Installation
1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create environment file:
   ```bash
   cp .env.example .env
   ```

4. Update `.env` file with your MongoDB connection string:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/vidyavichara
   JWT_SECRET=your-jwt-secret-key-here
   NODE_ENV=development
   ```

### Running the Server

#### Development Mode (with auto-restart)
```bash
npm run dev
```

#### Production Mode
```bash
npm start
```

### API Endpoints

#### Health Check
- **GET** `/health` - Check API status

#### Courses
- **GET** `/api/courses` - Get all courses
- **POST** `/api/courses` - Create new course
- **GET** `/api/courses/:id` - Get specific course
- **PUT** `/api/courses/:id` - Update course
- **DELETE** `/api/courses/:id` - Delete course

#### Sessions
- **POST** `/api/sessions` - Start new session
- **GET** `/api/sessions/:sessionId` - Get session details
- **PUT** `/api/sessions/:sessionId/end` - End session
- **GET** `/api/sessions/course/:courseId` - Get sessions for course
- **GET** `/api/sessions/course/:courseId/active` - Get active session

#### Questions
- **POST** `/api/questions` - Post new question
- **GET** `/api/questions/session/:sessionId` - Get questions for session
- **GET** `/api/questions/session/:sessionId/by-student` - Get questions grouped by student
- **PUT** `/api/questions/:id/status` - Update question status
- **DELETE** `/api/questions/:id` - Delete question

### Example Requests

#### Create a Course
```bash
curl -X POST http://localhost:5000/api/courses \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Software System Development",
    "code": "CS301",
    "instructor": "Dr. Smith",
    "description": "Advanced software engineering concepts"
  }'
```

#### Start a Session
```bash
curl -X POST http://localhost:5000/api/sessions \
  -H "Content-Type: application/json" \
  -d '{
    "courseId": "COURSE_ID_HERE",
    "instructor": "Dr. Smith"
  }'
```

#### Post a Question
```bash
curl -X POST http://localhost:5000/api/questions \
  -H "Content-Type: application/json" \
  -d '{
    "sessionId": "VV-ABC123",
    "studentName": "John Doe",
    "content": "What is the difference between horizontal and vertical scaling?"
  }'
```

### Features
- ✅ Course management
- ✅ Session creation and management
- ✅ Question posting and status updates
- ✅ Real-time question filtering
- ✅ Input validation and sanitization
- ✅ Rate limiting and security headers
- ✅ Error handling and logging
- ✅ CORS configuration

### Security Features
- Request rate limiting
- Input validation
- Security headers
- Error handling
- Request logging

### Database Schema
- **courses**: Store course information
- **sessions**: Active Q&A sessions
- **questions**: Individual questions with status tracking
- **users**: User management (future enhancement)

### Status Management
Questions can have multiple status flags:
- `isAnswered`: Boolean - marked when instructor addresses the question
- `isImportant`: Boolean - marked by instructor for class-wide attention
- `status`: String - computed field reflecting current state

### Development Notes
- Uses Express.js with MongoDB/Mongoose
- Implements comprehensive validation
- Includes proper error handling
- Configured for development and production environments
- Ready for Socket.io integration for real-time features