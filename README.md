# 🏢 Multi-Tenant SaaS Platform - Project & Task Management System

> **Enterprise-grade containerized application with strict tenant isolation and role-based access control**

A comprehensive SaaS solution for managing organizations, projects, and tasks with complete data isolation between tenants. Built with modern technologies and production-ready containerization.


![Project Status](https://img.shields.io/badge/status-production_ready-green)
![Docker](https://img.shields.io/badge/docker-containerized-blue)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## ✨ Core Features

### 🔒 Tenant Security Architecture
- **Shared Database, Isolated Schema** implementation
- Automatic `tenantId` injection in all database queries
- Complete data separation between organizations
- Global middleware enforcing tenant boundaries

### 👥 Advanced User Management
- **Three-tier Role System:**
  - **Super Administrator:** System-wide oversight
  - **Tenant Administrator:** Organization management
  - **Standard User:** Task-level operations
- Dynamic permissions based on role assignments
- Secure session management with JWT tokens

### 📊 Resource Limitation System
- Subscription-based quota enforcement
- Real-time validation of `max_users` and `max_projects`
- Automated restriction on resource creation
- Support for Free, Professional, and Enterprise tiers

### 🛡️ Security Implementation
- **bcrypt** for password hashing
- **Helmet.js** for HTTP header security
- **CORS** with strict origin policies
- Input validation and sanitization
- 24-hour JWT token expiration

### 🐳 Container Orchestration
- Multi-service Docker Compose setup
- Automatic database initialization
- Health checks and dependency management
- Persistent data storage with named volumes

### 🔄 Automated Operations
- Zero-configuration database migrations
- Pre-seeded demo data for testing
- Self-healing service dependencies
- Environment-based configuration

## 🏗️ System Architecture

### Technology Stack
**Frontend:** React 18 + Vite for User interface
**Styling:** Tailwind CSS 3.4 for Responsive design
**Backend:** Node.js + Express for API server
**Database:** PostgreSQL 15 for Data persistence
**ORM:** Sequelize 6 for Database operations
**Container:** Docker Compose for Service orchestration

### Service Architecture
- **Frontend Service:** Port 3000 - React application
- **Backend Service:** Port 5000 - REST API endpoints
- **Database Service:** Port 5432 - PostgreSQL instance

### Data Flow
1. User requests pass through tenant identification
2. Middleware injects tenant context
3. Database queries include tenant isolation
4. Responses filtered by user permissions

## 🚀 Quick Start Guide

### Prerequisites
- Docker Engine 20.10+
- Docker Compose 2.0+
- 4GB available RAM
- Git version control

### Installation Steps

1. **Clone the Repository**
```bash
git clone https://github.com/sarayu1201/multi-saas.git
cd multi-saas
```

2. **Launch the Application**
```bash
# Build and start all services
docker-compose up -d --build

# Monitor startup progress
docker-compose logs -f
```

3. **Verify Services**
```bash
# Check service status
docker-compose ps

# Test backend health
curl http://localhost:5000/api/health
# Expected: {"status":"ok","database":"connected"}
```

4. **Access the Application**
- **Frontend Interface:** http://localhost:3000
- **Backend API:** http://localhost:5000/api
- **Database:** localhost:5432 (credentials in env)

### Stopping the Application
```bash
# Graceful shutdown
docker-compose down

# Remove with volumes (clears database)
docker-compose down -v
```


## 📺 Video Demo
**[🎥 Click here to watch the Project Walkthrough & Architecture Demo](https://youtu.be/yZiGGtF_4So?si=geAwnyCV9oK8Xs43c)**

---

## 🔧 Configuration

### Environment Variables

The system uses these environment configurations:

**Database Configuration**
```
DB_HOST=database
DB_PORT=5432
DB_NAME=saas_db
DB_USER=postgres
DB_PASSWORD=postgres
```

**Application Settings**
```
PORT=5000
JWT_SECRET=supersecretkey_changeinproduction
FRONTEND_URL=http://frontend:3000
NODE_ENV=production
```

**Docker Service Names**
- `database`: PostgreSQL service
- `backend`: Node.js API service
- `frontend`: React application service

### Volume Management
- `postgres_data`: Persistent database storage
- Automatic backup on container restart
- Data persistence across deployments

## 👤 Authentication & Authorization

### Login Credentials

**Demo Organization (Tenant: demo)**
```
Administrator:
  Email: admin@demo.com
  Password: Demo@123
  Permissions: Full tenant access

Standard User:
  Email: user1@demo.com
  Password: User@123
  Permissions: Limited access
```

**System Administration**
```
Super Administrator:
  Email: superadmin@system.com
  Password: Admin@123
  Permissions: System-wide access
```

### Role Capabilities

**Super Admin**
- Tenant Management: Full
- User Management: Full
- Project Management: Full
- Task Management: Full

**Tenant Admin**
- Tenant Management: Restricted
- User Management: Full
- Project Management: Full
- Task Management: Full

**Standard User**
- Tenant Management: None
- User Management: None
- Project Management: Limited
- Task Management: Limited

## ⚙️ Environment Variables

The application is pre-configured for the evaluation environment. The `.env` variables are handled via `docker-compose.yml`.

| Variable | Description | Default Value |
| --- | --- | --- |
| `PORT` | Backend API Port | `5000` |
| `DB_HOST` | Database Service Name | `database` |
| `DB_PORT` | PostgreSQL Port | `5432` |
| `DB_NAME` | Database Name | `saas_db` |
| `DB_USER` | Database User | `postgres` |
| `DB_PASSWORD` | Database Password | `postgres` |
| `JWT_SECRET` | Secret for signing tokens | `supersecretkey...` |
| `FRONTEND_URL` | CORS Origin URL | `http://frontend:3000` |

---

## 📚 API Documentation

The backend exposes a comprehensive REST API. Full documentation is available in `docs/API.md`.

**Core Endpoints:**

| Method | Endpoint | Description | Access |
| --- | --- | --- | --- |
| `POST` | `/api/auth/login` | User login | Public |
| `POST` | `/api/auth/register-tenant` | Register new organization | Public |
| `GET` | `/api/projects` | List projects for tenant | Auth Required |
| `POST` | `/api/projects` | Create new project | Tenant Admin |
| `GET` | `/api/tenants` | List all tenants | Super Admin |

---

## 📁 Project Structure

```
multi-saas/
├── backend/
│   ├── src/
│   │   ├── config/         # Database and app configuration
│   │   ├── controllers/    # Request handlers
│   │   ├── middleware/     # Authentication and tenant isolation
│   │   ├── models/         # Sequelize data models
│   │   ├── routes/         # API route definitions
│   │   └── scripts/        # Database initialization scripts
│   ├── Dockerfile          # Backend container definition
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/          # Application views
│   │   ├── api/            # API client configuration
│   │   └── utils/          # Helper functions
│   ├── Dockerfile          # Frontend container definition
│   └── package.json
├── docker-compose.yml      # Multi-service orchestration
├── .env.example           # Environment template
└── README.md              # This documentation
```

## 🐛 Troubleshooting

### Common Issues

1. **Port Conflicts**
```bash
# Check port usage
sudo lsof -i :3000
sudo lsof -i :5000
sudo lsof -i :5432

# Stop conflicting services
sudo service apache2 stop  # If using port 80
sudo service nginx stop    # If using port 80
```

2. **Docker Container Issues**
```bash
# Restart all services
docker-compose down
docker-compose up -d

# Check container logs
docker-compose logs backend
docker-compose logs frontend
docker-compose logs database
```

3. **Database Connection Problems**
```bash
# Test database connectivity
docker exec -it multi-saas-database-1 psql -U postgres -d saas_db

# Reset database
docker-compose down -v
docker-compose up -d
```

### Health Checks
```bash
# Verify all services are running
docker-compose ps

# Check backend health endpoint
curl -f http://localhost:5000/api/health

# View real-time logs
docker-compose logs --tail=50 -f
```

## 🔄 Development Workflow

### Local Development
```bash
# Start development environment
docker-compose -f docker-compose.dev.yml up

# Run database migrations
docker-compose exec backend npm run migrate

# Seed development data
docker-compose exec backend npm run seed
```

### Production Deployment
```bash
# Build optimized images
docker-compose build --no-cache

# Deploy with production settings
docker-compose -f docker-compose.prod.yml up -d

# Monitor deployment
docker-compose logs -f --tail=100
```

## 📈 Monitoring & Maintenance

### Resource Monitoring
```bash
# View container resource usage
docker stats

# Check database size
docker exec multi-saas-database-1 psql -U postgres -d saas_db -c "SELECT pg_size_pretty(pg_database_size('saas_db'));"

# Monitor API requests
docker-compose logs backend --tail=100 | grep "GET\|POST\|PUT\|DELETE"
```

### Backup Procedures
```bash
# Create database backup
docker exec multi-saas-database-1 pg_dump -U postgres saas_db > backup_$(date +%Y%m%d).sql

# Restore from backup
cat backup.sql | docker exec -i multi-saas-database-1 psql -U postgres saas_db
```

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -am 'Add new feature'`)
4. Push to branch (`git push origin feature/improvement`)
5. Create a Pull Request

## 🆘 Support

For issues and questions:
1. Check the troubleshooting section
2. Review closed GitHub issues
3. Create a new issue with detailed description

