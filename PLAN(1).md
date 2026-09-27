# PLAN.md

# DeployCloud — Full Cloud Deployment Platform

## Master Implementation Specification for Claude Code

---

## 0. IMPORTANT INSTRUCTIONS TO CLAUDE CODE

You are the principal engineer responsible for building this entire application.

Do NOT create a mockup, static demo, fake dashboard, or collection of disconnected pages.

Build a fully functional, production-oriented cloud deployment platform.

The product should provide the major workflows users expect from a modern platform-as-a-service/deployment platform:

- Authentication
- Organizations / teams
- Projects
- Git repository integration
- Project importing
- Automatic framework detection
- Deployments
- Preview deployments
- Production deployments
- Build pipeline
- Real-time build logs
- Deployment URLs
- Custom domains
- DNS management
- SSL/TLS
- Environment variables
- Runtime functions
- Runtime logs
- Analytics
- Project settings
- Team management
- Roles and permissions
- Integrations
- Webhooks
- Deploy hooks
- Cron jobs
- Storage
- Usage
- Billing
- API keys
- CLI
- Notifications
- Audit logs
- Admin dashboard
- Security controls
- Documentation
- API reference
- Marketplace/integrations
- Advanced AI infrastructure hooks

The platform should be inspired by the product architecture and functionality of modern platforms such as Vercel, but must have:

- Original branding
- Original code
- Original UI
- Original copy
- Original icons/assets
- Original architecture

Do not copy proprietary source code, private infrastructure, trademarks, logos, or copyrighted visual assets.

---

# 1. PRODUCT NAME

Temporary working name:

```text
DeployCloud
```

Make branding configurable.

Example:

```env
NEXT_PUBLIC_APP_NAME=DeployCloud
NEXT_PUBLIC_ROOT_DOMAIN=deploycloud.app
NEXT_PUBLIC_DASHBOARD_DOMAIN=app.deploycloud.app
```

All branding must eventually be changeable from one configuration file.

---

# 2. PRIMARY OBJECTIVE

The final product should allow a developer to do this:

```text
GitHub Repository
       ↓
Connect Repository
       ↓
Create Project
       ↓
Detect Framework
       ↓
Configure Build
       ↓
Configure Environment Variables
       ↓
Deploy
       ↓
Build Worker
       ↓
Upload Artifacts
       ↓
Generate Deployment
       ↓
Generate HTTPS URL
       ↓
Preview / Production
```

Example:

User enters:

```text
Repository:
github.com/example/my-next-app

Project:
my-next-app

Framework:
Next.js

Branch:
main
```

The platform produces:

```text
Deployment successful

https://my-next-app-8f42.deploycloud.app

Environment:
Production

Status:
READY

Commit:
7f42a91

Build:
42 seconds
```

---

# 3. DESIGN DIRECTION

The website must feel like a premium developer platform.

Do NOT make it look like a generic admin template.

Design characteristics:

```text
Minimal
Premium
Developer-focused
Fast
Technical
Professional
Dense but readable
Responsive
Accessible
Keyboard-friendly
```

Use:

```text
Next.js
React
TypeScript
Tailwind CSS
shadcn/ui
Radix UI
Lucide Icons
Framer Motion
Monaco Editor
Recharts
```

Use subtle animations.

Avoid:

```text
Huge gradients everywhere
Excessive glassmorphism
Unnecessary cards
AI-generated-looking UI
Random rounded components
Huge text
Excessive shadows
Slow animations
```

---

# 4. APPLICATION STRUCTURE

Use a monorepo.

```text
deploycloud/
│
├── apps/
│   ├── web/
│   ├── api/
│   ├── worker/
│   ├── builder/
│   ├── router/
│   ├── admin/
│   └── docs/
│
├── packages/
│   ├── ui/
│   ├── database/
│   ├── auth/
│   ├── config/
│   ├── logger/
│   ├── queue/
│   ├── git/
│   ├── deployments/
│   ├── domains/
│   ├── storage/
│   ├── analytics/
│   ├── billing/
│   ├── security/
│   └── sdk/
│
├── infra/
│   ├── docker/
│   ├── kubernetes/
│   ├── terraform/
│   └── nginx/
│
├── scripts/
├── docs/
├── tests/
├── docker-compose.yml
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
└── PLAN.md
```

---

# 5. TECHNOLOGY STACK

## Frontend

```text
Next.js
React
TypeScript
Tailwind CSS
shadcn/ui
Radix UI
TanStack Query
Zustand
Framer Motion
Recharts
Monaco Editor
Lucide
```

## Backend

```text
Node.js
TypeScript
Fastify
Zod
Drizzle ORM
PostgreSQL
Redis
BullMQ
WebSockets / SSE
```

## Infrastructure

```text
Docker
Kubernetes
containerd
Nginx
Traefik or Envoy
S3-compatible storage
MinIO for development
PostgreSQL
Redis
Prometheus
Grafana
Loki
OpenTelemetry
```

## Authentication

Use:

```text
Better Auth
```

or an equivalent secure authentication implementation.

Support:

```text
Email/password
Google OAuth
GitHub OAuth
GitLab OAuth
Sessions
Email verification
Password reset
2FA
API tokens
```

---

# 6. LOCAL DEVELOPMENT

The entire platform must start with:

```bash
docker compose up -d
```

Then:

```bash
pnpm install
pnpm dev
```

Provide:

```text
.env.example
docker-compose.yml
README.md
```

The local stack should include:

```text
PostgreSQL
Redis
MinIO
API
Web
Worker
Builder
Router
```

---

# 7. ENVIRONMENT VARIABLES

Create:

```text
.env.example
```

Example:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/deploycloud

REDIS_URL=redis://localhost:6379

S3_ENDPOINT=http://localhost:9000
S3_ACCESS_KEY=minio
S3_SECRET_KEY=miniosecret
S3_BUCKET=deployments

AUTH_SECRET=

GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=

GITLAB_CLIENT_ID=
GITLAB_CLIENT_SECRET=

GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=

ROOT_DOMAIN=deploycloud.local

API_URL=http://localhost:4000
WEB_URL=http://localhost:3000

ENCRYPTION_KEY=

STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
```

Never commit secrets.

---

# 8. DATABASE

Use PostgreSQL.

Core tables:

```text
users
sessions
accounts
teams
team_members
team_invitations
projects
project_members
repositories
git_connections
deployments
deployment_files
builds
build_logs
runtime_logs
domains
dns_records
certificates
environment_variables
deployment_aliases
preview_environments
functions
function_invocations
function_logs
analytics_events
integrations
integration_installations
api_tokens
webhooks
deploy_hooks
cron_jobs
storage_buckets
storage_objects
usage_records
plans
subscriptions
invoices
notifications
audit_logs
feature_flags
```

---

# 9. USER MODEL

Example:

```ts
type User = {
  id: string
  email: string
  name: string
  avatarUrl?: string
  emailVerified: boolean
  createdAt: Date
  updatedAt: Date
}
```

---

# 10. TEAM MODEL

Every project belongs to a team.

```text
User
 ↓
Team
 ↓
Projects
 ↓
Deployments
```

Team roles:

```text
OWNER
ADMIN
MEMBER
BILLING
VIEWER
```

Permissions must be checked server-side.

Never trust frontend role checks.

---

# 11. PROJECT MODEL

Example:

```ts
type Project = {
  id: string
  teamId: string
  name: string
  slug: string

  framework?: string

  repositoryId?: string

  productionBranch: string

  rootDirectory: string

  installCommand?: string
  buildCommand?: string
  outputDirectory?: string

  createdAt: Date
  updatedAt: Date
}
```

---

# 12. AUTHENTICATION PAGES

Create:

```text
/login
/signup
/forgot-password
/reset-password
/verify-email
/2fa
```

Login example:

```text
┌──────────────────────────────┐
│        DeployCloud           │
│                              │
│  Email                       │
│  [ user@example.com       ]  │
│                              │
│  Password                    │
│  [ •••••••••••••         ]  │
│                              │
│  [ Sign In ]                 │
│                              │
│  Continue with GitHub        │
│  Continue with Google        │
│                              │
│  Forgot password?            │
└──────────────────────────────┘
```

---

# 13. DASHBOARD

Route:

```text
/dashboard
```

Navigation:

```text
Overview
Projects
Deployments
Domains
Storage
Analytics
Observability
Integrations
Team
Settings
Billing
```

Top navigation:

```text
Team selector
Search
Command menu
Notifications
Help
User menu
```

---

# 14. PROJECT LIST

Display:

```text
Projects

[ Add New ]

my-store
Next.js
Production: Ready

portfolio
Astro
Production: Ready

api-service
Node.js
Production: Error
```

Project actions:

```text
Open
Deploy
Settings
Domains
Delete
```

---

# 15. PROJECT CREATION

Route:

```text
/new
```

Step 1:

```text
Import Git Repository
```

Providers:

```text
GitHub
GitLab
Bitbucket
Azure DevOps
```

Also:

```text
Deploy with CLI
Upload ZIP
Template
```

---

# 16. GITHUB INTEGRATION

Implement GitHub OAuth / GitHub App.

Flow:

```text
User
 ↓
Connect GitHub
 ↓
OAuth
 ↓
Authorize
 ↓
Select repositories
 ↓
Return to DeployCloud
```

Repository list:

```text
example/frontend
example/backend
example/mobile-api
```

Search:

```text
Search repositories...
```

---

# 17. GITHUB WEBHOOKS

Endpoint:

```http
POST /api/webhooks/github
```

Verify:

```text
X-Hub-Signature-256
```

Events:

```text
push
pull_request
repository
installation
```

For push:

```text
Webhook
 ↓
Verify signature
 ↓
Find project
 ↓
Check branch
 ↓
Create deployment
 ↓
Queue build
```

---

# 18. PROJECT CONFIGURATION

After selecting repository:

```text
Project Name
Framework Preset
Root Directory
Build Command
Install Command
Output Directory
Node Version
Environment Variables
```

Example:

```text
Project Name:
shop

Framework:
Next.js

Root Directory:
/

Build Command:
npm run build

Install Command:
npm install

Output:
.next
```

---

# 19. FRAMEWORK DETECTION

Detect framework from repository.

Example:

```ts
function detectFramework(files: string[]) {
  if (
    files.includes("next.config.js") ||
    files.includes("next.config.mjs") ||
    files.includes("next.config.ts")
  ) {
    return "nextjs"
  }

  if (
    files.includes("vite.config.ts") ||
    files.includes("vite.config.js")
  ) {
    return "vite"
  }

  if (files.includes("astro.config.mjs")) {
    return "astro"
  }

  if (files.includes("nuxt.config.ts")) {
    return "nuxt"
  }

  return "unknown"
}
```

Return:

```json
{
  "framework": "nextjs",
  "buildCommand": "npm run build",
  "outputDirectory": ".next"
}
```

---

# 20. DEPLOYMENT ENGINE

Deployment states:

```text
QUEUED
CLONING
DETECTING
INSTALLING
BUILDING
TESTING
PACKAGING
UPLOADING
DEPLOYING
READY
ERROR
CANCELED
```

Architecture:

```text
API
 ↓
