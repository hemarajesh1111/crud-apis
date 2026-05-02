# API Usage Examples

This document provides practical examples of how to use the Project Management REST API.

## 📍 Table of Contents
- [cURL Examples](#curl-examples)
- [JavaScript/Node.js Examples](#javascriptnodejs-examples)
- [Python Examples](#python-examples)
- [Postman Collection](#postman-collection)

---

## cURL Examples

### 1. Get All Projects
```bash
curl http://localhost:3000/api/v1/projects
```

### 2. Get All Projects with Filtering
```bash
# Filter by status
curl http://localhost:3000/api/v1/projects?status=in-progress

# Filter by priority
curl http://localhost:3000/api/v1/projects?priority=high

# Filter by both
curl http://localhost:3000/api/v1/projects?status=in-progress&priority=high
```

### 3. Get Single Project
```bash
curl http://localhost:3000/api/v1/projects/1
```

### 4. Create New Project
```bash
curl -X POST http://localhost:3000/api/v1/projects \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Mobile App Development",
    "description": "Develop iOS and Android apps",
    "status": "planning",
    "priority": "high",
    "dueDate": "2026-08-15"
  }'
```

### 5. Create Project with Minimal Data
```bash
curl -X POST http://localhost:3000/api/v1/projects \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Simple Project",
    "description": "A simple project description"
  }'
```

### 6. Full Update (PUT) - All Fields Required
```bash
curl -X PUT http://localhost:3000/api/v1/projects/1 \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Updated E-Commerce Platform",
    "description": "A full-featured e-commerce platform with payment integration",
    "status": "completed",
    "priority": "high",
    "dueDate": "2026-06-30"
  }'
```

### 7. Partial Update (PATCH) - Only Changed Fields
```bash
# Update only status
curl -X PATCH http://localhost:3000/api/v1/projects/1 \
  -H "Content-Type: application/json" \
  -d '{
    "status": "completed"
  }'

# Update status and priority
curl -X PATCH http://localhost:3000/api/v1/projects/1 \
  -H "Content-Type: application/json" \
  -d '{
    "status": "in-progress",
    "priority": "urgent"
  }'
```

### 8. Delete Project
```bash
curl -X DELETE http://localhost:3000/api/v1/projects/1
```

### 9. Health Check
```bash
curl http://localhost:3000/health
```

---

## JavaScript/Node.js Examples

### Using Fetch API

```javascript
const API_URL = 'http://localhost:3000/api/v1';

// 1. Get all projects
async function getAllProjects() {
  try {
    const response = await fetch(`${API_URL}/projects`);
    const data = await response.json();
    console.log('All projects:', data.data.projects);
  } catch (error) {
    console.error('Error fetching projects:', error);
  }
}

// 2. Get projects by status
async function getProjectsByStatus(status) {
  try {
    const response = await fetch(`${API_URL}/projects?status=${status}`);
    const data = await response.json();
    console.log(`Projects with status ${status}:`, data.data.projects);
  } catch (error) {
    console.error('Error fetching projects:', error);
  }
}

// 3. Get single project
async function getProjectById(id) {
  try {
    const response = await fetch(`${API_URL}/projects/${id}`);
    const data = await response.json();
    if (data.success) {
      console.log('Project:', data.data);
    } else {
      console.error('Error:', data.error.message);
    }
  } catch (error) {
    console.error('Error fetching project:', error);
  }
}

// 4. Create new project
async function createProject(projectData) {
  try {
    const response = await fetch(`${API_URL}/projects`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(projectData),
    });
    const data = await response.json();
    if (data.success) {
      console.log('Project created:', data.data);
    } else {
      console.error('Error:', data.error.message);
    }
  } catch (error) {
    console.error('Error creating project:', error);
  }
}

// 5. Update project (full update)
async function updateProject(id, projectData) {
  try {
    const response = await fetch(`${API_URL}/projects/${id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(projectData),
    });
    const data = await response.json();
    if (data.success) {
      console.log('Project updated:', data.data);
    } else {
      console.error('Error:', data.error.message);
    }
  } catch (error) {
    console.error('Error updating project:', error);
  }
}

// 6. Partial update (patch)
async function partialUpdateProject(id, updateData) {
  try {
    const response = await fetch(`${API_URL}/projects/${id}`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(updateData),
    });
    const data = await response.json();
    if (data.success) {
      console.log('Project patched:', data.data);
    } else {
      console.error('Error:', data.error.message);
    }
  } catch (error) {
    console.error('Error updating project:', error);
  }
}

