# CloudMart (Frontend) & Rave Lite (Full-stack Demo)

This repository contains a React frontend, a full-stack demo with a Node.js + Express backend, and deployment artifacts. The two main parts are:

- `frontend/` — React + Vite frontend (source code and UI used by the app)
- `backend/` — Node.js + Express API that supports products, orders, AI endpoints (DynamoDB + AI integrations)

You can run the frontend standalone (for UI development) or run both the frontend and the backend together for a full-stack local test.

---

## Quick Overview

- Frontend: `frontend/src/` (React + Vite)
- Backend: `backend/src/` (Node.js + Express, connects to DynamoDB and AI services)
- Default API base used by frontend: `VITE_API_BASE_URL` (set in `.env` for local development)

---

## Getting started (local development)

Prerequisites:
- Node.js (v14+)
- npm (or yarn)
- Docker (optional, for DynamoDB Local or LocalStack)

We recommend running the backend and frontend in separate terminals.

1) Run the backend

```bash
cd backend
npm ci
# Copy or create a .env file and add the required env variables below
# Then start the backend:
npm run dev
```

By default the backend listens on port 3000 (see `backend/src/server.js`).

2) Run the frontend

```bash
cd frontend
npm ci
# Configure the API base URL — points to the running backend
echo "VITE_API_BASE_URL=http://localhost:3000/api" > .env
npm run dev
```

Open the dev site at the Vite dev URL (usually `http://localhost:5173`).

---

## Environment variables

Backend `.env` (example):

```text
PORT=3000
AWS_REGION=your_aws_region
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
OPENAI_API_KEY=your_openai_api_key
OPENAI_ASSISTANT_ID=your_openai_assistant_id
BEDROCK_AGENT_ID=your_bedrock_agent_id
BEDROCK_AGENT_ALIAS_ID=your_bedrock_agent_alias_id
# Optional dev-only variables noted below
DYNAMODB_ENDPOINT=http://localhost:8000
MOCK_AI=true
```

Frontend `.env`:

```text
VITE_API_BASE_URL=http://localhost:3000/api
```

Notes:
- If you do not have real AWS or OpenAI credentials, follow the section below to run locally using DynamoDB Local and mock AI responses.

---

## Running without AWS/OpenAI (free local development)

For fast local development where you don't want to configure AWS or OpenAI keys, you have two options:

### Option A - DynamoDB Local + mock AI

1. Run DynamoDB Local (Docker):

```bash
docker run -d -p 8000:8000 --name dynamodb-local amazon/dynamodb-local
```

2. Use local endpoint for DynamoDB in backend `.env`:

```bash
DYNAMODB_ENDPOINT=http://localhost:8000
AWS_ACCESS_KEY_ID=local
AWS_SECRET_ACCESS_KEY=local
AWS_REGION=us-east-1
MOCK_AI=true
```

3. Start backend and frontend as described above. The backend uses `DYNAMODB_ENDPOINT` to connect to local DynamoDB and `MOCK_AI=true` to return canned AI responses.

### Option B - LocalStack (full AWS API emulation)

LocalStack is heavier but simulates AWS APIs. Use the LocalStack Docker image to emulate AWS services. Adjust `DYNAMODB_ENDPOINT` to `http://localhost:4566` and create tables using `aws --endpoint-url http://localhost:4566 dynamodb create-table ...`.

---

## API endpoints (examples)

The backend exposes common REST endpoints for the demo application at `/api/*`:
- `GET /api/products`
- `GET /api/products/:id`
- `POST /api/products`
- `PUT /api/products/:id`
- `DELETE /api/products/:id`

- `POST /api/orders` — create a new order
- `GET /api/orders` — admin list orders
- `GET /api/orders/user?email=...` — orders by user email

- AI endpoints (Bedrock/OpenAI): `POST /api/ai/*` and `POST /api/ai/bedrock/*`

Check the backend `src/routes` for additional endpoints and controllers.

---

## Production build / Docker

There is an existing `Dockerfile` in the repo that builds and serves the static frontend using `serve`. To build and run the production image:

```bash
docker build -t cloudmart-frontend .
docker run -p 5001:5001 cloudmart-frontend
```

In production you typically set `VITE_API_BASE_URL` to the backend's public URL.

---

## Best practices & tips

