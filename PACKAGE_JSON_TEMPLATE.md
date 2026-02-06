# Example Package.json for SQLoot Projects

This document provides a template `package.json` for SQLoot projects using Bun.

## Basic Template

```json
{
  "name": "@sqloot/project-name",
  "version": "0.1.0",
  "description": "Brief description of your project",
  "type": "module",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "types": "./dist/index.d.ts",
      "import": "./dist/index.js"
    }
  },
  "scripts": {
    "dev": "bun run --watch src/index.ts",
    "build": "bun build src/index.ts --outdir dist --target bun",
    "start": "bun run dist/index.js",
    "test": "bun test",
    "test:watch": "bun test --watch",
    "lint": "bunx @biomejs/biome lint .",
    "format": "bunx @biomejs/biome format --write .",
    "check": "bunx @biomejs/biome check --apply .",
    "typecheck": "bunx tsc --noEmit",
    "clean": "rm -rf dist",
    "prepack": "bun run build"
  },
  "keywords": [
    "sqloot",
    "typescript",
    "bun"
  ],
  "author": "SQLoot Team",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/SQLoot/project-name.git"
  },
  "bugs": {
    "url": "https://github.com/SQLoot/project-name/issues"
  },
  "homepage": "https://github.com/SQLoot/project-name#readme",
  "devDependencies": {
    "@biomejs/biome": "latest",
    "@types/bun": "latest",
    "typescript": "^5.3.0"
  },
  "dependencies": {},
  "peerDependencies": {},
  "engines": {
    "node": ">=20.0.0"
  },
  "publishConfig": {
    "access": "public"
  }
}
```

## Script Explanations

### Development
- `dev`: Run TypeScript directly with hot reload
- `build`: Compile TypeScript to JavaScript
- `start`: Run the compiled JavaScript

### Testing
- `test`: Run all tests once
- `test:watch`: Run tests in watch mode

### Code Quality
- `lint`: Check code for linting issues
- `format`: Auto-format code
- `check`: Run all Biome checks and auto-fix
- `typecheck`: Type check without emitting files

### Utilities
- `clean`: Remove build artifacts
- `prepack`: Run before npm publish

## Common Additions

### For Libraries
```json
{
  "files": [
    "dist",
    "README.md",
    "LICENSE"
  ],
  "sideEffects": false
}
```

### For CLI Tools
```json
{
  "bin": {
    "my-cli": "./dist/cli.js"
  }
}
```

### With Database (e.g., Drizzle)
```json
{
  "scripts": {
    "db:generate": "drizzle-kit generate:sqlite",
    "db:migrate": "bun run src/db/migrate.ts",
    "db:studio": "drizzle-kit studio"
  },
  "dependencies": {
    "drizzle-orm": "latest"
  },
  "devDependencies": {
    "drizzle-kit": "latest"
  }
}
```

### With Web Framework (e.g., Hono)
```json
{
  "scripts": {
    "dev": "bun run --hot src/index.ts"
  },
  "dependencies": {
    "hono": "latest"
  }
}
```

## Best Practices

### Version Management
- Use `^` for dependencies to allow minor/patch updates
- Use exact versions for critical dependencies
- Keep `@types/bun` and `typescript` up to date

### Scripts
- Keep scripts simple and composable
- Use `bunx` for one-off tools (no install needed)
- Prefix database/infra scripts with category (`db:`, `infra:`)

### Dependencies
- Minimize dependencies when possible
- Prefer Bun's built-in APIs over external packages
- Audit dependencies regularly with `bun audit`

### Publishing
- Always include `LICENSE` and `README.md`
- Use `files` field to control what's published
- Test package installation before publishing

## Biome Configuration

Create `biome.json` in your project root:

```json
{
  "$schema": "https://biomejs.dev/schemas/1.5.0/schema.json",
  "organizeImports": {
    "enabled": true
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "style": {
        "useNodejsImportProtocol": "error"
      },
      "suspicious": {
        "noExplicitAny": "warn"
      }
    }
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 100
  },
  "javascript": {
    "formatter": {
      "quoteStyle": "single",
      "trailingComma": "es5",
      "semicolons": "asNeeded"
    }
  }
}
```

## TypeScript Configuration

Create `tsconfig.json` in your project root:

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "lib": ["ESNext"],
    "moduleResolution": "bundler",
    "types": ["bun-types"],
    
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

## Testing with Bun

Create test files with `.test.ts` suffix:

```typescript
import { describe, it, expect } from 'bun:test'

describe('Feature', () => {
  it('should work correctly', () => {
    expect(true).toBe(true)
  })
})
```

## Example Project Structure

```
project-name/
├── src/
│   ├── index.ts
│   ├── lib/
│   └── utils/
├── tests/
│   └── index.test.ts
├── dist/           # Build output (gitignored)
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
├── biome.json
├── tsconfig.json
├── package.json
├── bun.lockb
├── .gitignore
├── README.md
└── LICENSE
```

## Related Resources

- [Bun Documentation](https://bun.sh/docs)
- [Biome Documentation](https://biomejs.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [SQLoot Contributing Guide](CONTRIBUTING.md)