Redis/BullMQ
 ↓
Deployment Worker
 ↓
Build Worker
 ↓
Artifact Storage
 ↓
Deployment Router
```

---

# 21. DEPLOYMENT QUEUE

When user deploys:

```ts
await deploymentQueue.add("build", {
  deploymentId,
  projectId,
  commitSha,
  environment
})
```

Worker:

```ts
worker.process(async job => {
  const deployment = await getDeployment(job.data.deploymentId)

  await updateStatus(deployment.id, "CLONING")

  await cloneRepository()

  await updateStatus(deployment.id, "INSTALLING")

  await installDependencies()

  await updateStatus(deployment.id, "BUILDING")

  await runBuild()

  await updateStatus(deployment.id, "UPLOADING")

  await uploadArtifacts()

  await updateStatus(deployment.id, "READY")
})
```

---

# 22. BUILD ISOLATION

Never run arbitrary user code directly inside the API server.

Builds must run inside isolated environments.

Development:

```bash
docker run --rm \
  --memory=4g \
  --cpus=2 \
  --network=none \
  deploycloud-builder
```

Production:

```text
Kubernetes
+
isolated builder pods
```

Implement:

```text
CPU limits
RAM limits
Disk limits
Timeouts
PID limits
Network policies
Read-only base filesystem
Temporary workspace
Non-root execution
Seccomp
AppArmor where available
```

---

# 23. BUILD LOGS

Store build logs.

Example:

```text
[10:31:01] Starting deployment...
[10:31:02] Cloning repository...
[10:31:05] Repository cloned
[10:31:05] Detecting framework...
[10:31:05] Framework: Next.js
[10:31:06] Installing dependencies...
[10:31:19] Dependencies installed
[10:31:19] Running npm run build
[10:31:38] Build successful
[10:31:40] Uploading artifacts...
[10:31:42] Deployment ready
```

---

# 24. REAL-TIME LOGS

Use:

```text
Server-Sent Events
```

Endpoint:

```http
GET /api/deployments/:id/logs/stream
```

Frontend:

```ts
const source = new EventSource(
  `/api/deployments/${deploymentId}/logs/stream`
)

source.onmessage = event => {
  const log = JSON.parse(event.data)

  appendLog(log)
}
```

Use WebSockets if bidirectional communication becomes necessary.

---

# 25. DEPLOYMENT URL

Each deployment gets:

```text
https://<deployment-id>.<root-domain>
```

Example:

```text
https://dpl-01j2k.deploycloud.app
```

Project alias:

```text
https://shop.deploycloud.app
```

Preview:

```text
https://shop-git-feature-login.deploycloud.app
```

---

# 26. DEPLOYMENT ROUTER

Request:

```http
GET https://shop.deploycloud.app/
```

Router:

```text
Host
 ↓
Find alias
 ↓
Find project
 ↓
Find active deployment
 ↓
Resolve artifact
 ↓
Serve
```

For:

```text
/api/users
```

route to the correct server function.

---

# 27. STATIC DEPLOYMENTS

Upload build artifacts to S3/MinIO.

Example:

```text
bucket/
└── deployments/
    └── dpl_123/
        ├── index.html
        ├── assets/
        ├── _next/
        └── manifest.json
```

Use immutable deployment IDs.

---

# 28. PREVIEW DEPLOYMENTS

When:

```bash
git push origin feature/login
```

create:

```text
Preview Deployment

https://shop-git-feature-login.deploycloud.app
```

Preview environments must not automatically replace production.

---

# 29. PULL REQUEST PREVIEWS

For GitHub PR:

```text
PR #42
 ↓
Build
 ↓
Preview
 ↓
GitHub comment/status
```

Example:

```text
DeployCloud Preview

Deployment: READY

https://shop-git-feature-login.deploycloud.app
```

---

# 30. PRODUCTION DEPLOYMENT

Production branch:

```text
main
```

When main changes:

```text
Push
 ↓
Build
 ↓
Test
 ↓
Deploy
 ↓
Update production alias
```

---

# 31. ROLLBACK

Deployment page:

```text
v44 READY
v43 READY
v42 READY
```

Action:

```text
Promote to Production
```

Do not rebuild.

Simply update:

```text
production alias
```

from:

```text
deployment_44
```

to:

```text
deployment_43
```

---

# 32. DEPLOYMENT PAGE

Route:

```text
/dashboard/projects/:project/deployments/:deployment
```

Tabs:

```text
Overview
Build Logs
Runtime Logs
Functions
Analytics
Domains
```

Information:

```text
Status
Commit
Branch
Environment
Framework
Build time
Created time
Deployment URL
Creator
```

Actions:

```text
Visit
Redeploy
Promote
Rollback
Cancel
Delete
```

---

# 33. ENVIRONMENT VARIABLES

Support:

```text
Development
Preview
Production
```

Example:

```text
DATABASE_URL
************
Production

NEXT_PUBLIC_API_URL
https://api.example.com
Preview
```

API:

```http
POST /api/projects/:projectId/env
```

Input:

```json
{
  "key": "DATABASE_URL",
  "value": "postgresql://...",
  "targets": [
    "production",
    "preview"
  ]
}
```

Encrypt values before storing.

Never expose secret values after creation unless explicitly designed for that purpose.

Never print them in logs.

---

# 34. DOMAIN MANAGEMENT

Route:

```text
/dashboard/projects/:project/domains
```

Add domain:

```text
example.com
```

Output:

```text
Domain verification required

Type: TXT

Name:
_deploycloud

Value:
deploycloud-verification=abc123
```

After verification:

```text
● Valid
```

---

# 35. DNS

Support:

```text
A
AAAA
CNAME
TXT
MX
CAA
NS
```

DNS UI:

```text
Type    Name    Value                 TTL

A       @       203.0.113.10          300

CNAME   www     shop.deploycloud.app  300

TXT     @       verification=abc      300
```

---

# 36. SSL/TLS

Implement:

```text
Let's Encrypt
ACME
```

Flow:

```text
Domain
 ↓
Verification
 ↓
ACME challenge
 ↓
Certificate
 ↓
Proxy
 ↓
HTTPS
```

Automatically renew certificates.

---

# 37. REDIRECTS

Support:

```text
example.com → www.example.com
```

And:

```text
www.example.com → example.com
```

Also project-level redirects.

Example:

```json
{
  "source": "/old",
  "destination": "/new",
  "statusCode": 308
}
```

---

# 38. SERVERLESS FUNCTIONS

Support functions such as:

```text
api/hello.ts
api/users.ts
api/payment.ts
```

Example:

```ts
export default async function handler(req, res) {
  res.status(200).json({
    message: "Hello World"
  })
}
```

Request:

```http
GET /api/hello
```

Output:

```json
{
  "message": "Hello World"
}
```

---

# 39. FUNCTION RUNTIME

Initial runtimes:

```text
Node.js 20
Node.js 22
Python 3.11
Python 3.12
```

Later:

```text
Go
Rust
Bun
```

---

# 40. FUNCTION CONFIGURATION

Allow:

```text
Memory
CPU
Timeout
Concurrency
Region
Runtime
```

Example:

```json
{
  "runtime": "nodejs22",
  "memory": 1024,
  "timeout": 60,
  "region": "india-central"
}
```

---

# 41. FUNCTION LOGS

Example:

```text
Function:
api/orders

Invocation:
inv_123

Duration:
182ms

Status:
200

Logs:

[12:31:04] Request received
[12:31:04] Database connected
[12:31:04] Query completed
[12:31:04] Response sent
```

---

# 42. FUNCTION METRICS

Display:

```text
Invocations
Errors
Average duration
p50
p95
p99
Cold starts
Memory usage
Timeouts
```

Charts:

```text
Requests
Errors
Latency
```

---

# 43. ANALYTICS

Collect privacy-conscious analytics.

Events:

```text
page_view
route_change
click
error
performance
```

Dashboard:

```text
Visitors
Page Views
Top Pages
Referrers
Countries
Devices
Browsers
Performance
```

Example:

```text
Visitors        12,481
Page Views      39,284
Bounce Rate     31.4%
```

Do not collect unnecessary personal information.

---

# 44. WEB PERFORMANCE

Track:

```text
LCP
CLS
INP
FCP
TTFB
```

Show:

```text
Performance score
```

and trends.

---

# 45. OBSERVABILITY

Build:

```text
Logs
Metrics
Traces
Errors
```

Use:

```text
OpenTelemetry
Prometheus
Loki
Grafana
```

Application UI:

```text
Observability

Logs
Metrics
Traces
Errors
```

---

# 46. LOG SEARCH

Allow:

```text
Search logs
Filter by deployment
Filter by function
Filter by status
Filter by timestamp
Filter by environment
```

Example:

```text
status:500
```

or:

```text
deployment:dpl_123
```

---

# 47. ERROR TRACKING

Capture:

```text
Exception
Stack trace
Deployment
Function
Request
User agent
Timestamp
```

Group identical errors.

Example:

```text
TypeError: Cannot read properties of undefined

Occurrences:
1,842

First seen:
2h ago

Last seen:
2m ago
```

---

# 48. STORAGE

Provide object storage.

UI:

```text
Storage

Buckets

uploads
media
backups
```

Bucket actions:

```text
Create
Delete
Upload
Download
Delete object
Generate signed URL
```

---

# 49. OBJECT STORAGE API

Upload:

```http
PUT /api/storage/:bucket/:key
```

Download:

```http
GET /api/storage/:bucket/:key
```

Signed URL:

```http
POST /api/storage/signed-url
```

Response:

```json
{
  "url": "https://storage.example.com/signed/abc"
}
```

---

# 50. DATABASE STORAGE

Offer optional managed database integrations.

Initial integrations:

```text
PostgreSQL
MySQL
Redis
MongoDB
```

The platform may initially provision external database connections rather than operating databases itself.

---

# 51. CRON JOBS

Project settings:

```text
Cron Jobs
```

Example:

```text
0 0 * * *
```

Target:

```text
/api/cleanup
```

Output:

```text
Next run:
Tomorrow 00:00
```

Scheduler:

```text
Cron scheduler
 ↓
Queue
 ↓
Function invocation
```

---

# 52. WEBHOOKS

Project webhooks:

```text
Deployment Created
Deployment Ready
Deployment Failed
Domain Added
Domain Removed
```

Example:

```http
POST https://example.com/webhook
```

Payload:

```json
{
  "event": "deployment.ready",
  "deploymentId": "dpl_123",
  "projectId": "prj_123"
}
```

Implement:

```text
signature
retry
exponential backoff
delivery logs
replay
```

---

# 53. DEPLOY HOOKS

Create endpoint:

```text
https://api.deploycloud.app/hooks/deploy/abc123
```

Calling it:

```bash
curl https://api.deploycloud.app/hooks/deploy/abc123
```

creates a deployment.

---

# 54. API

Version API:

```text
/api/v1
```

Endpoints:

```text
GET    /projects
POST   /projects
GET    /projects/:id
PATCH  /projects/:id
DELETE /projects/:id

