# 📚 API Documentation

## Base URL

```
Development: http://localhost:3000/api
Production: https://your-domain.com/api
```

## Authentication

All API endpoints (except authentication) require a valid JWT token in the Authorization header:

```http
Authorization: Bearer <your-jwt-token>
```

## Response Format

All API responses follow a consistent format:

### Success Response
```json
{
  "success": true,
  "data": { ... },
  "message": "Operation completed successfully"
}
```

### Error Response
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": { ... }
  }
}
```

## Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## 🔐 Authentication Endpoints

### POST /api/auth/signup

Register a new user account.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "name": "John Doe",
  "business_name": "Acme Corp",
  "business_address": "123 Main St, City, State 12345",
  "business_phone": "+1-555-0123",
  "business_email": "contact@acmecorp.com"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "name": "John Doe",
      "business_name": "Acme Corp"
    },
    "token": "jwt-token-here"
  },
  "message": "User registered successfully"
}
```

### POST /api/auth/signin

Authenticate user and return JWT token.

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "name": "John Doe",
      "business_name": "Acme Corp"
    },
    "token": "jwt-token-here"
  },
  "message": "Login successful"
}
```

### POST /api/auth/signout

Sign out the current user.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "message": "Signed out successfully"
}
```

### GET /api/auth/profile

Get current user profile.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe",
    "business_name": "Acme Corp",
    "business_address": "123 Main St, City, State 12345",
    "business_phone": "+1-555-0123",
    "business_email": "contact@acmecorp.com",
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

### PUT /api/auth/profile

Update user profile.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "name": "John Smith",
  "business_name": "Smith Corp",
  "business_address": "456 Oak Ave, City, State 12345",
  "business_phone": "+1-555-0456",
  "business_email": "contact@smithcorp.com"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Smith",
    "business_name": "Smith Corp",
    "business_address": "456 Oak Ave, City, State 12345",
    "business_phone": "+1-555-0456",
    "business_email": "contact@smithcorp.com",
    "updated_at": "2024-01-01T12:00:00Z"
  },
  "message": "Profile updated successfully"
}
```

---

## 👥 Client Management Endpoints

### GET /api/clients

Get all clients for the authenticated user.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Query Parameters:**
- `page` (optional): Page number (default: 1)
- `limit` (optional): Items per page (default: 20)
- `search` (optional): Search term for name or company

**Response:**
```json
{
  "success": true,
  "data": {
    "clients": [
      {
        "id": "uuid",
        "name": "Jane Doe",
        "company": "Doe Industries",
        "email": "jane@doeindustries.com",
        "phone": "+1-555-0789",
        "address": "789 Pine St, City, State 12345",
        "created_at": "2024-01-01T00:00:00Z",
        "updated_at": "2024-01-01T00:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 1,
      "pages": 1
    }
  }
}
```

### GET /api/clients/:id

Get a specific client by ID.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Jane Doe",
    "company": "Doe Industries",
    "email": "jane@doeindustries.com",
    "phone": "+1-555-0789",
    "address": "789 Pine St, City, State 12345",
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

### POST /api/clients

Create a new client.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "name": "Jane Doe",
  "company": "Doe Industries",
  "email": "jane@doeindustries.com",
  "phone": "+1-555-0789",
  "address": "789 Pine St, City, State 12345"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Jane Doe",
    "company": "Doe Industries",
    "email": "jane@doeindustries.com",
    "phone": "+1-555-0789",
    "address": "789 Pine St, City, State 12345",
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  },
  "message": "Client created successfully"
}
```

### PUT /api/clients/:id

Update an existing client.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "name": "Jane Smith",
  "company": "Smith Industries",
  "email": "jane@smithindustries.com",
  "phone": "+1-555-0123",
  "address": "123 New St, City, State 12345"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Jane Smith",
    "company": "Smith Industries",
    "email": "jane@smithindustries.com",
    "phone": "+1-555-0123",
    "address": "123 New St, City, State 12345",
    "updated_at": "2024-01-01T12:00:00Z"
  },
  "message": "Client updated successfully"
}
```

### DELETE /api/clients/:id

Delete a client.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "message": "Client deleted successfully"
}
```

### GET /api/clients/search/:query