- Admin routes in the demo are not protected; add authentication & route guards before production usage.
- Run DynamoDB Local or LocalStack to avoid cloud AWS usage during development.
- Use `MOCK_AI=true` to test AI UIs without API keys.
- Keep an eye on CORS headers for the backend (backend includes `cors()` by default).

---

## Contributing

If you'd like to contribute or test locally, please fork and make a feature branch or open a PR. If you need a fully local test setup for CI or team members, consider adding a `docker-compose.yml` to bring up DynamoDB Local, backend, and frontend together.

---

## Contact

Maintainer: Ishaq Ansari — ishaq.ansari.work@gmail.com

---

License: ISC
# Rave Lite

Cloudmart is a modern e-commerce platform powered by AI, designed to provide a seamless and intuitive shopping experience. Built with React, Node.js, and AWS services, this application demonstrates how artificial intelligence can enhance online retail.

## Features

- **AI-powered Shopping Assistant**: Get product recommendations and support through an integrated AI assistant
- **Product Browsing**: Browse through a catalog of products with search functionality
- **Shopping Cart**: Add products to your cart and manage quantities
- **Order Management**: Place orders and track their status
- **User Profiles**: Manage your user information and view order history
- **Admin Dashboard**: Manage products and orders (admin access)

## Project Structure

The project is organized into frontend and backend directories:

### Frontend (React + Vite)

```
frontend/
├── public/             # Public assets
├── src/                # Source files
│   ├── assets/         # Static assets
│   ├── components/     # React components
│   ├── config/         # Configuration files
│   └── utils/          # Utility functions
└── package.json        # Dependencies and scripts
```

### Backend (Node.js + Express)

```
backend/
├── src/
│   ├── controllers/    # Request handlers
│   ├── routes/         # API route definitions
│   ├── services/       # Business logic
│   └── lambda/         # AWS Lambda functions
└── package.json        # Dependencies and scripts
```

## Technologies Used

- **Frontend**:
  - React
  - React Router
  - Tailwind CSS
  - Axios
  - Vite

- **Backend**:
  - Node.js
  - Express
  - AWS DynamoDB
  - AWS Lambda
  - OpenAI API
  - AWS Bedrock

## Getting Started

### Prerequisites

- Node.js (v14+)
- npm or yarn
- AWS account with appropriate credentials
- OpenAI API key

### Installation

1. Clone the repository:
```
git clone <repository-url>
cd cloudmart
```

2. Install backend dependencies:
```
cd backend
npm install
```

3. Set up environment variables:
Create a `.env` file in the backend directory with the following variables:
```
OPENAI_API_KEY=your_openai_api_key
OPENAI_ASSISTANT_ID=your_assistant_id
BEDROCK_AGENT_ID=your_bedrock_agent_id
BEDROCK_AGENT_ALIAS_ID=your_bedrock_agent_alias_id
AWS_REGION=your_aws_region
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
```

4. Install frontend dependencies:
```
cd ../frontend
npm install
```

5. Create a `.env` file in the frontend directory:
```
VITE_API_BASE_URL=http://localhost:3000/api
```

### Running the Application

1. Start the backend server:
```
cd backend
npm run dev
```

2. Start the frontend development server:
```
cd frontend
npm run dev
```

3. Access the application at `http://localhost:5173`

## API Endpoints

### Products

- `GET /api/products` - Get all products
- `GET /api/products/:id` - Get a specific product
- `POST /api/products` - Create a new product
- `PUT /api/products/:id` - Update a product
- `DELETE /api/products/:id` - Delete a product

### Orders

- `POST /api/orders` - Create a new order
- `GET /api/orders` - Get all orders
- `GET /api/orders/user` - Get orders by user email
- `GET /api/orders/:id` - Get a specific order
- `PUT /api/orders/:id` - Update an order
- `DELETE /api/orders/:id` - Delete an order

### AI Assistant

- `POST /api/ai/start` - Start a new OpenAI conversation
- `POST /api/ai/message` - Send a message to OpenAI assistant
- `POST /api/ai/bedrock/start` - Start a new AWS Bedrock conversation
- `POST /api/ai/bedrock/message` - Send a message to AWS Bedrock assistant

## Building for Production

1. Build the frontend:
```
cd frontend
npm run build
```

2. The production-ready files will be available in the `frontend/dist` directory, which can be served by the backend or a static file server.

## Contact

For questions or more information, please contact [Ishaq Ansari](mailto:ishaq.ansari.work@gmail.com).

## License

This project is licensed under the ISC License.
