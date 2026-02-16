# Contributing to COVID-19 Patient Outcome Prediction System

We welcome contributions to improve this project! This document provides guidelines for contributing.

## Code of Conduct

Be respectful, inclusive, and professional in all interactions.

## Getting Started

### Fork & Clone
```bash
git clone https://github.com/your-username/covid-prediction.git
cd covid-prediction
git checkout -b feature/your-feature-name
```

### Setup Development Environment
```bash
pnpm install
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows
pip install -r requirements.txt
pnpm dev
```

## Development Workflow

### 1. Create Feature Branch
```bash
git checkout -b feature/descriptive-name
```

### 2. Make Changes

- **Frontend:** Update `.tsx` files in `/app` or `/components`
- **Backend:** Update `/scripts` or API routes
- **ML Model:** Modify `/scripts/train_model.py`

### 3. Test Your Changes

```bash
# Frontend
pnpm dev  # Test in browser

# Backend
python scripts/train_model.py  # Train/test model

# Lint
pnpm lint

# Build
pnpm build
```

### 4. Commit Changes

```bash
git add .
git commit -m "feat: Add descriptive message"
```

Use conventional commits:
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation update
- `refactor:` Code refactoring
- `test:` Testing changes
- `chore:` Maintenance

### 5. Push & Create Pull Request

```bash
git push origin feature/descriptive-name
```

Create PR on GitHub with:
- Clear title
- Description of changes
- Reference to any related issues
- Screenshots for UI changes

## Code Standards

### TypeScript/JavaScript
- Use TypeScript for type safety
- Follow ESLint configuration
- Use meaningful variable names
- Add comments for complex logic

### Python
- Follow PEP 8 style guide
- Use type hints where possible
- Write docstrings for functions
- Use meaningful variable names

### React Components
- Use functional components with hooks
- Extract reusable logic into custom hooks
- Keep components focused and small
- Use proper error handling

## Testing

### Frontend
```bash
pnpm test
```

### Backend/ML
```bash
python -m pytest scripts/tests/
```

## Documentation

- Update README.md for major changes
- Add docstrings to Python functions
- Comment complex algorithms
- Update API documentation

## Pull Request Process

1. Ensure code follows standards
2. Add tests for new features
3. Update documentation
4. Request review from maintainers
5. Address review comments
6. Merge after approval

## Reporting Issues

### Bug Reports
Include:
- Python and Node.js version
- OS and browser (if frontend)
- Steps to reproduce
- Expected vs actual behavior
- Error messages/screenshots

### Feature Requests
Include:
- Clear description of the feature
- Use cases and benefits
- Possible implementation approach
- Related issues or external references

## Project Structure

```
covid-prediction/
├── app/                 # Next.js app directory
├── components/          # React components
├── scripts/            # Python scripts
├── public/             # Static files
├── tests/              # Test files
├── docs/               # Documentation
└── [config files]      # Configuration
```

## Git Workflow

```
main (production) ← develop (staging) ← feature branches
```

1. Create feature branch from `develop`
2. Submit PR to `develop`
3. After testing, merge to `main`
4. Maintain clean commit history

## Release Process

1. Update version in `package.json`
2. Update CHANGELOG.md
3. Create git tag
4. Push to GitHub
5. Create release notes
6. Deploy to production

## Communication

- **Issues:** For bugs and features
- **Discussions:** For questions and ideas
- **Email:** For sensitive matters

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Recognition

Contributors will be acknowledged in:
- CONTRIBUTORS.md
- Release notes
- GitHub contributors page

Thank you for contributing!
