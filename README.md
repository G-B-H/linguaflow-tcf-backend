# LinguaFlow TCF Backend

> Full-stack backend for LinguaFlow TCF Canada exam prep platform. Express + TypeScript + MongoDB with CI/CD pipeline and Docker containerization.

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ (LTS)
- npm or yarn
- MongoDB (local or cloud instance)
- Git

### Local Development

1. **Clone and Install**
```bash
git clone https://github.com/G-B-H/linguaflow-tcf-backend.git
cd linguaflow-tcf-backend
npm install
```

2. **Environment Setup**
```bash
cp .env.example .env.local
# Edit .env.local with your configuration
```

3. **Start Development Server**
```bash
npm run dev
```

## 📁 Project Structure

```
linguaflow-tcf-backend/
├── src/
│   ├── api/              # API routes & endpoints
│   ├── services/         # Business logic
│   ├── models/           # Database schemas (MongoDB)
│   ├── middleware/       # Express middleware
│   ├── utils/            # Utility functions
│   ├── config/           # Configuration files
│   └── index.ts          # Application entry point
├── tests/                # Unit & integration tests
├── .github/workflows/    # CI/CD pipelines
├── docker/
│   └── Dockerfile        # Docker configuration
├── package.json
├── tsconfig.json         # TypeScript configuration
└── .env.example
```

## 🔧 Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Language**: TypeScript
- **Database**: MongoDB + Mongoose
- **API**: RESTful with structured response formats
- **Authentication**: JWT tokens
- **Containerization**: Docker & Docker Compose
- **CI/CD**: GitHub Actions

## 📦 Setup Instructions

### 1. Initialize Node.js Project
```bash
npm init -y
npm install express typescript ts-node dotenv cors helmet mongoose
npm install -D @types/node @types/express nodemon
```

### 2. TypeScript Configuration
Create `tsconfig.json` with proper compiler options.

### 3. Project Folders
```bash
mkdir -p src/{api,services,models,middleware,utils,config}
mkdir -p tests
mkdir -p docker
```

### 4. Git Setup & Workflow

**Branch Strategy:**
- `main` - Production ready
- `develop` - Integration branch
- `feature/*` - Feature branches
- `fix/*` - Bug fix branches

## 🔌 API Endpoints (Planned)

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/refresh` - Refresh token

### Users
- `GET /api/users/:id` - Get user profile
- `PUT /api/users/:id` - Update user
- `GET /api/users/:id/progress` - Get learning progress

### Lessons
- `GET /api/lessons` - List all lessons
- `GET /api/lessons/:id` - Get lesson details
- `POST /api/lessons/:id/complete` - Mark lesson complete

### Exercises
- `GET /api/exercises` - List exercises
- `POST /api/exercises/:id/submit` - Submit exercise
- `GET /api/exercises/:id/results` - Get exercise results

### Exams
- `GET /api/exams` - List available exams
- `POST /api/exams/start` - Start exam session
- `POST /api/exams/:id/submit` - Submit exam answers
- `GET /api/exams/:id/results` - Get exam results

## 🐳 Docker Setup

```bash
# Build image
docker build -f docker/Dockerfile -t linguaflow-backend:latest .

# Run container
docker run -p 3000:3000 --env-file .env linguaflow-backend:latest
```

## 🔄 CI/CD Pipeline

GitHub Actions workflows configured for:
- ✅ Lint & format checks (ESLint)
- ✅ TypeScript compilation
- ✅ Unit tests (Jest)
- ✅ Docker build validation
- ✅ Deployment staging/production

## 📊 Database Schema

### Collections Planned
- `users` - User accounts & profiles
- `courses` - Course content
- `lessons` - Individual lessons
- `exercises` - Practice exercises
- `exams` - Exam configurations
- `userProgress` - User learning progress
- `submissions` - Exercise & exam submissions
- `results` - Scored results

## 🚀 Deployment

### Hosting Options
- Render.com (recommended)
- Railway.app
- Heroku (legacy)
- AWS (production)

### Environment Variables
```
NODE_ENV=production
PORT=3000
MONGODB_URI=your_mongodb_connection
JWT_SECRET=your_secret_key
CORS_ORIGIN=your_frontend_url
```

## 📝 Development Workflow

1. Create feature branch from `develop`
2. Make changes with TypeScript
3. Run tests: `npm test`
4. Lint code: `npm run lint`
5. Submit PR with description
6. CI/CD pipeline validates
7. Merge to `develop` when approved
8. Deploy to staging
9. Merge `develop` → `main` for production

## 🧪 Testing

```bash
# Run tests
npm test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch
```

## 📚 API Documentation

Swagger/OpenAPI documentation available at `/api/docs` (planned)

## 🤝 Contributing

1. Follow the development workflow above
2. Ensure TypeScript strict mode compliance
3. Add tests for new features
4. Update documentation
5. Follow commit message conventions

## 📄 License

MIT

## 👨‍💼 Project Maintainer

G-B-H

---

**Next Steps:**
1. Set up local development environment
2. Create base Express server
3. Configure MongoDB connection
4. Implement authentication system
5. Build API endpoints
6. Set up CI/CD workflows
7. Configure Docker containerization