Search clients by name or company.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "clients": [
      {
        "id": "uuid",
        "name": "Jane Doe",
        "company": "Doe Industries",
        "email": "jane@doeindustries.com"
      }
    ]
  }
}
```

---

## 🧾 Invoice Management Endpoints

### GET /api/invoices

Get all invoices for the authenticated user.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Query Parameters:**
- `page` (optional): Page number (default: 1)
- `limit` (optional): Items per page (default: 20)
- `status` (optional): Filter by status (draft, sent, paid, overdue, cancelled)
- `client_id` (optional): Filter by client ID
- `date_from` (optional): Filter from date (YYYY-MM-DD)
- `date_to` (optional): Filter to date (YYYY-MM-DD)

**Response:**
```json
{
  "success": true,
  "data": {
    "invoices": [
      {
        "id": "uuid",
        "invoice_number": "INV-001",
        "title": "Website Development",
        "client": {
          "id": "uuid",
          "name": "Jane Doe",
          "company": "Doe Industries"
        },
        "invoice_date": "2024-01-01",
        "due_date": "2024-01-31",
        "status": "sent",
        "total_amount": 1500.00,
        "currency": "USD",
        "created_at": "2024-01-01T00:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 1,
      "pages": 1
    }
  }
}
```

### GET /api/invoices/summary

Get dashboard summary statistics.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "total_invoices": 25,
    "total_revenue": 125000.00,
    "pending_amount": 15000.00,
    "overdue_amount": 5000.00,
    "status_breakdown": {
      "draft": 5,
      "sent": 10,
      "paid": 8,
      "overdue": 2,
      "cancelled": 0
    },
    "recent_invoices": [
      {
        "id": "uuid",
        "invoice_number": "INV-025",
        "client_name": "Jane Doe",
        "total_amount": 2500.00,
        "status": "sent",
        "created_at": "2024-01-15T00:00:00Z"
      }
    ]
  }
}
```

### GET /api/invoices/:id

Get a specific invoice with all details.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "invoice_number": "INV-001",
    "title": "Website Development",
    "client": {
      "id": "uuid",
      "name": "Jane Doe",
      "company": "Doe Industries",
      "email": "jane@doeindustries.com",
      "phone": "+1-555-0789",
      "address": "789 Pine St, City, State 12345"
    },
    "invoice_date": "2024-01-01",
    "due_date": "2024-01-31",
    "payment_terms": "net_30",
    "currency": "USD",
    "subtotal": 1200.00,
    "tax_rate": 10.00,
    "tax_amount": 120.00,
    "discount": 0.00,
    "total_amount": 1320.00,
    "status": "sent",
    "reference_number": "REF-001",
    "payment_instructions": "Payment due within 30 days",
    "notes": "Thank you for your business!",
    "items": [
      {
        "id": "uuid",
        "description": "Website Design",
        "quantity": 1,
        "unit_price": 800.00,
        "total": 800.00
      },
      {
        "id": "uuid",
        "description": "Development",
        "quantity": 10,
        "unit_price": 40.00,
        "total": 400.00
      }
    ],
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

### POST /api/invoices

Create a new invoice.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "client_id": "uuid",
  "title": "Website Development",
  "invoice_date": "2024-01-01",
  "due_date": "2024-01-31",
  "payment_terms": "net_30",
  "currency": "USD",
  "tax_rate": 10.00,
  "discount": 0.00,
  "reference_number": "REF-001",
  "payment_instructions": "Payment due within 30 days",
  "notes": "Thank you for your business!",
  "items": [
    {
      "description": "Website Design",
      "quantity": 1,
      "unit_price": 800.00
    },
    {
      "description": "Development",
      "quantity": 10,
      "unit_price": 40.00
    }
  ]
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "invoice_number": "INV-001",
    "title": "Website Development",
    "total_amount": 1320.00,
    "status": "draft",
    "created_at": "2024-01-01T00:00:00Z"
  },
  "message": "Invoice created successfully"
}
```

### PUT /api/invoices/:id

Update an existing invoice.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "title": "Updated Website Development",
  "due_date": "2024-02-15",
  "tax_rate": 8.50,
  "notes": "Updated project scope"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "title": "Updated Website Development",
    "due_date": "2024-02-15",
    "tax_rate": 8.50,
    "notes": "Updated project scope",
    "updated_at": "2024-01-15T12:00:00Z"
  },
  "message": "Invoice updated successfully"
}
```

### PATCH /api/invoices/:id/status

Update invoice status.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "status": "sent"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "sent",
    "updated_at": "2024-01-15T12:00:00Z"
  },
  "message": "Invoice status updated successfully"
}
```

### DELETE /api/invoices/:id

Delete an invoice.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "message": "Invoice deleted successfully"
}
```

### POST /api/invoices/:id/duplicate

Duplicate an existing invoice.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "invoice_number": "INV-002",
    "title": "Website Development (Copy)",
    "status": "draft",
    "created_at": "2024-01-15T12:00:00Z"
  },
  "message": "Invoice duplicated successfully"
}
```

---

## 🤖 AI Processing Endpoints

### POST /api/ai/parse-invoice

Parse invoice data from text using AI.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "text": "I built a website for ACME on January 22, 2024. Its due in 30 days. It took me 10 hours and charged 40$ an hour. I also hosted their website and charged 5 dollars for the year. I was then requested to make a change which i completed in 3 hours and charged 20$ and hour for. the invoice number is 12345."
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "client_name": "ACME",
    "invoice_number": "12345",
    "invoice_date": "2024-01-22",
    "due_date": "2024-02-21",
    "items": [
      {
        "description": "Website Development",
        "quantity": 10,
        "unit_price": 40.00,
        "total": 400.00
      },
      {
        "description": "Website Hosting",
        "quantity": 1,
        "unit_price": 5.00,
        "total": 5.00
      },
      {
        "description": "Website Changes",
        "quantity": 3,
        "unit_price": 20.00,
        "total": 60.00
      }
    ],
    "subtotal": 465.00,
    "total_amount": 465.00
  },
  "message": "Invoice parsed successfully"
}
```