// 7. Delete project
async function deleteProject(id) {
  try {
    const response = await fetch(`${API_URL}/projects/${id}`, {
      method: 'DELETE',
    });
    const data = await response.json();
    if (data.success) {
      console.log('Project deleted successfully');
    } else {
      console.error('Error:', data.error.message);
    }
  } catch (error) {
    console.error('Error deleting project:', error);
  }
}

// Usage examples
getAllProjects();
getProjectsByStatus('in-progress');
getProjectById('1');

createProject({
  title: 'New Web Application',
  description: 'Build a modern web application',
  status: 'planning',
  priority: 'medium',
  dueDate: '2026-09-30',
});

updateProject('1', {
  title: 'Updated Title',
  description: 'Updated description',
  status: 'completed',
  priority: 'high',
  dueDate: '2026-06-30',
});

partialUpdateProject('1', {
  status: 'in-progress',
  priority: 'urgent',
});

deleteProject('1');
```

### Using Axios

```javascript
const axios = require('axios');

const API_URL = 'http://localhost:3000/api/v1';
const api = axios.create({ baseURL: API_URL });

// Get all projects
api.get('/projects')
  .then(res => console.log('Projects:', res.data.data.projects))
  .catch(err => console.error('Error:', err.response.data.error));

// Create project
api.post('/projects', {
  title: 'New Project',
  description: 'Project description',
  priority: 'high',
})
  .then(res => console.log('Created:', res.data.data))
  .catch(err => console.error('Error:', err.response.data.error));

// Update project
api.put('/projects/1', {
  title: 'Updated Title',
  description: 'Updated description',
  status: 'completed',
  priority: 'high',
  dueDate: '2026-06-30',
})
  .then(res => console.log('Updated:', res.data.data))
  .catch(err => console.error('Error:', err.response.data.error));

// Patch project
api.patch('/projects/1', { status: 'in-progress' })
  .then(res => console.log('Patched:', res.data.data))
  .catch(err => console.error('Error:', err.response.data.error));

// Delete project
api.delete('/projects/1')
  .then(res => console.log('Deleted'))
  .catch(err => console.error('Error:', err.response.data.error));
```

---

## Python Examples

### Using Requests Library

```python
import requests
import json

API_URL = 'http://localhost:3000/api/v1'

# 1. Get all projects
def get_all_projects():
    response = requests.get(f'{API_URL}/projects')
    data = response.json()
    print('All projects:', json.dumps(data, indent=2))

# 2. Get projects by filter
def get_projects_by_status(status):
    response = requests.get(f'{API_URL}/projects?status={status}')
    data = response.json()
    print(f'Projects with {status}:', json.dumps(data, indent=2))

# 3. Get single project
def get_project(project_id):
    response = requests.get(f'{API_URL}/projects/{project_id}')
    data = response.json()
    if data['success']:
        print('Project:', json.dumps(data['data'], indent=2))
    else:
        print('Error:', data['error']['message'])

# 4. Create project
def create_project(title, description, status='planning', priority='medium', due_date=None):
    payload = {
        'title': title,
        'description': description,
        'status': status,
        'priority': priority,
    }
    if due_date:
        payload['dueDate'] = due_date
    
    response = requests.post(f'{API_URL}/projects', json=payload)
    data = response.json()
    if data['success']:
        print('Created project:', json.dumps(data['data'], indent=2))
    else:
        print('Error:', data['error']['message'])

