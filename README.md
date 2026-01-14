# Frontend Components & API Validation Standards

This repository defines coding standards and best practices for frontend components and API validation.

## Overview

This rule provides standards for:
- **Frontend Components**: Styling, animations, and naming conventions
- **API Validation**: Schema validation, return types, and type exports

## Frontend Components Standards

### Location: `components/` directory

When working in the components directory, follow these standards:

#### 1. Styling
- **Always use Tailwind CSS** for all styling
- Avoid inline styles or CSS modules unless absolutely necessary
- Use Tailwind utility classes for consistent design

#### 2. Animations
- **Use Framer Motion** for all animations
- Prefer Framer Motion over CSS animations or other animation libraries
- Keep animations performant and accessible

#### 3. Naming Conventions
- Use PascalCase for component names (e.g., `UserProfile.tsx`, `NavigationBar.tsx`)
- Match component file names to component names
- Use descriptive, semantic names that indicate component purpose

### Example Component Structure

```tsx
import { motion } from 'framer-motion';

export const UserProfile = () => {
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      className="flex items-center gap-4 p-4 bg-white rounded-lg shadow-md"
    >
      {/* Component content */}
    </motion.div>
  );
};
```

## API Validation Standards

### Location: `app/api/` directory

When working in the API directory, follow these standards:

#### 1. Validation
- **Use Zod** for all input validation
- Validate all request bodies, query parameters, and path parameters
- Define schemas before route handlers

#### 2. Return Types
- **Define return types with Zod schemas**
- Create response schemas for all API endpoints
- Ensure type safety for API responses

#### 3. Type Exports
- **Export types generated from schemas**
- Use `z.infer<typeof schema>` to generate TypeScript types
- Export types alongside schemas for reuse

### Example API Route Structure

```typescript
import { z } from 'zod';

// Define input schema
const CreateUserSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
  age: z.number().int().positive().optional(),
});

// Define response schema
const UserResponseSchema = z.object({
  id: z.string(),
  name: z.string(),
  email: z.string(),
  createdAt: z.string().datetime(),
});

// Export types
export type CreateUserInput = z.infer<typeof CreateUserSchema>;
export type UserResponse = z.infer<typeof UserResponseSchema>;

// Use in route handler
export async function POST(request: Request) {
  const body = await request.json();
  const validatedInput = CreateUserSchema.parse(body);
  
  // ... business logic ...
  
  const response: UserResponse = {
    id: '123',
    name: validatedInput.name,
    email: validatedInput.email,
    createdAt: new Date().toISOString(),
  };
  
  return Response.json(UserResponseSchema.parse(response));
}
```

## Best Practices

### Frontend Components
- Keep components focused and single-purpose
- Extract reusable logic into custom hooks
- Use TypeScript for type safety
- Write accessible HTML with proper ARIA attributes

### API Validation
- Validate early, fail fast
- Provide clear error messages for validation failures
- Use Zod's built-in error handling
- Consider using middleware for common validation patterns

## Contributing

When adding new components or API endpoints:
1. Follow the standards outlined in this document
2. Ensure all validation is in place
3. Export types for external use
4. Update this README if standards evolve

## License

[Specify your license here]
