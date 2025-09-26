# Frontend Architecture - VidyaVichara

## Overview

React-based frontend for VidyaVichara classroom Q&A sticky board system with separate interfaces for instructors and students.

## Component Structure

### Main App Components

```
src/
├── components/
│   ├── common/
│   │   ├── Header.js           # Navigation and branding
│   │   ├── LoadingSpinner.js   # Loading indicator
│   │   ├── ErrorBoundary.js    # Error handling
│   │   └── StickyNote.js       # Reusable sticky note component
│   ├── instructor/
│   │   ├── CourseSelector.js   # Select/create courses
│   │   ├── SessionManager.js   # Start/manage sessions
│   │   ├── StudentGrid.js      # Grid of students and their questions
│   │   ├── QuestionFilters.js  # Filter questions by status
│   │   └── SessionControls.js  # Session control buttons
│   └── student/
│       ├── SessionJoin.js      # Join session with ID
│       ├── QuestionForm.js     # Post new questions
│       ├── QuestionList.js     # View all questions
│       └── StatusIndicator.js  # Show question status
├── pages/
│   ├── Home.js                 # Landing page - choose role
│   ├── InstructorDashboard.js  # Main instructor interface
│   └── StudentInterface.js     # Main student interface
├── services/
│   ├── api.js                  # API service layer
│   ├── courseService.js        # Course-related API calls
│   ├── sessionService.js       # Session management API
│   └── questionService.js      # Question operations API
├── hooks/
│   ├── useApi.js              # Generic API hook
│   ├── useSession.js          # Session state management
│   └── useQuestions.js        # Question state management
├── context/
│   └── AppContext.js          # Global state management
├── styles/
│   ├── GlobalStyles.js        # Global CSS-in-JS styles
│   ├── theme.js               # Color scheme and styling variables
│   └── animations.js          # Animation definitions
└── utils/
    ├── constants.js           # Frontend constants
    ├── validators.js          # Form validation utilities
    └── helpers.js             # Utility functions
```

## User Flow

### Instructor Flow

1. **Landing Page** → Choose "Instructor"
2. **Course Selection** → Select existing course or create new
3. **Session Management** → Start new session (generates unique ID)
4. **Session Dashboard** → View student questions in grid layout
5. **Question Management** → Filter, mark as answered/important
6. **Session End** → End session when complete

### Student Flow

1. **Landing Page** → Choose "Student"
2. **Session Join** → Enter session ID provided by instructor
3. **Question Interface** → Post questions and view others
4. **Real-time Updates** → See status changes from instructor
5. **Session End** → Graceful handling when session ends

## State Management Strategy

### Global State (React Context)

- Current user role (instructor/student)
- Active session information
- Global loading and error states
- Theme and UI preferences

### Local State (Component State)

- Form inputs and validation
- Component-specific UI states
- Temporary data and interactions

### Server State (Custom Hooks)

- API data caching
- Real-time question updates
- Session status monitoring

## UI/UX Design Principles

### Sticky Note Design

- Colorful, paper-like appearance
- Different colors for status (unanswered: yellow, answered: green, important: red)
- Smooth animations for status changes
- Hover effects and interactions

### Responsive Design

- Mobile-first approach
- Grid layouts that adapt to screen size
- Touch-friendly interactions
- Accessible keyboard navigation

### Real-time Feedback

- Immediate visual feedback for actions
- Loading states for API operations
- Success/error notifications
- Status indicators for connection

## Component Specifications

### StickyNote Component

```javascript
Props:
- question: Object (content, studentName, status, timestamp)
- isInstructor: Boolean
- onStatusChange: Function
- onClick: Function (optional)
- size: String ('small'|'medium'|'large')
```

### StudentGrid Component (Instructor View)

- Grid layout of student names
- Each cell shows student's questions as mini sticky notes
- Click to expand and see full questions
- Filter controls for status
- Real-time updates when new questions arrive

### QuestionForm Component (Student View)

- Simple form with student name and question input
- Character counter and validation
- Submit button with loading state
- Success feedback after submission

## Styling Strategy

### Styled Components

- Component-scoped CSS-in-JS
- Theme integration for consistency
- Props-based dynamic styling
- Animation integration

### Color Scheme

```javascript
colors: {
  primary: '#4A90E2',      // Blue for headers and buttons
  secondary: '#F5A623',    // Orange for accents
  success: '#7ED321',      // Green for answered questions
  warning: '#F5A623',      // Orange for important questions
  danger: '#D0021B',       // Red for errors
  info: '#9013FE',         // Purple for information
  background: '#F8F9FA',   // Light gray background
  surface: '#FFFFFF',      // White for cards and surfaces
  text: '#212529',         // Dark gray for text
  muted: '#6C757D'         // Medium gray for secondary text
}
```

### Sticky Note Colors

- **Unanswered**: `#FFEB3B` (Bright Yellow)
- **Answered**: `#4CAF50` (Green)
- **Important**: `#FF5722` (Deep Orange)
- **Answered + Important**: `#9C27B0` (Purple)

## API Integration

### Service Layer Pattern

- Centralized API calls in service files
- Error handling and response transformation
- Request interceptors for common headers
- Response caching where appropriate

### Error Handling Strategy

- Global error boundary for unhandled errors
- Toast notifications for user-facing errors
- Retry mechanisms for network failures
- Graceful degradation for offline scenarios

## Performance Optimizations

### Code Splitting

- Route-based code splitting
- Lazy loading of heavy components
- Dynamic imports for utilities

### State Optimization

- Memoization for expensive calculations
- Virtualization for large question lists
- Debounced search and filters
- Efficient re-rendering strategies

## Accessibility Features

### WCAG 2.1 Compliance

- Semantic HTML structure
- ARIA labels and roles
- Keyboard navigation support
- Screen reader compatibility
- Color contrast compliance
- Focus management

### Inclusive Design

- Clear visual hierarchy
- Consistent interaction patterns
- Error messages and help text
- Multiple ways to accomplish tasks

## Security Considerations

### Input Validation

- Client-side validation for UX
- Sanitization of user inputs
- XSS prevention measures
- CSRF protection where needed

### Data Handling

- No sensitive data in localStorage
- Secure session management
- Input length limits
- Rate limiting on forms

## Testing Strategy

### Unit Testing

- Component behavior testing
- Utility function testing
- Hook testing with React Testing Library

### Integration Testing

- API integration tests
- Component interaction tests
- User flow testing

### E2E Testing

- Critical path testing
- Cross-browser compatibility
- Mobile device testing
- Accessibility testing
