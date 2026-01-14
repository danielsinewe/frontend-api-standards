# Coding Standards Reference

## Quick Reference

### Frontend Components (`components/`)

| Standard | Requirement | Tool |
|----------|-------------|------|
| Styling | Always use Tailwind | Tailwind CSS |
| Animations | Always use Framer Motion | Framer Motion |
| Naming | PascalCase, descriptive | - |

### API Validation (`app/api/`)

| Standard | Requirement | Tool |
|----------|-------------|------|
| Validation | Use Zod for all validation | Zod |
| Return Types | Define with Zod schemas | Zod |
| Type Exports | Export inferred types | `z.infer<typeof schema>` |

## Checklist

### Before Creating a Component
- [ ] Component name uses PascalCase
- [ ] File name matches component name
- [ ] Using Tailwind for styling (no inline styles)
- [ ] Using Framer Motion for animations (if needed)
- [ ] Component is typed with TypeScript

### Before Creating an API Endpoint
- [ ] Input validation schema defined with Zod
- [ ] Response schema defined with Zod
- [ ] Types exported using `z.infer`
- [ ] All inputs validated before processing
- [ ] Response validated before returning

## Common Patterns

### Component with Animation
```tsx
import { motion } from 'framer-motion';

export const AnimatedCard = ({ children }: { children: React.ReactNode }) => {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
      className="p-6 bg-white rounded-lg shadow-md"
    >
      {children}
    </motion.div>
  );
};
```

### API Route with Full Validation
```typescript
import { z } from 'zod';

// Input schema
const InputSchema = z.object({
  // ... fields
});

// Response schema
const ResponseSchema = z.object({
  // ... fields
});

// Export types
export type Input = z.infer<typeof InputSchema>;
export type Response = z.infer<typeof ResponseSchema>;

// Route handler
export async function POST(request: Request) {
  try {
    const body = await request.json();
    const input = InputSchema.parse(body);
    
    // ... logic ...
    
    const response: Response = {
      // ... data
    };
    
    return Response.json(ResponseSchema.parse(response));
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json({ error: error.errors }, { status: 400 });
    }
    throw error;
  }
}
```
