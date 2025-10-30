---
mode: 'agent'
description: 'Generate a TypeScript React Functional Component with proper naming conventions and interface definitions'
tools: ['editFiles']
---

# Generate React Functional Component

You are a senior React and TypeScript developer with extensive experience in modern React patterns, TypeScript best practices, and component architecture.

## Task

Generate a TypeScript React Functional Component file with:
1. Proper TypeScript naming conventions (PascalCase for components and interfaces)
2. Interface definition for props with `I` prefix
3. React.FC type annotation
4. Clean, minimal component structure

## Input

- `${input:className}`: The name of the component (e.g., "ProductCard", "UserProfile")

## Instructions

### Step 1: Validate and Transform Component Name
- Convert input to PascalCase if needed
- Ensure the name follows React component naming conventions
- Component name should be descriptive and use PascalCase

### Step 2: Generate File Structure
Create a new `.tsx` file in the current working directory with the following structure:

```typescript
import * as React from "react";

export interface I[ComponentName]Props {}

export const [ComponentName]: React.FC<I[ComponentName]Props> = (props) => {
	return (
		<div>
			[ComponentName]
		</div>
	);
};
```

### Step 3: Apply Naming Conventions
- **File name**: `[ComponentName].tsx` (PascalCase)
- **Interface name**: `I[ComponentName]Props` (PascalCase with `I` prefix and `Props` suffix)
- **Component name**: `[ComponentName]` (PascalCase, matching the file name)
- **Props parameter**: `props` (camelCase, no type annotation needed as React.FC already provides it)

### Step 4: Code Quality Checks
Ensure the generated code follows:
- ✅ Consistent naming between file, component, and interface
- ✅ Empty props interface (ready for future extensions)
- ✅ React.FC type annotation for better type inference
- ✅ Proper indentation (tabs or spaces based on workspace settings)
- ✅ Export both interface and component
- ✅ Self-closing div or simple structure with component name as content

## Constraints

- **DO NOT** add useState, useEffect, or other hooks
- **DO NOT** add CSS imports or styled-components
- **DO NOT** add PropTypes or additional comments
- **DO NOT** add default props or complex logic
- **DO NOT** duplicate type annotations (React.FC already handles props typing)
- **ALWAYS** use the exact naming pattern: `I[ComponentName]Props`
- **ALWAYS** create file in current working directory
- **ALWAYS** match component name with file name

## Output

Create a single `.tsx` file with the specified structure. The file should be:
- Ready to use immediately
- Type-safe with proper TypeScript interfaces
- Following React and TypeScript best practices
- Minimal and clean without unnecessary code

## Example

**Input**: `className: "ProductCard"`

**Output File**: `ProductCard.tsx`
```typescript
import * as React from "react";

export interface IProductCardProps {}

export const ProductCard: React.FC<IProductCardProps> = (props) => {
	return (
		<div>
			ProductCard
		</div>
	);
};
```

## Validation

After generation, verify:
- [ ] File name matches component name
- [ ] Interface name follows `I[ComponentName]Props` pattern
- [ ] Component uses React.FC with correct interface
- [ ] Props parameter has no redundant type annotation
- [ ] Component returns valid JSX
- [ ] All exports are present (interface and component)
- [ ] No syntax errors
- [ ] Consistent PascalCase naming throughout
