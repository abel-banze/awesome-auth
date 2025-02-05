
# Awesome Auth

A lightweight, secure, and easy-to-use authentication library for Node.js and Next.js applications. Designed to provide core authentication features with minimal complexity.

---

## Features

- **Simple API**: Easy to set up and use.
- **Token-Based Authentication**: Uses JWT for stateless sessions.
- **Customizable Storage**: Supports MongoDB, PostgreSQL, or in-memory storage.
- **Secure by Default**: Passwords are hashed using bcrypt.
- **Extensible**: Easily add OAuth, MFA, or other advanced features.

---

## Installation

Install the package using npm:

```bash
npm install awesome-auth
```

Or using yarn:

```bash
yarn add awesome-auth
```

---

## Quick Start

### 1. Create an Authentication Instance

Create a configuration file for your authentication setup:

```typescript
// lib/authConfig.ts
import { createAuth } from 'awesome-auth';

const auth = createAuth({
  secret: 'your_jwt_secret', // Replace with a secure secret
  storageType: 'mongo',      // Use 'memory' for development
  dbUri: 'mongodb://localhost:27017/myapp', // MongoDB URI (optional for 'memory')
});

export default auth;
```

### 2. Register and Login Users

Use the `register` and `login` methods to manage user accounts.

#### Example: Register a New User

```typescript
await auth.register('john.doe', 'password123');
```

#### Example: Log In a User

```typescript
const token = await auth.login('john.doe', 'password123');
console.log(token); // JWT token
```

### 3. Protect Routes with Middleware

Use the `middleware` method to protect routes.

#### Example: Protect an API Route

```typescript
// pages/api/protected.ts
import { NextApiRequest, NextApiResponse } from 'next';
import auth from '../../lib/authConfig';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  auth.middleware(req, res, () => {
    res.status(200).json({ message: 'This is a protected route', user: req.user });
  });
}
```

---

## Configuration

### Options

When creating an instance of `awesome-auth`, you can pass the following options:

| Option       | Type     | Description                                                                 | Required |
|--------------|----------|-----------------------------------------------------------------------------|----------|
| `secret`     | `string` | A secure secret key used to sign JWT tokens.                               | Yes      |
| `storageType`| `string` | The type of storage backend (`mongo`, `postgres`, or `memory`).            | Yes      |
| `dbUri`      | `string` | The database connection URI (required for `mongo` or `postgres`).         | Optional |

---

## API Reference

### Methods

#### `createAuth(config: AuthConfig): SimpleAuth`

Creates an instance of the authentication system.

- **Parameters**:
  - `config`: An object containing the configuration options.

#### `register(username: string, password: string): Promise<void>`

Registers a new user.

- **Parameters**:
  - `username`: The username for the new user.
  - `password`: The password for the new user.

#### `login(username: string, password: string): Promise<string>`

Logs in a user and returns a JWT token.

- **Parameters**:
  - `username`: The username of the user.
  - `password`: The password of the user.

#### `verify(token: string): any`

Verifies a JWT token and returns the decoded payload.

- **Parameters**:
  - `token`: The JWT token to verify.

#### `middleware(req: any, res: any, next: any): void`

Middleware function to protect routes.

- **Parameters**:
  - `req`: The request object.
  - `res`: The response object.
  - `next`: The next middleware function.

---

## Storage Backends

### MongoDB

To use MongoDB as the storage backend, set `storageType` to `'mongo'` and provide a valid `dbUri`.

```typescript
createAuth({
  secret: 'your_jwt_secret',
  storageType: 'mongo',
  dbUri: 'mongodb://localhost:27017/myapp',
});
```

### In-Memory (For Development)

For quick development or testing, use the in-memory storage backend by setting `storageType` to `'memory'`.

```typescript
createAuth({
  secret: 'your_jwt_secret',
  storageType: 'memory',
});
```

---

## Example Usage in Next.js

Here’s how you can integrate `awesome-auth` into a Next.js application.

### 1. Create an API Route for Login

```typescript
// pages/api/login.ts
import { NextApiRequest, NextApiResponse } from 'next';
import auth from '../../lib/authConfig';

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method !== 'POST') return res.status(405).send('Method Not Allowed');

  const { username, password } = req.body;

  try {
    const token = await auth.login(username, password);
    res.status(200).json({ token });
  } catch (error) {
    res.status(401).send('Invalid credentials');
  }
}
```

### 2. Protect a Route

```typescript
// pages/api/protected.ts
import { NextApiRequest, NextApiResponse } from 'next';
import auth from '../../lib/authConfig';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  auth.middleware(req, res, () => {
    res.status(200).json({ message: 'This is a protected route', user: req.user });
  });
}
```

---

## Security Considerations

- **Use a Strong Secret Key**: Always use a long, random, and securely generated secret key for signing JWT tokens.
- **Hash Passwords**: Passwords are automatically hashed using bcrypt, but ensure that your database schema enforces strong password policies.
- **Protect Tokens**: Store JWT tokens securely on the client side (e.g., in `localStorage` or `HttpOnly cookies`).

---

## Contributing

Contributions are welcome! If you find a bug or have an idea for improvement, please open an issue or submit a pull request.

---

## License

MIT License

Copyright (c) [2025] [Abel Banze]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.