GET    /deployments
POST   /deployments
GET    /deployments/:id
POST   /deployments/:id/redeploy
POST   /deployments/:id/promote
DELETE /deployments/:id

GET    /domains
POST   /domains
DELETE /domains/:id

GET    /env
POST   /env
PATCH  /env/:id
DELETE /env/:id

GET    /teams
POST   /teams

GET    /members
POST   /members/invite

GET    /usage
```

---

# 55. API AUTHENTICATION

Support:

```text
Bearer tokens
```

Example:

```http
Authorization: Bearer dc_xxxxxxxxx
```

API token format:

```text
dc_live_xxxxxxxxxxxxxxxxx
```

Store only hashes of long-lived tokens.

---

# 56. CLI

Create:

```text
@deploycloud/cli
```

Commands:

```bash
deploycloud login
deploycloud logout

deploycloud init

deploycloud deploy

deploycloud deploy --prod

deploycloud projects

deploycloud deployments

deploycloud logs

deploycloud env pull

deploycloud env push

deploycloud domains

deploycloud domains add example.com
```

Example:

```bash
deploycloud deploy
```

Output:

```text
Deploying project...

✓ Uploaded
✓ Build started
✓ Build completed
✓ Deployment ready

Production:
https://shop.deploycloud.app
```

---

# 57. CLI LINKING

```bash
deploycloud link
```

creates:

```text
.deploycloud/project.json
```

Example:

```json
{
  "projectId": "prj_123",
  "teamId": "team_123"
}
```

---

# 58. GIT DEPLOYMENT

Support automatic:

```text
push → deployment
```

Configuration:

```text
Production Branch: main
Preview Branches: all
```

---

# 59. TEAM MANAGEMENT

Team page:

```text
Members

Alex
Owner

John
Admin

Sarah
Member

Mike
Viewer
```

Actions:

```text
Invite
Remove
Change role
Resend invitation
```

---

# 60. TEAM INVITATIONS

Email:

```text
You have been invited to join Acme Team

[Accept invitation]
```

Token must be:

```text
random
single-use
expiring
hashed at rest
```

---

# 61. AUDIT LOGS

Record:

```text
User
Action
Resource
Timestamp
IP
User Agent
```

Example:

```text
Alex deployed shop
2 minutes ago

John changed DATABASE_URL
1 hour ago

Sarah added example.com
3 hours ago
```

---

# 62. NOTIFICATIONS

Support:

```text
In-app
Email
Webhook
```

Events:

```text
Deployment ready
Deployment failed
Domain verified
Certificate expiring
Team invitation
Billing event
Usage limit
```

---

# 63. SEARCH

Global search should search:

```text
Projects
Deployments
Domains
Teams
Functions
Logs
Settings
Documentation
```

Command:

```text
Ctrl/Cmd + K
```

---

# 64. BILLING

Plans should be configurable.

Example:

```text
Hobby
Pro
Team
Enterprise
```

Do not hardcode prices throughout the codebase.

Use:

```text
plans
plan_features
subscriptions
usage_records
```

---

# 65. USAGE METERING

Track:

```text
Build minutes
Function invocations
Function compute time
Bandwidth
Storage
Analytics events
Team members
Deployments
```

Example:

```text
Usage this month

Build:
142 min

Bandwidth:
34 GB

Function:
128,421 invocations

Storage:
4.2 GB
```

---

# 66. STRIPE

If billing is enabled, integrate Stripe.

Flows:

```text
Create checkout
Subscribe
Upgrade
Downgrade
Cancel
Invoice
Payment failure
Webhook
```

Webhook:

```http
POST /api/webhooks/stripe
```

Verify signature.

---

# 67. SECURITY

Implement:

```text
Rate limiting
CSRF protection
CORS
Content Security Policy
Secure cookies
Password hashing
Encryption
Audit logs
RBAC
API token hashing
Webhook signatures
Input validation
SQL parameterization
Command injection prevention
Path traversal protection
SSRF protection
Build isolation
Container isolation
Resource limits
```

---

# 68. SSRF PROTECTION

This is mandatory.

Any feature that fetches external URLs must validate:

```text
Private IP ranges
Loopback
Link-local
Cloud metadata addresses
Internal DNS
IPv6 private ranges
Redirect targets
```

Never allow arbitrary server-side URL fetching without validation.

---

# 69. BUILD SECURITY

User-controlled build scripts are untrusted code.

Builder must have:

```text
No host filesystem access
No Docker socket
No Kubernetes credentials
No cloud credentials
No internal service credentials
No metadata service access
Network disabled or allowlisted
Resource limits
Timeout
Ephemeral filesystem
```

---

# 70. ADMIN DASHBOARD

Route:

```text
/admin
```

Admin features:

```text
Users
Teams
Projects
Deployments
Build workers
Runtime workers
Domains
Certificates
Storage
Usage
Billing
Feature flags
System health
Audit logs
Queues
```

Dashboard:

```text
Users             12,842
Teams              2,481
Projects          18,291
Deployments       91,284

Build Queue          12
Runtime Instances    34

System:
● Healthy
```

---

# 71. SYSTEM HEALTH

Monitor:

```text
API
Database
Redis
Object Storage
Build Queue
Runtime Queue
Router
DNS
Certificates
```

Health endpoint:

```http
GET /health
```

Response:

```json
{
  "status": "healthy",
  "services": {
    "database": "healthy",
    "redis": "healthy",
    "storage": "healthy",
    "queue": "healthy"
  }
}
```

---

# 72. RATE LIMITING

Implement Redis-based rate limiting.

Examples:

```text
Login:
10 requests / minute / IP

API:
100 requests / minute / token

Deployment:
20 deployments / hour / project
```

Make limits configurable.

---

# 73. API DOCUMENTATION

Generate OpenAPI.

Route:

```text
/docs
```

Provide:

```text
OpenAPI JSON
Swagger UI
API examples
Authentication documentation
Error documentation
SDK examples
```

---

# 74. ERROR FORMAT

Every API error should use:

```json
{
  "error": {
    "code": "PROJECT_NOT_FOUND",
    "message": "Project does not exist.",
    "requestId": "req_123"
  }
}
```

Never expose stack traces to users in production.

---

# 75. REQUEST IDs

Every request gets:

```text
x-request-id
```

Example:

```text
req_01j2k3...
```

Propagate it through:

```text
API
Queue
Worker
Build
Logs
Database
```

---

# 76. LOGGING

Use structured JSON logs.

Example:

```json
{
  "level": "info",
  "service": "deployment-worker",
  "deploymentId": "dpl_123",
  "requestId": "req_123",
  "message": "Build completed"
}
```

---

# 77. MONITORING

Expose Prometheus metrics.

Examples:

```text
deployments_total
deployment_duration_seconds
build_queue_size
function_invocations_total
function_errors_total
http_requests_total
http_request_duration_seconds
```

---

# 78. TRACING

Use OpenTelemetry.

Trace:

```text
HTTP request
 ↓
API
 ↓
Database
 ↓
Queue
 ↓
Worker
 ↓
Storage
```

---

# 79. DESIGN SYSTEM

Create a reusable component system.

Components:

```text
Button
Input
Textarea
Select
Combobox
Dialog
Drawer
Dropdown
Tabs
Table
DataTable
Badge
Tooltip
Toast
Alert
Card
CodeBlock
LogViewer
Terminal
CommandMenu
Sidebar
Breadcrumb
Pagination
DatePicker
Chart
EmptyState
Skeleton
```

---

# 80. LOG VIEWER

Build a high-quality terminal-like log interface.

Requirements:

```text
Virtualized rendering
Auto-scroll
Pause
Resume
Search
Copy
Download
Timestamp
Log level
Filter
```

Example:

```text
10:41:01 INFO  Cloning repository
10:41:05 INFO  Installing dependencies
10:41:19 INFO  Building project
10:41:38 INFO  Build successful
10:41:40 INFO  Deployment ready
```

---

# 81. PROJECT SETTINGS

Tabs:

```text
General
Build & Deployment
Environment Variables
Domains
Functions
Git
Integrations
Cron
Webhooks
Security
Usage
Danger Zone
```

Danger Zone:

```text
Delete Project
```

Require confirmation.

---

# 82. PROJECT GIT SETTINGS

Show:

```text
Repository
Branch
Git Provider
Auto Deploy
Ignored Build Step
Root Directory
```

Example:

```text
Repository:
github.com/acme/shop

Production Branch:
main

Automatic deployments:
Enabled
```

---

# 83. IGNORE BUILD STEP

Allow command:

```bash
git diff --quiet HEAD^ HEAD ./frontend
```

If command indicates no relevant changes:

```text
Deployment skipped
```

---

# 84. BUILD CACHE

Support caching:

```text
node_modules
pnpm store
npm cache
build cache
framework cache
```

Cache key:

```text
project + lockfile hash + runtime version
```

Invalidate safely when dependencies change.

---

# 85. BUILD ARTIFACTS

Artifact metadata:

```json
{
  "deploymentId": "dpl_123",
  "size": 1832912,
  "files": 241,
  "checksum": "sha256:..."
}
```

Use checksums.

---

# 86. DEPLOYMENT MANIFEST

Create:

```text
manifest.json
```

Example:

```json
{
  "version": 1,
  "deploymentId": "dpl_123",
  "framework": "nextjs",
  "routes": [],
  "assets": [],
  "functions": []
}
```

---

# 87. ROUTING CONFIGURATION

Support project routing configuration.

Example:

```json
{
  "redirects": [
    {
      "source": "/old",
      "destination": "/new",
      "statusCode": 308
    }
  ],
  "headers": [
    {
      "source": "/api/:path*",
      "headers": {
        "Cache-Control": "no-store"
      }
    }
  ]
}
```

---

# 88. HTTP HEADERS

Allow project headers.

Examples:

```text
Cache-Control
X-Frame-Options
Content-Security-Policy
Referrer-Policy
Permissions-Policy
```

---

# 89. EDGE MIDDLEWARE

Provide a middleware layer for request processing.

Example:

```ts
export default function middleware(request) {
  if (!request.headers.get("authorization")) {
    return Response.redirect("/login")
  }
}
```

Keep execution constrained and secure.

---

# 90. CACHING

Support:

```text
Browser cache
CDN cache
Server cache
Function cache
Build cache
```

Cache invalidation:

```text
deployment
manual purge
TTL
tag
path
```

---

# 91. IMAGE OPTIMIZATION

Provide image transformation service.

Input:

```text
/image?url=https://example.com/a.jpg&w=1200&q=80
```

Output:

```text
Optimized WebP/AVIF image
```

Implement:

```text
width
height
quality
format
fit
```

Protect against SSRF.

---

# 92. EDGE CDN

Architecture:

```text
User
 ↓
