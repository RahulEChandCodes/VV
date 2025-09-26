# Backend Architecture - VidyaVichara

## Database Schema Design

### Collections Overview

1. **courses** - Store course information
2. **sessions** - Active Q&A sessions for courses
3. **questions** - Individual questions posted by students
4. **users** - Basic user information (instructors/students)

### Schema Relationships

#### Course Schema

```javascript
{
  _id: ObjectId,
  title: String (required),
  code: String (required, unique),
  instructor: ObjectId (ref: User),
  description: String,
  createdAt: Date,
  updatedAt: Date
}
```

#### Session Schema

```javascript
{
  _id: ObjectId,
  sessionId: String (unique, auto-generated),
  course: ObjectId (ref: Course),
  instructor: ObjectId (ref: User),
  isActive: Boolean (default: true),
  startTime: Date,
  endTime: Date,
  createdAt: Date,
  updatedAt: Date
}
```

#### Question Schema

```javascript
{
  _id: ObjectId,
  sessionId: ObjectId (ref: Session),
  studentName: String (required),
  content: String (required),
  status: String (enum: ['unanswered', 'answered', 'important']),
  isImportant: Boolean (default: false),
  isAnswered: Boolean (default: false),
  timestamp: Date,
  createdAt: Date,
  updatedAt: Date
}
```

#### User Schema (Optional - for future authentication)

```javascript
{
  _id: ObjectId,
  name: String,
  email: String,
  role: String (enum: ['instructor', 'student']),
  createdAt: Date
}
```

## API Endpoints Design

### Course Management

- `GET /api/courses` - Get all courses
- `POST /api/courses` - Create new course
- `GET /api/courses/:id` - Get specific course
- `PUT /api/courses/:id` - Update course
- `DELETE /api/courses/:id` - Delete course

### Session Management

- `POST /api/sessions` - Start new session for a course
- `GET /api/sessions/:sessionId` - Get session details
- `PUT /api/sessions/:sessionId/end` - End session
- `GET /api/courses/:courseId/sessions` - Get all sessions for a course

### Question Management

- `POST /api/questions` - Post new question to session
- `GET /api/sessions/:sessionId/questions` - Get all questions for session
- `PUT /api/questions/:id/status` - Update question status (answered/important)
- `DELETE /api/questions/:id` - Delete question (optional)
- `GET /api/questions/filter` - Filter questions by status

### Real-time Features (Socket.io)

- `join-session` - Student/instructor joins session
- `new-question` - Broadcast new question to session
- `question-updated` - Broadcast status updates
- `session-ended` - Notify session end

## Directory Structure

```
backend/
├── config/
│   ├── database.js      # MongoDB connection
│   └── socket.js        # Socket.io configuration
├── models/
│   ├── Course.js        # Course schema
│   ├── Session.js       # Session schema
│   ├── Question.js      # Question schema
│   └── User.js          # User schema (future)
├── routes/
│   ├── courses.js       # Course routes
│   ├── sessions.js      # Session routes
│   └── questions.js     # Question routes
├── middleware/
│   ├── validation.js    # Input validation
│   ├── errorHandler.js  # Global error handling
│   └── cors.js          # CORS configuration
├── utils/
│   ├── sessionGenerator.js  # Generate unique session IDs
│   └── constants.js     # App constants
├── server.js            # Main server file
├── package.json
└── .env.example
```

## Status Management Logic

- **Unanswered**: Default state for new questions
- **Answered**: Marked by instructor when question is addressed
- **Important**: Can be marked by instructor (can coexist with answered)
- **Both**: Question can be both answered AND important

## Security Considerations

- Input validation for all endpoints
- Rate limiting for question posting
- Session ID validation
- CORS configuration for frontend integration
- Basic sanitization of question content

## Performance Optimizations

- Indexed fields: sessionId, course, timestamp
- Pagination for question lists
- Connection pooling for MongoDB
- Efficient real-time updates via Socket.io rooms