### POST /api/ai/suggest-content

Get AI suggestions for invoice content.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "context": "web development project",
  "client_name": "Tech Corp",
  "project_type": "website"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "suggested_title": "Website Development - Tech Corp",
    "suggested_items": [
      {
        "description": "Frontend Development",
        "suggested_price": 150.00
      },
      {
        "description": "Backend Development",
        "suggested_price": 200.00
      },
      {
        "description": "Database Setup",
        "suggested_price": 100.00
      }
    ],
    "suggested_notes": "Thank you for choosing our web development services. We're excited to work with Tech Corp on this project."
  },
  "message": "Content suggestions generated successfully"
}
```

### POST /api/ai/validate-data

Validate invoice data using AI.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Request Body:**
```json
{
  "invoice_data": {
    "client_name": "ACME Corp",
    "items": [
      {
        "description": "Website Development",
        "quantity": 1,
        "unit_price": 1000.00
      }
    ],
    "total_amount": 1000.00
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "is_valid": true,
    "suggestions": [
      "Consider adding a tax rate for business invoices",
      "Payment terms could be more specific"
    ],
    "warnings": [],
    "confidence_score": 0.95
  },
  "message": "Data validation completed"
}
```

---

## 📊 Export Endpoints

### GET /api/invoices/:id/export/pdf

Export invoice as PDF.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Response:**
- Content-Type: `application/pdf`
- File download with invoice PDF

### GET /api/invoices/export/csv

Export all invoices as CSV.

**Headers:**
```http
Authorization: Bearer <jwt-token>
```

**Query Parameters:**
- `date_from` (optional): Export from date
- `date_to` (optional): Export to date
- `status` (optional): Filter by status

**Response:**
- Content-Type: `text/csv`
- File download with invoice data

---

## 🚨 Error Handling

### Common Error Codes

| Code | Description | Solution |
|------|-------------|----------|
| `INVALID_CREDENTIALS` | Invalid email or password | Check credentials |
| `TOKEN_EXPIRED` | JWT token has expired | Re-authenticate |
| `INSUFFICIENT_PERMISSIONS` | User lacks required permissions | Check user role |
| `VALIDATION_ERROR` | Request data validation failed | Check request format |
| `RESOURCE_NOT_FOUND` | Requested resource doesn't exist | Verify resource ID |
| `RATE_LIMIT_EXCEEDED` | Too many requests | Wait and retry |
| `AI_SERVICE_ERROR` | AI processing failed | Retry or contact support |

### Error Response Examples

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": {
      "field": "email",
      "reason": "Invalid email format"
    }
  }
}
```

---

## 🔒 Security Considerations

### Authentication
- All endpoints require valid JWT tokens
- Tokens expire after 24 hours
- Refresh tokens available for extended sessions

### Rate Limiting
- 100 requests per minute per user
- 1000 requests per hour per user
- AI endpoints: 10 requests per minute per user

### Data Validation
- All input data is validated and sanitized
- SQL injection prevention through parameterized queries
- XSS protection through input sanitization

### Privacy
- Row-level security ensures users only access their own data
- All sensitive data is encrypted in transit and at rest
- Audit logging for all data modifications

---

## 📝 SDK Examples

### JavaScript/Node.js

```javascript
const axios = require('axios');

const api = axios.create({
  baseURL: 'http://localhost:3000/api',
  headers: {
    'Content-Type': 'application/json'
  }
});

// Add auth token
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('authToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Example: Create invoice
const createInvoice = async (invoiceData) => {
  try {
    const response = await api.post('/invoices', invoiceData);
    return response.data;
  } catch (error) {
    console.error('Error creating invoice:', error.response.data);
    throw error;
  }
};
```

### Python

```python
import requests

class InvoiceAPIClient:
    def __init__(self, base_url, token=None):
        self.base_url = base_url
        self.token = token
        self.session = requests.Session()
        if token:
            self.session.headers.update({
                'Authorization': f'Bearer {token}'
            })
    
    def create_invoice(self, invoice_data):
        response = self.session.post(
            f'{self.base_url}/invoices',
            json=invoice_data
        )
        response.raise_for_status()
        return response.json()
```

This comprehensive API documentation provides developers with all the information needed to integrate with the AI Invoice Generator API effectively.

**Created by**: Pranay Tyagi & Finn Connelly  
**Note**: This is a private project. The API is not publicly available, but this documentation is provided for reference and understanding of the system capabilities.