CDN
 ↓
Router
 ↓
Deployment
```

Cache static files.

Do not proxy all dynamic traffic through cache.

---

# 93. CUSTOM ERROR PAGES

Provide:

```text
404
500
503
Deployment not found
Project suspended
Domain not configured
```

Example:

```text
404

This deployment could not find
the requested resource.

Request ID:
req_123
```

---

# 94. MAINTENANCE MODE

Admin can enable:

```text
Maintenance mode
```

Output:

```text
DeployCloud is temporarily unavailable.

We are performing maintenance.
```

---

# 95. FEATURE FLAGS

Create:

```text
feature_flags
```

Examples:

```text
functions
analytics
storage
ai_gateway
sandbox
```

Allow:

```text
global
team
user
```

targeting.

---

# 96. INTEGRATIONS

Integration system:

```text
Integration
 ↓
Installation
 ↓
OAuth
 ↓
Scopes
 ↓
Credentials
 ↓
Project connection
```

Initial integrations:

```text
GitHub
GitLab
Slack
Discord
Sentry
PostgreSQL
Redis
Stripe
Cloudflare
AWS
```

---

# 97. INTEGRATION PAGE

Show:

```text
GitHub
Connected

Slack
Not connected

Sentry
Connected

Stripe
Not connected
```

---

# 98. MARKETPLACE

Create:

```text
/marketplace
```

Categories:

```text
Databases
Monitoring
Authentication
Payments
Email
Storage
AI
Analytics
Security
```

Each integration:

```text
Logo
Name
Description
Category
Install
Documentation
```

---

# 99. AI INFRASTRUCTURE

Create an extensible AI layer.

Features can include:

```text
AI SDK
AI Gateway
Model providers
Streaming
Usage tracking
API keys
Model routing
```

Do not hardcode a single AI vendor.

Provider abstraction:

```ts
interface AIProvider {
  generateText(input: GenerateTextInput): Promise<GenerateTextResult>

  streamText(
    input: GenerateTextInput
  ): AsyncIterable<AIStreamChunk>
}
```

Providers:

```text
OpenAI-compatible
Anthropic-compatible
Google-compatible
Local models
Custom HTTP providers
```

---

# 100. AI GATEWAY

Endpoint:

```http
POST /api/ai/generate
```

Input:

```json
{
  "model": "provider/model",
  "messages": [
    {
      "role": "user",
      "content": "Explain TypeScript generics"
    }
  ]
}
```

Response:

```json
{
  "text": "TypeScript generics allow..."
}
```

Track:

```text
provider
model
tokens
latency
cost
status
```

---

# 101. SANDBOX

Provide optional isolated compute environments.

Use:

```text
microVM
container
Kubernetes
```

Users can create temporary environments.

Example:

```text
Create Sandbox

Runtime:
Node.js

CPU:
2

Memory:
4 GB

Lifetime:
30 minutes
```

---

# 102. WORKFLOW ENGINE

Implement a durable workflow abstraction.

Example:

```ts
const workflow = defineWorkflow("send-report", async () => {
  const data = await step("fetch-data", fetchData)

  await step("generate-report", () => generateReport(data))

  await step("send-email", () => sendEmail(data))
})
```

Store state so workflows can resume after failure.

---

# 103. QUEUES

Provide project queues.

Example:

```text
Queue:
email-processing

Messages:
12,381

Consumers:
4
```

API:

```ts
await queue.send({
  type: "email",
  payload: {
    userId: "123"
  }
})
```

---

# 104. BACKGROUND JOBS

Use BullMQ initially.

Jobs:

```text
deployment
build
email
analytics aggregation
certificate renewal
backup
cron
webhook delivery
```

---

# 105. RETRIES

Every asynchronous job should support:

```text
max attempts
backoff
dead letter queue
manual retry
```

Example:

```text
Attempt 1
 ↓
Failed
 ↓
10 seconds
 ↓
Attempt 2
 ↓
Failed
 ↓
30 seconds
 ↓
Attempt 3
```

---

# 106. IDE / CODE EDITOR

Where configuration editing is useful, use Monaco.

Example:

```text
Environment JSON
Deployment configuration
Workflow editor
Function editor
```

---

# 107. DOCUMENTATION WEBSITE

Create:

```text
/docs
```

Structure:

```text
Introduction
Getting Started
Projects
Deployments
Git
Domains
Environment Variables
Functions
Storage
Analytics
Cron
Queues
API
CLI
SDK
Security
Billing
Troubleshooting
```

---

# 108. DOCUMENTATION SEARCH

Use:

```text
Cmd/Ctrl + K
```

Search documentation.

Example:

```text
Search:
environment variables
```

Results:

```text
Environment Variables
Using Environment Variables
Secret Management
CLI Environment Variables
```

---

# 109. LANDING PAGE

Create a premium landing page.

Sections:

```text
Hero
Deployment demo
Features
Git integration
Preview deployments
Functions
Analytics
Storage
Security
Teams
AI infrastructure
Pricing
FAQ
CTA
Footer
```

Hero:

```text
Ship software without managing infrastructure.

Connect your repository.
Deploy in seconds.
Scale automatically.
```

CTA:

```text
Start Deploying
Explore Documentation
```

---

# 110. LIVE DEPLOYMENT DEMO

Landing page should visually demonstrate:

```text
git push
 ↓
Build
 ↓
Deployment
 ↓
Live URL
```

Animate terminal logs.

Example:

```text
$ deploycloud deploy

✓ Detecting Next.js
✓ Installing dependencies
✓ Building
✓ Uploading
✓ Deployment ready

https://demo.deploycloud.app
```

---

# 111. PRICING PAGE

Route:

```text
/pricing
```

Plan cards:

```text
Hobby
Pro
Team
Enterprise
```

Features should be generated from configuration.

Never hardcode billing logic into React components.

---

# 112. RESPONSIVE DESIGN

Support:

```text
Mobile
Tablet
Desktop
Large desktop
```

Dashboard mobile navigation:

```text
Bottom navigation
```

or:

```text
Collapsible sidebar
```

Do not simply shrink desktop UI.

---

# 113. ACCESSIBILITY

Implement:

```text
Keyboard navigation
Focus states
ARIA labels
Screen reader support
Reduced motion
Semantic HTML
```

Command menu must be keyboard accessible.

---

# 114. DARK / LIGHT MODE

Support:

```text
Dark
Light
System
```

Persist preference.

Avoid flashing incorrect theme during initial load.

---

# 115. PERFORMANCE

Target:

```text
Lighthouse > 90
Fast dashboard navigation
Minimal JS
Code splitting
Image optimization
Server components where appropriate
Streaming
Caching
```

Do not sacrifice usability for artificial performance scores.

---

# 116. TESTING

Create:

```text
Unit tests
Integration tests
API tests
Component tests
E2E tests
Security tests
Load tests
```

Use:

```text
Vitest
Playwright
Testing Library
```

---

# 117. CRITICAL E2E TEST

The following test must work:

```text
Signup
 ↓
Create team
 ↓
Connect GitHub
 ↓
Import repository
 ↓
Create project
 ↓
Configure variables
 ↓
Deploy
 ↓
Build succeeds
 ↓
Deployment URL generated
 ↓
Open URL
 ↓
Production page loads
```

---

# 118. DEPLOYMENT FAILURE TEST

Create a test project whose build intentionally fails.

Expected:

```text
Deployment failed

Build command exited with code 1

View logs
Retry
```

The API server must remain healthy.

---

# 119. ROLLBACK TEST

Create:

```text
deployment A
deployment B
```

Promote B.

Then rollback to A.

Expected:

```text
Production alias → A
```

without rebuilding A.

---

# 120. DOMAIN TEST

Test:

```text
Add domain
Verify domain
Create certificate
Attach domain
Open HTTPS
```

---

# 121. API TEST

Test:

```bash
curl \
  -H "Authorization: Bearer dc_live_xxx" \
  https://api.deploycloud.app/v1/projects
```

Expected:

```json
{
  "data": []
}
```

---

# 122. CLI TEST

Run:

```bash
deploycloud login

deploycloud init

deploycloud deploy
```

Expected:

```text
✓ Project linked
✓ Deployment created
✓ Build complete
✓ URL available
```

---

# 123. ERROR HANDLING

Never allow:

```text
Unhandled Promise Rejection
```

or:

```text
White screen
```

User-facing errors must provide:

```text
What happened
Why
Request ID
Possible solution
Retry action
```

---

# 124. EMPTY STATES

Every list needs an empty state.

Example:

```text
No deployments yet.

Push your repository or deploy
using the CLI to create your first deployment.

[Create Deployment]
```

---

# 125. LOADING STATES

Every asynchronous screen needs:

```text
Skeleton
Loading indicator
Progress
```

Never freeze the interface.

---

# 126. REAL-TIME DASHBOARD

Deployment status should update without page refresh.

Example:

```text
QUEUED
 ↓
BUILDING
 ↓
READY
```

UI updates automatically.

---

# 127. NOTIFICATION CENTER

Bell icon:

```text
3
```

Notifications:

```text
Deployment ready
Deployment failed
Domain verified
Team invitation
Usage warning
```

---

# 128. PROJECT OVERVIEW

Display:

```text
Project
Environment
Production URL
Latest deployment
Recent deployments
Runtime status
Bandwidth
Functions
Domains
```

---

# 129. DEPLOYMENT TIMELINE

Display:

```text
Created
 ↓
Queued
 ↓
Build started
 ↓
Build completed
 ↓
Uploaded
 ↓
Ready
```

Each event:

```text
timestamp
duration
status
```

---

# 130. ENVIRONMENT SWITCHER

Deployment UI:

```text
Production
Preview
Development
```

When selected, all relevant:

```text
Deployments
Variables
Domains
Analytics
Logs
```

change context.

---

# 131. PROJECT TEMPLATES

Provide templates:

```text
Next.js
React
Vite
Astro
Node API
Python API
Static HTML
```

Template flow:

```text
Choose template
 ↓
Create repository
 ↓
Create project
 ↓
Deploy
```

---

# 132. ONE-CLICK DEPLOY

Template page:

```text
Next.js SaaS Starter

[Deploy]
```

Click:

```text
Repository created
 ↓
Project created
 ↓
Deployment started
```

---

# 133. IMPORT FROM ZIP

Allow:

```text
Upload ZIP
```

Flow:

```text
ZIP
 ↓
Extract
 ↓
Security scan
 ↓
Framework detection
 ↓
Build
 ↓