# 5. Update project (full)
def update_project(project_id, title, description, status, priority, due_date):
    payload = {
        'title': title,
        'description': description,
        'status': status,
        'priority': priority,
        'dueDate': due_date,
    }
    response = requests.put(f'{API_URL}/projects/{project_id}', json=payload)
    data = response.json()
    if data['success']:
        print('Updated project:', json.dumps(data['data'], indent=2))
    else:
        print('Error:', data['error']['message'])

# 6. Partial update
def patch_project(project_id, **kwargs):
    response = requests.patch(f'{API_URL}/projects/{project_id}', json=kwargs)
    data = response.json()
    if data['success']:
        print('Patched project:', json.dumps(data['data'], indent=2))
    else:
        print('Error:', data['error']['message'])

# 7. Delete project
def delete_project(project_id):
    response = requests.delete(f'{API_URL}/projects/{project_id}')
    data = response.json()
    if data['success']:
        print('Project deleted successfully')
    else:
        print('Error:', data['error']['message'])

# Usage examples
if __name__ == '__main__':
    # Get all
    get_all_projects()
    
    # Create
    create_project(
        title='Python Project',
        description='A project created from Python',
        status='planning',
        priority='high',
        due_date='2026-12-31'
    )
    
    # Update
    update_project(
        project_id='1',
        title='Updated Title',
        description='Updated description',
        status='completed',
        priority='high',
        due_date='2026-06-30'
    )
    
    # Patch
    patch_project('1', status='in-progress')
    
    # Delete
    delete_project('1')
```

---

## Postman Collection

Import this JSON into Postman as a collection:

```json
{
  "info": {
    "name": "Project Management API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Get All Projects",
      "request": {
        "method": "GET",
        "url": "http://localhost:3000/api/v1/projects"
      }
    },
    {
      "name": "Get Project by ID",
      "request": {
        "method": "GET",
        "url": "http://localhost:3000/api/v1/projects/1"
      }
    },
    {
      "name": "Create Project",
      "request": {
        "method": "POST",
        "url": "http://localhost:3000/api/v1/projects",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\"title\":\"New Project\",\"description\":\"Project description\",\"status\":\"planning\",\"priority\":\"medium\",\"dueDate\":\"2026-12-31\"}"
        }
      }
    },
    {
      "name": "Update Project (PUT)",
      "request": {
        "method": "PUT",
        "url": "http://localhost:3000/api/v1/projects/1",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\"title\":\"Updated Title\",\"description\":\"Updated description\",\"status\":\"completed\",\"priority\":\"high\",\"dueDate\":\"2026-06-30\"}"
        }
      }
    },
    {
      "name": "Partial Update Project (PATCH)",
      "request": {
        "method": "PATCH",
        "url": "http://localhost:3000/api/v1/projects/1",
        "header": [
          {
            "key": "Content-Type",
            "value": "application/json"
          }
        ],
        "body": {
          "mode": "raw",
          "raw": "{\"status\":\"in-progress\"}"
        }
      }
    },
    {
      "name": "Delete Project",
      "request": {
        "method": "DELETE",
        "url": "http://localhost:3000/api/v1/projects/1"
      }
    }
  ]
}
```

---

## Tips & Best Practices

1. **Always validate data** before sending to the API
2. **Use PATCH** for partial updates to save bandwidth
3. **Use PUT** when replacing entire resource
4. **Handle errors** with proper error checking
5. **Use query parameters** for filtering to reduce data transfer
6. **Set appropriate timeouts** for API requests
7. **Log API responses** for debugging
8. **Implement retry logic** for failed requests
9. **Cache responses** when appropriate
10. **Use async/await** for cleaner code

---

Made with ❤️ for clean, maintainable code
