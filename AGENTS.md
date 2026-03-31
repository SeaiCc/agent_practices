# Agent Guidelines

## Project Overview

<!-- Add project description, purpose, and key technologies here -->

## Build Commands

### Package Installation
```bash
npm install          # Install dependencies
pnpm install         # Install with pnpm (preferred)
yarn install         # Install with yarn
bun install          # Install with bun
```

### Running the Project
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run preview      # Preview production build
npm start            # Start production server
```

### Code Quality
```bash
npm run lint         # Run linter
npm run lint:fix     # Run linter with auto-fix
npm run format       # Format code with Prettier
npm run typecheck    # Run TypeScript type checking
```

### Testing
```bash
npm test             # Run all tests
npm run test:watch   # Run tests in watch mode
npm run test:coverage # Run tests with coverage report

# Run a single test file
npx vitest run src/__tests__/specific.test.ts
npx jest src/__tests__/specific.test.ts
npx mocha --require ts-node/register src/__tests__/specific.test.ts

# Run tests matching a pattern
npm test -- --grep "test name pattern"
```

### Git Hooks
```bash
npm run prepare      # Set up git hooks (husky)
```

## Code Style Guidelines

### General Principles
- Write self-documenting code with clear variable and function names
- Keep functions small and focused (single responsibility)
- Maximum line length: 100 characters
- Use async/await over raw promises
- Handle errors explicitly with try/catch or .catch()

### TypeScript/JavaScript

#### Naming Conventions
```typescript
// Variables and functions: camelCase
const userName = 'John';
function getUserById(id: string) {}

// Classes and types: PascalCase
class UserService {}
interface UserConfig {}
type ApiResponse = {}

// Constants: UPPER_SNAKE_CASE
const MAX_RETRY_COUNT = 3;
const API_BASE_URL = 'https://api.example.com';

// Files: kebab-case
// user-service.ts, user-config.ts, api-utils.ts
```

#### Imports
```typescript
// 1. External libraries
import React from 'react';
import { useState, useEffect } from 'react';

// 2. Internal modules (absolute paths)
import { UserService } from '@/services/user';
import { Button } from '@/components/ui';

// 3. Internal relative imports
import { helper } from './utils/helper';
import type { Config } from './types';

// 4. Type-only imports
import type { User } from '@/types';

// Ordering: alphabetical within each group
// Side effect imports at the top
```

#### Types
```typescript
// Use interfaces for object shapes
interface User {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}

// Use type aliases for unions, intersections, mapped types
type Status = 'pending' | 'active' | 'inactive';
type MaybeUser = User | null;
type UserMap = Record<string, User>;

// Avoid any - use unknown for truly unknown types
function parseJSON(input: string): unknown {
  return JSON.parse(input);
}

// Use explicit return types for exported functions
export function getUser(id: string): Promise<User> {
  // implementation
}
```

#### Functions
```typescript
// Use arrow functions for callbacks
const handleClick = (event: MouseEvent) => {
  event.preventDefault();
};

// Use async/await
async function fetchUsers(): Promise<User[]> {
  try {
    const response = await api.get('/users');
    return response.data;
  } catch (error) {
    console.error('Failed to fetch users:', error);
    throw error;
  }
}

// Destructure parameters
function createUser({ name, email, role = 'user' }: UserInput): User {
  // implementation
}
```

### Python

#### Naming Conventions
```python
# Variables and functions: snake_case
user_name = 'John'
def get_user_by_id(user_id):
    pass

# Classes: PascalCase
class UserService:
    pass

# Constants: UPPER_SNAKE_CASE
MAX_RETRY_COUNT = 3
API_BASE_URL = 'https://api.example.com'
```

#### Imports
```python
# Standard library
import os
import sys
from datetime import datetime

# Third party
import pandas as pd
from fastapi import FastAPI

# Local application
from .models import User
from .utils import format_date
```

#### Type Hints
```python
from typing import Optional, List, Dict

def get_user(user_id: str) -> Optional[Dict]:
    # implementation

async def process_items(items: List[str]) -> List[Dict]:
    # implementation
```

### Error Handling
```typescript
// Always handle errors in async functions
async function fetchData() {
  try {
    const data = await api.get('/data');
    return data;
  } catch (error) {
    if (error instanceof NetworkError) {
      throw new UserFriendlyError('Network connection failed');
    }
    throw error;
  }
}

// Use Result pattern when appropriate
type Result<T, E = Error> = { ok: true; value: T } | { ok: false; error: E };
```

### React/Components
```tsx
// Component naming: PascalCase
const UserProfile = ({ userId }: { userId: string }) => {
  const [user, setUser] = useState<User | null>(null);

  useEffect(() => {
    loadUser(userId).then(setUser);
  }, [userId]);

  if (!user) return <Loading />;
  
  return (
    <div className="user-profile">
      <h2>{user.name}</h2>
    </div>
  );
};

// Event handlers: handle* prefix
const handleClick = (event: MouseEvent) => {};

// Boolean variables: is*, has*, should*
const isLoading = true;
const hasError = false;
const shouldRender = true;
```

### SQL
```sql
-- Keywords: UPPERCASE
SELECT user_id, user_name, created_at
FROM users
WHERE status = 'active'
  AND created_at > '2024-01-01'
ORDER BY created_at DESC;

-- Table names: plural, snake_case
-- user_orders, product_categories

-- Column names: snake_case
-- user_id, first_name, created_at
```

## Testing Guidelines

### Test File Naming
```
src/__tests__/user.service.test.ts
src/components/__tests__/Button.test.tsx
tests/unit/user.test.js
tests/integration/api.test.js
```

### Test Structure
```typescript
// Describe -> It/Test -> Expect
describe('UserService', () => {
  describe('getUser', () => {
    it('should return user when valid id provided', async () => {
      const user = await userService.getUser('123');
      expect(user).toBeDefined();
      expect(user.id).toBe('123');
    });

    it('should throw error for invalid id', async () => {
      await expect(userService.getUser('invalid'))
        .rejects.toThrow('User not found');
    });
  });
});
```

### Best Practices
- One assertion concept per test
- Use descriptive test names: "should return user when valid id provided"
- Mock external dependencies
- Test edge cases and error scenarios
- Keep tests independent and idempotent

## Git Workflow

### Commit Messages
```
feat: add user authentication
fix: resolve login redirect issue
docs: update API documentation
style: format code according to lint rules
refactor: extract validation logic
test: add unit tests for UserService
chore: update dependencies
```

### Branch Naming
```
feature/user-authentication
fix/login-redirect
hotfix/security-patch
```

## Environment Variables

```bash
# .env.example
NODE_ENV=development
API_URL=https://api.example.com
DATABASE_URL=postgresql://localhost:5432/app
```

## Additional Notes

<!-- Add project-specific rules, patterns, and conventions here -->

## Troubleshooting

### Common Issues
- **Port already in use**: Run `lsof -i :3000` and kill the process
- **Node version mismatch**: Use `nvm use` or update Node version
- **Cache issues**: Delete `node_modules` and reinstall, or run `npm run clean`

### Getting Help
```bash
npm run doctor  # Check for common issues
```