Deploy
```

Never extract archives without preventing:

```text
Zip Slip
path traversal
symlink attacks
```

---

# 134. GIT CLONE SECURITY

Validate:

```text
Repository URL
Provider
OAuth token
Redirects
Git protocol
```

Never allow arbitrary credential leakage.

---

# 135. SECRETS

Secrets must:

```text
Encrypted at rest
Masked in UI
Masked in logs
Never included in analytics
Never sent to frontend unnecessarily
```

---

# 136. BACKUPS

Backup:

```text
PostgreSQL
Deployment metadata
Configuration
Billing metadata
```

Do not backup ephemeral build workspaces.

---

# 137. DATA RETENTION

Make configurable:

```text
Build logs
Runtime logs
Analytics
Audit logs
Deployments
Artifacts
```

Example:

```text
Logs:
30 days

Analytics:
90 days

Audit logs:
365 days
```

---

# 138. DELETE / DATA SAFETY

Deletion should use:

```text
soft delete
confirmation
audit record
async cleanup
```

For destructive actions:

```text
Type project name to confirm
```

---

# 139. MULTI-TENANCY

Every resource must be scoped.

Example:

```text
team_id
project_id
```

Never query resources without authorization context.

Use:

```text
team ownership
project membership
RBAC
```

---

# 140. TENANT SECURITY TEST

Explicitly test:

```text
User A cannot access User B's project
User A cannot read User B's env vars
User A cannot access User B's logs
User A cannot access User B's artifacts
```

---

# 141. API PAGINATION

Use cursor pagination.

Example:

```http
GET /api/v1/deployments?limit=25&cursor=abc
```

Response:

```json
{
  "data": [],
  "pagination": {
    "nextCursor": "xyz"
  }
}
```

---

# 142. SORTING / FILTERING

Deployments:

```text
status
branch
environment
createdAt
creator
```

Projects:

```text
name
framework
status
updatedAt
```

---

# 143. DATABASE INDEXING

Create indexes for:

```text
team_id
project_id
deployment_id
created_at
status
branch
environment
domain
email
```

Use query analysis for expensive queries.

---

# 144. CACHING

Redis cache:

```text
project metadata
team metadata
deployment status
DNS verification
rate limits
sessions where appropriate
```

Never cache secrets accidentally.

---

# 145. BACKGROUND CLEANUP

Workers should clean:

```text
expired sessions
expired invitations
old logs
old artifacts
expired certificates
failed jobs
temporary sandboxes
```

---

# 146. DEPLOYMENT CONCURRENCY

Prevent deployment race conditions.

Example:

```text
Deployment A
Deployment B
Deployment C
```

If B completes after C, C must remain production if C is newer according to deployment policy.

Use:

```text
optimistic locking
deployment sequence
transactional alias update
```

---

# 147. IDEMPOTENCY

Deployment creation API must support:

```http
Idempotency-Key: abc123
```

Repeated request should not create duplicate deployment.

---

# 148. WEBHOOK IDEMPOTENCY

Store:

```text
provider_event_id
```

and reject duplicate events.

---

# 149. ASYNC ARCHITECTURE

Never make the API wait for:

```text
Build
Upload
Certificate
Analytics aggregation
Webhook retry
```

Use queues.

---

# 150. BUILD TIMEOUT

Example:

```text
Maximum build time:
30 minutes
```

Configurable by plan.

On timeout:

```text
BUILD_TIMEOUT
```

and clean resources.

---

# 151. RUNTIME TIMEOUT

Functions should terminate after configured timeout.

Example:

```text
60 seconds
```

Do not allow abandoned processes.

---

# 152. RESOURCE ACCOUNTING

Track:

```text
CPU
RAM
Disk
Network
Build duration
Function duration
```

Use usage events:

```json
{
  "teamId": "team_123",
  "resource": "build_minutes",
  "quantity": 4.2
}
```

---

# 153. ADMIN QUEUE VIEW

Show:

```text
Queue

deployment-build:
12 waiting

function-runtime:
4 waiting

webhook:
23 waiting
```

Actions:

```text
Pause
Resume
Retry
Drain
```

---

# 154. WORKER DASHBOARD

Show:

```text
Worker
Status
CPU
Memory
Jobs
Last heartbeat
```

Example:

```text
builder-03
● Healthy
CPU 42%
RAM 63%
Jobs 14
```

---

# 155. WORKER HEARTBEATS

Every worker sends:

```text
heartbeat
```

If heartbeat expires:

```text
mark unhealthy
reschedule jobs
```

---

# 156. DEPLOYMENT LOCKS

Prevent:

```text
two simultaneous production promotions
```

Use database transaction/advisory lock.

---

# 157. RATE-LIMITED DEPLOYMENT

Example:

```text
10 deployments/minute/team
```

If exceeded:

```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Too many deployments."
  }
}
```

---

# 158. SECURITY HEADERS

Default:

```text
Strict-Transport-Security
X-Content-Type-Options
Referrer-Policy
Permissions-Policy
```

Allow project overrides where safe.

---

# 159. CONTENT SECURITY

Dashboard should use:

```text
CSP
Trusted Types where practical
Nonce-based scripts
```

Avoid unsafe inline scripts.

---

# 160. FILE UPLOAD SECURITY

For uploaded files:

```text
Size limits
MIME validation
Extension validation
Virus scanning hook
Path sanitization
Zip bomb protection
Zip Slip protection
```

---

# 161. API SDK

Generate TypeScript SDK.

Example:

```ts
import { DeployCloud } from "@deploycloud/sdk"

const client = new DeployCloud({
  token: process.env.DEPLOYCLOUD_TOKEN
})

const projects = await client.projects.list()
```

---

# 162. PYTHON SDK

Later provide:

```python
from deploycloud import Client

client = Client(token="dc_live_xxx")

projects = client.projects.list()
```

---

# 163. WEBHOOK SDK

Provide signature verification:

```ts
verifyWebhookSignature({
  payload,
  signature,
  secret
})
```

---

# 164. ERROR CODES

Centralize:

```text
AUTH_REQUIRED
FORBIDDEN
PROJECT_NOT_FOUND
DEPLOYMENT_NOT_FOUND
DOMAIN_NOT_FOUND
INVALID_ENVIRONMENT
BUILD_FAILED
BUILD_TIMEOUT
FUNCTION_TIMEOUT
RATE_LIMITED
INVALID_TOKEN
INVALID_WEBHOOK
RESOURCE_LIMIT
```

---

# 165. INTERNATIONALIZATION

Architecture should support:

```text
English
Hindi
```

Do not hardcode user-facing text throughout components.

---

# 166. TIMEZONE

Store timestamps as:

```text
UTC
```

Display in user's timezone.

---

# 167. DATE FORMAT

Use locale-aware formatting.

Example:

```text
Sep 27, 2026
```

Relative:

```text
3 minutes ago
```

---

# 168. ACCESS LOGS

For security-sensitive actions:

```text
login
logout
password change
API token creation
domain change
environment variable change
deployment
project deletion
team role change
```

---

# 169. API TOKEN MANAGEMENT

Page:

```text
Settings → Tokens
```

Create:

```text
Name:
CI/CD

Scope:
Project deployment

Expiration:
90 days
```

Show token only once.

---

# 170. TOKEN SCOPES

Examples:

```text
projects:read
projects:write
deployments:read
deployments:write
domains:read
domains:write
env:read
env:write
teams:read
teams:write
```

Prefer least privilege.

---

# 171. SERVICE TOKENS

Support project-specific deployment tokens.

Useful for:

```text
CI/CD
external automation
deploy hooks
```

---

# 172. PROJECT ACCESS TOKENS

Allow:

```text
read-only
deployment
full
```

---

# 173. ADMIN IMPERSONATION

If implemented, make it:

```text
explicit
audited
time-limited
reason-required
```

---

# 174. SUPPORT SYSTEM

Provide:

```text
Help
Documentation
Contact support
Status
```

---

# 175. STATUS PAGE

Create:

```text
/status
```

Components:

```text
API
Dashboard
Deployments
Builds
Functions
Storage
DNS
```

Example:

```text
All systems operational
```

---

# 176. INCIDENT SYSTEM

Admin can create incident:

```text
Investigating
Identified
Monitoring
Resolved
```

Display status page.

---

# 177. MAINTENANCE WINDOWS

Admin can schedule:

```text
Start
End
Affected services
Message
```

---

# 178. DEPLOYMENT COMMENTS

Allow internal project comments:

```text
Deployment dpl_123

Alex:
"Testing payment changes."

John:
"Looks good."
```

---

# 179. DEPLOYMENT LABELS

Allow:

```text
production
staging
release
hotfix
```

---

# 180. ENVIRONMENT ALIASES

Support:

```text
production
preview
development
staging
```

Custom environments should be architecturally possible.

---

# 181. STAGING

Allow:

```text
staging.example.com
```

to point at a specific deployment/environment.

---

# 182. DATABASE MIGRATIONS

Use migration files.

Commands:

```bash
pnpm db:migrate
pnpm db:generate
pnpm db:seed
```

Seed:

```text
Admin user
Demo team
Demo project
Demo deployments
```

---

# 183. DEVELOPMENT DEMO MODE

Provide demo data.

Dashboard should look populated after:

```bash
pnpm db:seed
```

But clearly separate demo data from production data.

---

# 184. DEMO PROJECT

Create:

```text
demo-next-app
```

with:

```text
READY deployment
FAILED deployment
PREVIEW deployment
```

This helps test UI states.

---

# 185. COMPONENT STATES

Every major component must support:

```text
Loading
Empty
Success
Error
Disabled
Permission denied
```

---

# 186. TABLE DESIGN

Tables should support:

```text
sorting
filtering
pagination
column visibility
row actions
bulk actions
```

---

# 187. MOBILE DASHBOARD

Mobile:

```text
Projects
Deployments
Logs
Settings
```

Use bottom navigation or compact navigation.

---

# 188. COMMAND MENU ACTIONS

Example:

```text
> Create project
> Deploy project
> Open logs
> Search projects
> Search deployments
> Switch team
> Open settings
> Add domain
```

---

# 189. KEYBOARD SHORTCUTS

Support:

```text
Cmd/Ctrl + K
G then P → Projects
G then D → Deployments
G then S → Settings
```

Show shortcuts in command menu.

---

# 190. TOASTS

Examples:

```text
Deployment started
Domain added
Environment variable updated
Project deleted
Rollback successful
```

Do not use toasts for critical errors that require user action.

---

# 191. CONFIRMATION DIALOGS

Dangerous actions:

```text
Delete project
Delete domain
Delete deployment
Remove team member
Rotate credentials
```

Require confirmation.

---

# 192. ACCESS CONTROL

Every server endpoint should follow:

```text
Authenticate
 ↓
Identify team
 ↓
Check membership
 ↓
Check permission
 ↓
