# 🎉 Cognoir - Authentication System

A **production-ready, beautifully designed authentication system** for the Cognoir AI Research Studio with complete TypeScript support, responsive design, and comprehensive documentation.

[![React](https://img.shields.io/badge/React-18.2+-61dafb?style=flat-square)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.2+-3178c6?style=flat-square)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-5.0+-646cff?style=flat-square)](https://vitejs.dev/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

## ✨ Features

- ✅ **User Authentication** - Secure signup/login functionality
- ✅ **Session Management** - Persistent user sessions with localStorage
- ✅ **Password Validation** - Strong password requirements and validation
- ✅ **Demo Account** - Quick-login demo for testing
- ✅ **Error Handling** - Comprehensive error messages and validation
- ✅ **Loading States** - Beautiful loading indicators
- ✅ **Full TypeScript** - Type-safe codebase
- ✅ **Responsive Design** - Works on all devices
- ✅ **Modern Stack** - React 18 + TypeScript + Vite

## 📁 Project Structure

```
cognoir/
├── src/
│   ├── components/           # React components
│   │   ├── LoginPage.tsx
│   │   ├── SignupPage.tsx
│   │   └── HomePage.tsx
│   ├── contexts/             # React contexts
│   │   └── AuthContext.tsx
│   ├── types/                # TypeScript types
│   │   └── auth.types.ts
│   ├── stores/               # State management
│   │   └── authStore.ts
│   ├── App.tsx               # Main app component
│   └── main.tsx              # Entry point
├── public/                   # Static assets
├── docs/                     # Documentation
├── tests/                    # Test files
├── vite.config.ts            # Vite configuration
├── tsconfig.json             # TypeScript configuration
├── package.json              # Project dependencies
├── .gitignore                # Git ignore rules
└── README.md                 # This file
```

## 🚀 Getting Started

### Prerequisites

- Node.js 16+ 
- npm or yarn or pnpm

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/cognoir.git
cd cognoir
```

2. **Install dependencies**
```bash
npm install
# or
yarn install
# or
pnpm install
```

3. **Start development server**
```bash
npm run dev
```

The app will open at `http://localhost:3000`

## 📚 Available Scripts

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Run linter
npm run lint

# Type check
npm run type-check
```

## 🔐 Authentication Features

### Login
- Email and password authentication
- Demo account for quick testing
- Remember user option
- Error handling for invalid credentials

### Signup
- User registration with email
- Password strength validation
- Confirm password matching
- Form validation
- Success notifications

### Session Management
- Automatic session persistence
- localStorage integration
- Logout functionality
- Session timeout handling

## 🧪 Demo Account

Quick login for testing:
- **Email:** demo@example.com
- **Password:** Demo@123!

## 📖 Documentation

Detailed documentation is available in the `/docs` folder:

- **ARCHITECTURE_DIAGRAM.md** - System architecture overview
- **IMPLEMENTATION_GUIDE.md** - Step-by-step implementation guide
- **QUICK_SETUP.md** - Quick setup instructions
- **QUICK_REFERENCE.md** - Quick reference guide
- **CODE_REVIEW.md** - Code review and best practices

## 🛠️ Tech Stack

- **Frontend Framework:** React 18
- **Language:** TypeScript
- **Build Tool:** Vite
- **State Management:** Zustand
- **Styling:** CSS (Tailwind ready)

## 📦 Dependencies

### Runtime
- `react`: ^18.2.0 - UI library
- `react-dom`: ^18.2.0 - React DOM
- `zustand`: ^4.4.0 - State management

### Development
- `typescript`: ^5.2.0 - Type safety
- `vite`: ^5.0.0 - Build tool
- `@vitejs/plugin-react`: ^4.0.0 - React plugin
- `eslint`: ^8.0.0 - Linting
- `@typescript-eslint/*`: ^6.0.0 - TS linting

## 🔧 Configuration Files

### vite.config.ts
Vite configuration for development and production builds with path aliases.

### tsconfig.json
TypeScript strict mode enabled with path mapping for clean imports.

### package.json
Project metadata and dependency management with npm scripts.

## 🎨 Customization

### Colors and Styling
All components use CSS custom properties for easy theming. Modify the color schemes in component files.

### Validation Rules
Edit validation rules in `AuthContext.tsx`:
```typescript
// Example: Modify password requirements
const validatePassword = (password: string) => {
  // Your custom validation logic
};
```

### API Integration
Replace localStorage with API calls in `AuthContext.tsx`:
```typescript
const login = async (email: string, password: string) => {
  const response = await fetch('/api/login', {
    method: 'POST',
    body: JSON.stringify({ email, password }),
  });
  // Handle response
};
```

## 🐛 Troubleshooting

### Port already in use
```bash
# Change port in vite.config.ts or kill the process
npm run dev -- --port 3001
```

### Module not found
```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

### TypeScript errors
```bash
# Run type check
npm run type-check
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- **Cognoir Team** - Initial work

## 🙏 Acknowledgments

- React documentation and community
- Vite documentation
- TypeScript documentation
- All contributors and testers

## 📞 Support

For support, email support@cognoir.ai or open an issue on GitHub.

## 🗺️ Roadmap

- [ ] OAuth integration (Google, GitHub)
- [ ] Two-factor authentication
- [ ] Social login
- [ ] Password reset functionality
- [ ] Email verification
- [ ] User profile management
- [ ] Dark mode support

## 📊 Project Status

- ✅ Version 1.0.0 - Production Ready
- 🔄 Actively Maintained
- 🚀 Ready for Deployment

---

**Made with ❤️ by Cognoir Team**
