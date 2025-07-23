# 🤝 Contributing to Automated Calorie Counter

First off, thank you for considering contributing to the Automated Calorie Counter! It's people like you that will make this a great tool for everyone.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Process](#development-process)
- [Style Guidelines](#style-guidelines)
- [Testing Guidelines](#testing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

## 📜 Code of Conduct

This project and everyone participating in it is governed by our Code of Conduct. By participating, you are expected to uphold this code. Please report unacceptable behavior to the project maintainers.

### Our Standards

- Be respectful and inclusive
- Welcome newcomers and help them get started
- Focus on what is best for the community
- Show empathy towards other community members

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ and npm
- Git
- A code editor (we recommend VS Code)
- Basic knowledge of React and TypeScript

### Setting up your development environment

1. **Fork the repository**
   ```bash
   # Click the 'Fork' button on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/calorie-counter.git
   cd calorie-counter
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/ORIGINAL_OWNER/calorie-counter.git
   ```

4. **Install dependencies**
   ```bash
   npm install
   ```

5. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Add your OpenAI API key to .env
   ```

6. **Run the development server**
   ```bash
   npm run dev
   ```

## 💡 How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues to avoid duplicates. When you create a bug report, include:

- **Clear and descriptive title**
- **Steps to reproduce** the issue
- **Expected behavior** vs what actually happened
- **Screenshots** if applicable
- **System information** (OS, browser, version)

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion, include:

- **Clear and descriptive title**
- **Detailed description** of the proposed functionality
- **Use cases** for the enhancement
- **Mockups or examples** if applicable

### Code Contributions

#### Priority Areas

Based on our quality and performance assessments, these areas need immediate attention:

1. **🧪 Test Coverage** (CRITICAL)
   - We currently have 0% test coverage
   - Help us implement unit tests, integration tests, and E2E tests
   - Focus on critical components first

2. **🔒 Security & Validation**
   - Input validation and sanitization
   - XSS protection
   - Error boundary implementation

3. **⚡ Performance Optimization**
   - Bundle size reduction
   - Code splitting implementation
   - Converting inline styles to Tailwind classes

4. **♿ Accessibility**
   - Adding ARIA labels
   - Keyboard navigation support
   - Screen reader compatibility

5. **✨ New Features**
   - Photo-based food recognition
   - Voice input
   - Receipt scanning
   - Wearable integration

## 🔄 Development Process

### Branching Strategy

We use Git Flow for our branching strategy:

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - New features
- `bugfix/*` - Bug fixes
- `hotfix/*` - Urgent production fixes

### Creating a Feature Branch

```bash
git checkout develop
git pull upstream develop
git checkout -b feature/your-feature-name
```

### Commit Messages

We follow the Conventional Commits specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

Example:
```
feat(camera): add photo-based food recognition

Implemented integration with Google Vision API for food detection.
Users can now take photos to log meals automatically.

Closes #123
```

## 🎨 Style Guidelines

### TypeScript/JavaScript

We use ESLint and Prettier for code formatting. Run before committing:

```bash
npm run lint
npm run format
```

Key conventions:
- Use functional components with hooks
- Prefer TypeScript interfaces over types
- Use meaningful variable names
- Add JSDoc comments for complex functions

### CSS/Tailwind

- Use Tailwind classes instead of inline styles
- Follow mobile-first responsive design
- Maintain consistent spacing (use Tailwind's spacing scale)
- Use semantic color names from our palette

### Component Structure

```typescript
// ComponentName.tsx
import React from 'react';

interface ComponentNameProps {
  // Props definition
}

export const ComponentName: React.FC<ComponentNameProps> = ({ prop1, prop2 }) => {
  // Hooks
  
  // Event handlers
  
  // Render
  return (
    <div className="tailwind-classes">
      {/* Component content */}
    </div>
  );
};
```

## 🧪 Testing Guidelines

### Unit Tests

```typescript
// ComponentName.test.tsx
import { render, screen } from '@testing-library/react';
import { ComponentName } from './ComponentName';

describe('ComponentName', () => {
  it('should render correctly', () => {
    render(<ComponentName />);
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });
});
```

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage
npm test -- --coverage
```

### Test Coverage Requirements

- New features must include tests
- Aim for >80% code coverage
- Critical paths must have 100% coverage

## 🔀 Pull Request Process

1. **Update your branch**
   ```bash
   git fetch upstream
   git rebase upstream/develop
   ```

2. **Run quality checks**
   ```bash
   npm run lint
   npm run test
   npm run build
   ```

3. **Create Pull Request**
   - Use a clear, descriptive title
   - Reference any related issues
   - Include screenshots for UI changes
   - Describe your changes in detail

4. **PR Checklist**
   - [ ] Code follows style guidelines
   - [ ] Tests added/updated
   - [ ] Documentation updated
   - [ ] No console.logs or debugging code
   - [ ] Responsive design tested
   - [ ] Accessibility checked

5. **Review Process**
   - At least 1 approval required
   - All CI checks must pass
   - Resolve all review comments

## 🏗️ Project Structure

```
calorie-counter/
├── src/
│   ├── components/    # React components
│   │   ├── common/   # Reusable components
│   │   ├── features/ # Feature-specific components
│   │   └── layout/   # Layout components
│   ├── hooks/        # Custom React hooks
│   ├── services/     # API and external services
│   ├── utils/        # Helper functions
│   ├── types/        # TypeScript types/interfaces
│   └── styles/       # Global styles
├── tests/            # Test files
├── public/           # Static assets
└── docs/            # Documentation
```

## 👥 Community

### Getting Help

- **Discord**: Join our Discord server for real-time help
- **GitHub Discussions**: For longer-form discussions
- **Stack Overflow**: Tag questions with `calorie-counter`

### Weekly Meetings

We hold weekly community calls every Thursday at 5 PM UTC. Agenda and joining details are posted in GitHub Discussions.

### Recognition

Contributors who make significant contributions will be:
- Added to our Contributors list
- Highlighted in release notes
- Invited to become maintainers

## 📝 Documentation

When adding new features or making significant changes:

1. Update the README if needed
2. Add/update API documentation
3. Include inline code comments
4. Update FEATURES.md for new features
5. Add user documentation if applicable

## 🚀 Release Process

We follow semantic versioning (MAJOR.MINOR.PATCH):

- MAJOR: Breaking changes
- MINOR: New features
- PATCH: Bug fixes

Releases are automated through GitHub Actions when changes are merged to `main`.

## 🙏 Thank You!

Your contributions make this project better for everyone. We appreciate your time and effort in improving the Automated Calorie Counter!

---

If you have questions not covered here, please open a discussion or reach out to the maintainers. Happy coding! 🎉