Perform action
```

---

# 193. OBJECT STORAGE SECURITY

Bucket objects should not automatically become public.

Support:

```text
private
public
signed URL
```

---

# 194. CDN CACHE PURGING

API:

```http
POST /api/projects/:id/cache/purge
```

Input:

```json
{
  "paths": [
    "/products/*"
  ]
}
```

---

# 195. IMAGE CACHE

Cache optimized images.

Key:

```text
source + width + height + quality + format
```

---

# 196. ANALYTICS PRIVACY

Avoid storing:

```text
raw passwords
authentication tokens
unnecessary PII
secret environment variables
```

Provide configurable retention.

---

# 197. DATA EXPORT

User can export:

```text
Projects
Deployments
Analytics
Audit logs
Billing
```

Format:

```text
JSON
CSV
```

---

# 198. ACCOUNT DELETION

Flow:

```text
Settings
 ↓
Delete account
 ↓
Confirm email
 ↓
Confirm password
 ↓
Schedule deletion
```

Allow configurable grace period.

---

# 199. TEAM DELETION

Owner-only.

Require:

```text
Type team slug
```

Then:

```text
projects
domains
deployments
storage
```

must be cleaned asynchronously.

---

# 200. ARCHITECTURE RULES

Never:

```text
Put business logic directly in React components
Put secrets in frontend
Run builds inside API
Trust client authorization
Store passwords plaintext
Expose database directly
Run arbitrary containers without isolation
Use synchronous deployment requests
```

---

# 201. SERVICE BOUNDARIES

Keep:

```text
Auth
Projects
Deployments
Builds
Domains
Functions
Storage
Analytics
Billing
```

as independent modules.

They may initially run in the same Node process, but interfaces should allow extraction into separate services.

---

# 202. API LAYER

Recommended structure:

```text
Controller
 ↓
Service
 ↓
Repository
 ↓
Database
```

Example:

```ts
router.post("/projects", createProjectController)

async function createProjectController(req, reply) {
  const input = createProjectSchema.parse(req.body)

  const project = await projectService.create({
    actor: req.user,
    input
  })

  return reply.send(project)
}
```

---

# 203. VALIDATION

Use Zod.

Example:

```ts
const createProjectSchema = z.object({
  name: z.string().min(1).max(100),
  framework: z.string().optional(),
  repositoryId: z.string().optional()
})
```

---

# 204. DATABASE TRANSACTIONS

Use transactions for:

```text
project creation
deployment creation
production promotion
team membership changes
billing state changes
domain assignment
```

---

# 205. DISTRIBUTED LOCKING

Use:

```text
PostgreSQL advisory locks
```

or:

```text
Redis locks
```

for:

```text
certificate renewal
production promotion
cleanup
```

---

# 206. DEPLOYMENT ARTIFACT INTEGRITY

Generate:

```text
SHA-256 checksum
```

for artifacts.

Store:

```text
checksum
size
createdAt
```

---

# 207. DEPLOYMENT RETENTION

Configurable by plan.

Example:

```text
Hobby:
20 deployments

Pro:
100 deployments

Team:
500 deployments
```

Do not hardcode these numbers.

---

# 208. DOMAIN ROUTING

Routing should support:

```text
project.deploycloud.app
custom.example.com
branch.project.deploycloud.app
deployment-id.deploycloud.app
```

Normalize hosts before lookup.

---

# 209. HOST HEADER SECURITY

Never blindly use:

```text
Host
```

to construct internal URLs.

Validate against known domains and aliases.

---

# 210. DNS VERIFICATION

Support:

```text
TXT verification
CNAME verification
```

Do not consider a domain verified merely because the user entered it.

---

# 211. CERTIFICATE SECURITY

Private keys:

```text
encrypted
restricted
never returned through normal API
```

Renew certificates automatically.

---

# 212. BUILD ENVIRONMENT

Provide:

```text
Node
npm
pnpm
yarn
Python
pip
Go
Rust
```

Make builder images versioned.

Example:

```text
deploycloud/builder:node22
deploycloud/builder:python312
```

---

# 213. BUILD CACHE SECURITY

Never allow one tenant's private build cache to expose another tenant's data.

Cache namespace:

```text
team/project/cache-key
```

with access checks.

---

# 214. BUILD OUTPUT VALIDATION

Validate:

```text
artifact size
path
symlink
permissions
file count
```

Reject dangerous artifacts.

---

# 215. SYMLINK SECURITY

Do not allow build artifacts to escape artifact root through symlinks.

---

# 216. PATH TRAVERSAL SECURITY

Reject:

```text
../
..\
absolute filesystem paths
```

when processing user-provided file paths.

---

# 217. ZIP SECURITY

Protect against:

```text
Zip Slip
Zip bombs
symlink traversal
huge file counts
huge decompressed size
```

---

# 218. SSRF SECURITY

Protect:

```text
image optimizer
webhooks
domain verification
URL import
Git clone
external integrations
```

against SSRF.

---

# 219. WEBHOOK DELIVERY

Store:

```text
event
endpoint
attempt
response
duration
status
```

Allow:

```text
Retry
Replay
Disable
```

---

# 220. WEBHOOK RETRY

Use:

```text
1s
5s
30s
2m
10m
```

with jitter.

---

# 221. EMAIL SYSTEM

Use provider abstraction:

```ts
interface EmailProvider {
  send(input: EmailMessage): Promise<void>
}
```

Support:

```text
SMTP
Resend
SendGrid
SES
```

---

# 222. EMAIL TEMPLATES

Create:

```text
verification
password reset
team invitation
deployment ready
deployment failed
billing
domain verification
certificate expiry
```

---

# 223. SEARCH INDEX

Start with PostgreSQL full-text search.

Later:

```text
OpenSearch
Meilisearch
Typesense
```

---

# 224. LOG INDEX

For initial implementation:

```text
PostgreSQL
```

At scale:

```text
Loki
OpenSearch
ClickHouse
```

---

# 225. ANALYTICS DATABASE

Start with:

```text
PostgreSQL
```

At scale:

```text
ClickHouse
```

Keep analytics repository abstracted.

---

# 226. HIGH-SCALE ARCHITECTURE

When scaling:

```text
             CDN
              |
         Edge Router
              |
         API Gateway
              |
    +---------+----------+
    |         |          |
 Projects Deployments Billing
    |         |
    |       Queue
    |         |
    |      Workers
    |         |
    +---- Storage
```

Use horizontal scaling.

---

# 227. KUBERNETES

Provide manifests for:

```text
api
web
worker
builder
router
postgres
redis
minio
```

For production:

```text
PostgreSQL managed service
Redis managed service
Object storage
Kubernetes
Ingress
Cert Manager
Prometheus
Loki
```

---

# 228. TERRAFORM

Create optional Terraform modules for:

```text
Kubernetes
Object storage
PostgreSQL
Redis
DNS
Load balancer
```

---

# 229. CI/CD

Use GitHub Actions.

Pipeline:

```text
lint
 ↓
typecheck
 ↓
unit tests
 ↓
integration tests
 ↓
build
 ↓
security scan
 ↓
docker build
 ↓
deploy
```

---

# 230. CODE QUALITY

Use:

```text
ESLint
Prettier
TypeScript strict mode
Husky
lint-staged
```

No:

```text
any
```

unless justified.

---

# 231. COMMIT CONVENTION

Use:

```text
feat:
fix:
refactor:
docs:
test:
chore:
security:
```

---

# 232. SECURITY SCANNING

CI should run:

```text
npm audit
dependency scanning
secret scanning
container scanning
SAST
```

Use configurable tooling.

---

# 233. DEPENDENCY POLICY

Pin important production dependencies.

Regularly update.

Do not blindly install packages just because they simplify one small feature.

---

# 234. CODE ORGANIZATION

Do not create giant files.

Prefer:

```text
components/
services/
repositories/
schemas/
routes/
workers/
utils/
```

---

# 235. FRONTEND ROUTES

Implement:

```text
/
 /login
 /signup
 /dashboard
 /new
 /projects
 /projects/:id
 /projects/:id/deployments
 /projects/:id/domains
 /projects/:id/settings
 /projects/:id/analytics
 /projects/:id/logs
 /domains
 /storage
 /analytics
 /integrations
 /team
 /settings
 /billing
 /docs
 /pricing
 /marketplace
 /status
 /admin
```

---

# 236. PROJECT PAGE NAVIGATION

Project header:

```text
Project name
Production URL
Status
Deploy button
```

Tabs:

```text
Overview
Deployments
Logs
Analytics
Domains
Settings
```

---

# 237. DEPLOY BUTTON

Click:

```text
Deploy
```

opens:

```text
Environment
Branch
Commit
Variables
```

Then:

```text
[Deploy]
```

---

# 238. DEPLOYMENT PROGRESS

Show:

```text
Queued       ✓
Cloning      ✓
Installing   ✓
Building     ●
Uploading
Ready
```

---

# 239. FAILED BUILD UI

Example:

```text
Deployment failed

Build command:
npm run build

Exit code:
1

Error:
Module not found: @/components/Header

[View Logs]
[Retry Deployment]
```

---

# 240. PROJECT PAUSE

Allow admin/team owner to pause project.

Paused project:

```text
Deployments disabled
Functions disabled
```

---

# 241. PROJECT TRANSFER

Allow transfer between teams.

Require:

```text
owner permission
target team permission
confirmation
```

---

# 242. PROJECT DUPLICATION

Allow:

```text
Duplicate project
```

Creates:

```text
new project
same configuration
without production secrets unless explicitly copied
```

---

# 243. ENVIRONMENT VARIABLE IMPORT

Support:

```text
.env
.env.local
```

CLI:

```bash
deploycloud env push
```

---

# 244. ENVIRONMENT VARIABLE EXPORT

CLI:

```bash
deploycloud env pull
```

Protect output.

---

# 245. GITIGNORE

Never upload:

```text
.env
.env.local
node_modules
.git
```

unless explicitly required.

---

# 246. SOURCE ARCHIVE

Build system should clone source temporarily.

Delete after configurable retention.

---

# 247. BUILD LOG PRIVACY

Mask:

```text
API keys
tokens
passwords
known environment variables
```

Use a log redaction pipeline.

---

# 248. SECRET REDACTION

Example:

```text
DATABASE_URL=postgres://...
```

becomes:

```text
DATABASE_URL=[REDACTED]
```

---

# 249. SYSTEM SETTINGS

Admin configuration:

```text
root domain
max build duration
max artifact size
default region
default runtime
email provider
storage provider
billing provider
```

---

# 250. REGION ARCHITECTURE

Support:

```text
india-central
us-east
eu-west
```

Projects can select supported regions.

Do not advertise a region unless infrastructure actually exists there.

---

# 251. REGIONAL DEPLOYMENT

Deployment record:

```json
{
  "region": "india-central"
}
```

Router selects appropriate deployment.

---

# 252. EDGE ROUTING

Eventually support:

```text
User
 ↓
nearest edge
 ↓
regional deployment
```

---

# 253. FAILOVER

For supported production services:

```text
primary region
 ↓
health check
 ↓
secondary region
```

Do not claim automatic failover unless actually implemented.

---

# 254. HEALTH CHECKS

Deployment health:

```text
HTTP status
latency
error rate
```

---

# 255. UPTIME

Track:

```text
availability
downtime
latency
```

---

# 256. STATUS PAGE API

Public endpoint:

```http
GET /api/status
```

Response:

```json
{
  "status": "operational"
}
```

---

# 257. RATE LIMIT DASHBOARD

Users can see:

```text
API requests
Deployment requests
Function requests
```

and current limits.

---

# 258. USAGE ALERTS

Allow:

```text
50%
75%
90%
100%
```

notifications.

---

# 259. BILLING LIMIT ENFORCEMENT

If a paid resource exceeds quota:

```text
warn
throttle
block
```

based on plan configuration.

Never silently delete user data.

---

# 260. ENTERPRISE FEATURES

Architecture should support:

```text
SSO
SAML
SCIM
Audit logs
Advanced RBAC
IP allowlists
Dedicated infrastructure
Custom contracts
Private networking
```

Implement interfaces first if full enterprise infrastructure is unavailable.

Do not create fake functionality.

---

# 261. SSO

Later support:

```text
SAML 2.0
OIDC
```

---

# 262. SCIM

Later:

```text
User provisioning
Deprovisioning
Group synchronization
```

---

# 263. PRIVATE NETWORKING

Architecture placeholder for:

```text
VPC
Private endpoint
Internal services
```

---

# 264. CUSTOM BUILD IMAGES

Allow enterprise projects to select:

```text
custom builder image
```

Only allow trusted registry sources and enforce security controls.

---

# 265. DOCKER DEPLOYMENT

Support Docker-based applications.

Example:

```text
Dockerfile
```

Build:

```bash
docker build -t deployment .
```

Push:

```text
registry
```

Run:

```text
runtime
```

---

# 266. DOCKERFILE SECURITY

Scan:

```text
base image
packages
secrets
user
ports
```

Never expose Docker daemon socket to untrusted builds.

---

# 267. CONTAINER REGISTRY

Optional:

```text
registry.deploycloud.app
```

Features:

```text
repositories
tags
images
digests
vulnerabilities
```

---

# 268. IMAGE DEPLOYMENT

Input:

```text
registry.deploycloud.app/shop:latest
```

Output:

```text
running deployment
```

---

# 269. CUSTOM RUNTIMES

Architecture:

```text
Runtime interface
```

Example:

```ts
interface Runtime {
  prepare(): Promise<void>
  start(): Promise<void>
  stop(): Promise<void>
  health(): Promise<boolean>
}
```

---

# 270. SERVERLESS COLD START METRICS

Track:

```text
coldStart
warmStart
duration
```

---

# 271. FUNCTION CONCURRENCY

Configure:

```text
minimum instances
maximum instances
concurrency
```

---

# 272. FUNCTION AUTOSCALING

Architecture:

```text
Request
 ↓
Queue
 ↓
Load
 ↓
Scale workers
 ↓
Function
```

---

# 273. AUTOSCALING SAFETY

Use:

```text
maximum replicas
maximum concurrency
budget limit
```

to prevent runaway costs.

---

# 274. COST CONTROLS

Display:

```text
Estimated usage
Current usage
Projected usage
```

Do not make billing promises unless based on actual provider costs.

---

# 275. AI COST TRACKING

Track:

```text
provider
model
input tokens
output tokens
cached tokens
latency
cost
```

---

# 276. AI MODEL ROUTING

Allow rules:

```text
cheap
fast
high-quality
private
```

but expose the actual selected provider/model.

Do not misrepresent routing.

---

# 277. MCP SUPPORT

Architect an MCP-compatible API surface for platform resources.

Potential tools:

```text
list_projects
create_project
list_deployments
deploy_project
get_logs
list_domains
```

All actions require authenticated authorization.

---

# 278. AGENT API

Provide machine-readable project/deployment operations.

Example:

```json
{
  "action": "deploy",
  "projectId": "prj_123"
}
```

Return:

```json
{
  "deploymentId": "dpl_123",
  "status": "queued"
}
```

---

# 279. PLATFORM API

Allow external SaaS products to use DeployCloud.

Example:

```text
SaaS customer
 ↓
Create project
 ↓
Deploy
 ↓
Custom domain
 ↓
Runtime
```

---

# 280. MULTI-TENANT PLATFORM MODE

Support:

```text
Platform Owner
 ↓
Customer
 ↓
Customer Project
```

Customer should never access another customer's resources.

---

# 281. CUSTOMER PROVISIONING

API:

```http
POST /api/v1/platform/customers
```

Creates:

```text
team
user
project
```

---

# 282. DOMAIN PROVISIONING API

```http
POST /api/v1/platform/domains
```

---

# 283. PLATFORM EMBEDDING

Dashboard should eventually support:

```text
iframe
SDK
API
white-label
```

---

# 284. WHITE LABEL

Configurable:

```text
logo
name
colors
domain
email branding
```

---

# 285. FEATURE ENTITLEMENTS

Use:

```text
plan
feature
limit
```

Example:

```text
feature:
functions

limit:
100000
```

---

# 286. FEATURE CHECK

Server helper:

```ts
await entitlements.require(
  teamId,
  "functions"
)
```

---

# 287. QUOTA CHECK

```ts
await quota.require(
  teamId,
  "build_minutes",
  requestedAmount
)
```

---

# 288. API VERSIONING

Use:

```text
/v1
/v2
```

Do not break existing APIs.

---

# 289. DEPRECATION

API responses can include:

```text
Deprecation
Sunset
```

headers where appropriate.

---

# 290. CHANGELOG

Create:

```text
/changes
```

or:

```text
CHANGELOG.md
```

Document user-visible changes.

---

# 291. FEATURE FLAGS FOR ROLLOUT

Example:

```text
new-router
ai-gateway
sandbox
storage-v2
```

Roll out:

```text
internal
10%
50%
100%
```

---

# 292. DATABASE SEED

Seed:

```text
Admin
Demo user
Demo team
Demo projects
Demo deployments
Plans
Feature flags
```

---

# 293. DEMO LOGIN

Development-only:

```text
demo@deploycloud.local
```

Never enable demo credentials in production.

---

# 294. LOCAL DOMAIN TESTING

Provide:

```text
*.deploycloud.local
```

using local reverse proxy.

For local HTTPS optionally use:

```text
mkcert
```

---

# 295. PRODUCTION DNS

Production deployment should document:

```text
A
AAAA
CNAME
TXT
```

configuration.

---

# 296. PRODUCTION DEPLOYMENT

Provide:

```text
Docker images
Kubernetes manifests
Terraform
Environment configuration
Migration commands
Backup instructions
Rollback instructions
```

---

# 297. DOCKER COMPOSE SERVICES

At minimum:

```yaml
services:
  web:
  api:
  worker:
  builder:
  router:
  postgres:
  redis:
  minio:
```

---

# 298. DEVELOPMENT COMMANDS

Implement:

```bash
pnpm dev
pnpm build
pnpm test
pnpm lint
pnpm typecheck
pnpm db:migrate
pnpm db:seed
pnpm docker:build
```

---

# 299. HEALTH CHECKS

Every service:

```text
/health
```

or equivalent.

Docker/Kubernetes health checks must be configured.

---

# 300. GRACEFUL SHUTDOWN

Workers must stop accepting jobs before termination.

```text
SIGTERM
 ↓
stop new jobs
 ↓
finish safe jobs
 ↓
close DB
 ↓
close Redis
 ↓
exit
```

---

# 301. DEPLOYMENT WORKER RECOVERY

If worker crashes during build:

```text
job visibility timeout
 ↓
job requeued
 ↓
new worker
```

---

# 302. BUILD CANCELLATION

User clicks:

```text
Cancel
```

Worker:

```text
receive cancellation
 ↓
terminate container
 ↓
release resources
 ↓
mark canceled
```

---

# 303. DEPLOYMENT RETRY

Retry should create a new deployment attempt or a clearly tracked retry depending on architecture.

Never corrupt deployment state.

---

# 304. CONCURRENCY LIMITS

Per:

```text
user
team
project
worker
region
```

---

# 305. QUEUE PRIORITY

Support:

```text
production
preview
background
```

Production may receive higher priority according to plan/configuration.

---

# 306. BUILD PRIORITY

Do not allow users to bypass quotas by manipulating client payloads.

---

# 307. AUDITABLE BILLING

Every billable event should have:

```text
resource
quantity
timestamp
team
project
source
```

---

# 308. BILLING RECONCILIATION

Create jobs that compare:

```text
usage events
provider billing
subscription state
```

and detect discrepancies.

---

# 309. OBSERVABILITY OF DEPLOYCLOUD ITSELF

DeployCloud should monitor itself.

Track:

```text
API latency
DB latency
queue latency
build latency
deployment success rate
function success rate
router latency
storage latency
```

---

# 310. INTERNAL ADMIN METRICS

Dashboard:

```text
Deployment Success Rate
Average Build Time
P95 API Latency
Queue Depth
Error Rate
Active Workers
Storage Usage
```

---

# 311. SECURITY INCIDENT LOG

Admin-only.

Events:

```text
SSRF blocked
Rate limit exceeded
Invalid webhook signature
Authentication abuse
Suspicious upload
Container violation
```

---

# 312. SECURITY EVENT PIPELINE

```text
Security Event
 ↓
Security Logger
 ↓
Audit Store
 ↓
Admin Alert
```

---

# 313. ALERTING

Integrate:

```text
Email
Slack
PagerDuty
Webhook
```

for internal system alerts.

---

# 314. BACKPRESSURE

When queues are overloaded:

```text
queue
 ↓
rate control
 ↓
worker scaling
```

Do not allow unlimited jobs.

---

# 315. DISASTER RECOVERY

Document:

```text
Database restore
Object storage restore
Redis rebuild
Certificate recovery
DNS recovery
Deployment metadata recovery
```

---

# 316. RPO/RTO

Make targets configurable.

Do not claim enterprise SLA unless infrastructure supports it.

---

# 317. DOCUMENTATION REQUIREMENT

Every major feature must have:

```text
README
Architecture documentation
API documentation
Configuration documentation
Security notes
Troubleshooting
```

---

# 318. CODE COMMENTS

Comment:

```text
security-sensitive logic
distributed systems logic
non-obvious algorithms
provider-specific behavior
```

Do not fill obvious code with unnecessary comments.

---

# 319. NO FAKE FEATURES

This is extremely important.

Never implement a button that merely displays:

```text
Coming soon
Success
Connected
Deployed
```

unless the feature actually works.

If an advanced infrastructure feature cannot yet be implemented safely:

```text
Build the backend interface
Document the limitation
Hide/disable the feature
```

Do not fake infrastructure behavior.

---

# 320. IMPLEMENTATION PHASES

Claude Code MUST implement in this order.

## PHASE 1 — FOUNDATION

Implement:

```text
Monorepo
Next.js
API
PostgreSQL
Redis
Docker Compose
Authentication
UI system
Database
Logging
Environment configuration
```

Acceptance:

```bash
pnpm dev
```

works.

## PHASE 2 — DASHBOARD

Implement:

```text
Dashboard
Projects
Teams
Settings
Command menu
Notifications
Dark/light mode
Responsive UI
```

## PHASE 3 — GIT

Implement:

```text
GitHub OAuth/App
Repository listing
Repository selection
Webhooks
Branch detection
```

## PHASE 4 — DEPLOYMENT ENGINE

Implement:

```text
Deployment API
Queue
Worker
Builder
Docker isolation
Framework detection
Build logs
Artifact upload
Deployment states
```

This is the first major milestone.

## PHASE 5 — DEPLOYMENT ROUTER

Implement:

```text
Deployment URLs
Project aliases
Static asset serving
Routing
HTTPS development setup
```

## PHASE 6 — PREVIEW / PRODUCTION

Implement:

```text
Preview deployments
Production deployments
Branch aliases
PR previews
Production promotion
Rollback
```

## PHASE 7 — ENVIRONMENT VARIABLES

Implement:

```text
Development
Preview
Production
Encryption
CLI env commands
Secret redaction
```

## PHASE 8 — DOMAINS

Implement:

```text
Custom domains
DNS
Verification
SSL
Certificates
Redirects
```

## PHASE 9 — FUNCTIONS

Implement:

```text
Node runtime
Python runtime
Function routing
Function workers
Function logs
Function metrics
Timeouts
Concurrency
```

## PHASE 10 — OBSERVABILITY

Implement:

```text
Runtime logs
Metrics
Tracing
Error tracking
OpenTelemetry
Prometheus
Loki
```

## PHASE 11 — ANALYTICS

Implement:

```text
Web Analytics
Performance metrics
Dashboard
Privacy controls
Retention
```

## PHASE 12 — STORAGE

Implement:

```text
Object storage
Buckets
Objects
Signed URLs
Upload
Download
```

## PHASE 13 — CRON / QUEUES / WEBHOOKS

Implement:

```text
Cron
Background jobs
Queues
Webhooks
Deploy hooks
Retry system
Dead-letter queues
```

## PHASE 14 — BILLING

Implement:

```text
Plans
Usage
Entitlements
Quotas
Stripe
Invoices
Usage alerts
```

## PHASE 15 — CLI / SDK

Implement:

```text
CLI
TypeScript SDK
API tokens
Deployment commands
Environment commands
```

## PHASE 16 — ADMIN

Implement:

```text
Users
Teams
Projects
Workers
Queues
Usage
Billing
System health
Security events
Feature flags
```

## PHASE 17 — INTEGRATIONS

Implement:

```text
GitHub
GitLab
Slack
Sentry
Stripe
Cloudflare
Databases
```

## PHASE 18 — ADVANCED PLATFORM

Implement:

```text
AI Gateway
AI SDK abstraction
Sandbox
Workflow
Queue platform
Platform API
Multi-tenant customer provisioning
White-label
MCP interface
```

---

# 321. DEFINITION OF DONE

A feature is NOT complete merely because:

```text
UI exists
```

A feature is complete only when:

```text
UI
+
API
+
Database
+
Validation
+
Authorization
+
Error handling
+
Loading states
+
Tests
+
Documentation
```

are implemented where applicable.

---

# 322. CLAUDE CODE EXECUTION RULE

Do not attempt to build the entire platform in one giant generated change.

Work phase-by-phase.

For each phase:

```text
1. Inspect repository
2. Understand existing architecture
3. Implement feature
4. Run typecheck
5. Run lint
6. Run tests
7. Fix errors
8. Run build
9. Update documentation
10. Report completed work
```

---

# 323. BEFORE MODIFYING CODE

Always inspect:

```text
package.json
pnpm-workspace.yaml
turbo.json
.env.example
database schema
apps
packages
README
existing tests
```

Do not overwrite existing working functionality blindly.

---

# 324. DO NOT ASK FOR UNNECESSARY CONFIRMATION

If the architecture is clear, implement it.

Only ask when a decision genuinely requires user-specific information such as:

```text
cloud provider credentials
production domain
billing account
GitHub OAuth credentials
```

For everything else choose a sensible engineering default and document it.

---

# 325. WHEN A FEATURE REQUIRES EXTERNAL CREDENTIALS

Create:

```text
.env.example
```

and a setup document.

Example:

```text
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
```

The application must fail gracefully if the integration is not configured.

Example:

```text
GitHub integration is not configured.

Configure GitHub OAuth credentials in the environment.
```

---

# 326. UI QUALITY RULE

Every page must look intentionally designed.

Do not produce:

```text
generic cards
default HTML buttons
unstructured forms
giant empty spaces
random colors
```

Use consistent:

```text
spacing
typography
border radius
icons
states
interaction
```

---

# 327. ANIMATION RULE

Use animation only when it improves:

```text
navigation
feedback
state transitions
loading
deployment progress
command menu
dialogs
```

Avoid excessive animation.

Respect:

```css
prefers-reduced-motion
```

---

# 328. RESPONSIVE RULE

Do not hide major functionality simply because the screen is small.

Instead:

```text
desktop:
sidebar

tablet:
compact sidebar

mobile:
drawer/bottom navigation
```

---

# 329. ACCESSIBILITY RULE

No clickable `<div>` where a button/link is appropriate.

Every form field needs:

```text
label
error
description where necessary
```

---

# 330. SECURITY-FIRST RULE

Whenever implementing:

```text
URL fetching
file upload
code execution
webhooks
Git
containers
domains
DNS
secrets
billing
```

perform a security review before considering the feature complete.

---

# 331. TEST MATRIX

Minimum tests:

```text
Authentication
RBAC
Project creation
Git integration
Deployment creation
Build success
Build failure
Build timeout
Build cancellation
Preview deployment
Production deployment
Rollback
Environment variables
Domain verification
SSL
Functions
Function timeout
Storage
Cron
Webhook
API token
Rate limit
Billing
Tenant isolation
```

---

# 332. SECURITY TEST MATRIX

Test:

```text
SQL injection
XSS
CSRF
SSRF
Path traversal
Zip Slip
Zip bomb
Command injection
Container escape protections
Authorization bypass
IDOR
Webhook forgery
Token leakage
Secret leakage
Rate-limit bypass
```

---

# 333. PERFORMANCE TEST MATRIX

Test:

```text
100 concurrent dashboard requests
100 concurrent API requests
10 concurrent builds
1000 log events/sec
function burst
storage upload
deployment router throughput
```

Actual limits should be documented according to infrastructure.

---

# 334. FINAL PROJECT STRUCTURE

Target:

```text
deploycloud/
│
├── apps/
│   ├── web/
│   ├── api/
│   ├── worker/
│   ├── builder/
│   ├── router/
│   ├── admin/
│   └── docs/
│
├── packages/
│   ├── ui/
│   ├── auth/
│   ├── database/
│   ├── deployments/
│   ├── builders/
│   ├── runtime/
│   ├── domains/
│   ├── dns/
│   ├── storage/
│   ├── analytics/
│   ├── observability/
│   ├── billing/
│   ├── security/
│   ├── queue/
│   ├── git/
│   ├── integrations/
│   ├── sdk/
│   └── config/
│
├── infra/
│   ├── docker/
│   ├── kubernetes/
│   └── terraform/
│
├── docs/
├── tests/
├── PLAN.md
├── README.md
├── docker-compose.yml
├── package.json
├── pnpm-lock.yaml
├── pnpm-workspace.yaml
└── turbo.json
```

---

# 335. FIRST COMMANDS CLAUDE CODE SHOULD RUN

After reading this file:

```bash
pwd
ls -la
find . -maxdepth 2 -type f | sort | head -300
```

Then inspect:

```bash
cat package.json
```

if it exists.

Then determine whether this is:

```text
empty repository
existing application
partial implementation
```

Do not destroy an existing application.

---

# 336. FIRST IMPLEMENTATION TARGET

If the repository is empty:

Create:

```text
Turborepo
Next.js web
Fastify API
PostgreSQL
Redis
MinIO
BullMQ worker
Docker Compose
shared UI package
shared database package
```

Then implement:

```text
Authentication
 ↓
Dashboard
 ↓
Project creation
 ↓
Deployment engine
```

---

# 337. FIRST SUCCESS CRITERIA

The first meaningful milestone is:

```text
User signs up
 ↓
Creates team
 ↓
Creates project
 ↓
Uploads/links repository
 ↓
Clicks Deploy
 ↓
Deployment enters queue
 ↓
Builder runs
 ↓
Build logs appear live
 ↓
Artifacts are uploaded
 ↓
Deployment becomes READY
 ↓
Generated URL opens
```

If this works end-to-end, continue to:

```text
Preview
Production
Domains
Functions
Analytics
Storage
Billing
Advanced infrastructure
```

---

# 338. CLAUDE CODE RESPONSE FORMAT

After each phase, report:

```text
PHASE:
Status:

Implemented:
- ...
- ...
- ...

Files changed:
- ...
- ...

Database changes:
- ...

API changes:
- ...

Tests:
- ...

Build:
PASS/FAIL

Security checks:
- ...

Next phase:
...
```

Do not claim success unless the relevant tests/build actually pass.

---

# 339. FINAL ACCEPTANCE CRITERIA

The final platform must provide a coherent working experience:

```text
AUTH
 ↓
TEAM
 ↓
PROJECT
 ↓
GIT
 ↓
DEPLOYMENT
 ↓
BUILD
 ↓
ARTIFACT
 ↓
URL
 ↓
DOMAIN
 ↓
FUNCTIONS
 ↓
LOGS
 ↓
ANALYTICS
 ↓
STORAGE
 ↓
CRON
 ↓
WEBHOOKS
 ↓
USAGE
 ↓
BILLING
```

And advanced:

```text
AI
SANDBOX
WORKFLOW
QUEUES
PLATFORM API
MCP
MULTI-TENANT
WHITE-LABEL
```

---

# 340. FINAL INSTRUCTION

Read this entire `PLAN.md` before implementing.

Do not treat it as a request to generate only frontend pages.

Treat it as the master engineering specification for a full cloud deployment platform.

Build real functionality.

Keep the system modular.

Keep security as a first-class requirement.

Keep the UI polished.

Keep APIs documented.

Keep infrastructure reproducible.

Keep every feature testable.

Do not fake functionality.

Do not copy proprietary Vercel code or assets.

Build an original platform with comparable categories of developer workflows.

Start with Phase 1 and continue sequentially until the entire implementation is complete.
