# 🏗️ Login System Architecture & Data Flow

---

## 📊 System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         App.tsx                                 │
│                  (Main Entry Point)                             │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│              AuthProvider (AuthContext.tsx)                     │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │  State:                                                   │ │
│  │  • user: User | null                                      │ │
│  │  • loading: boolean                                       │ │
│  │  • error: string | null                                   │ │
│  │                                                            │ │
│  │  Methods:                                                 │ │
│  │  • login(email, password)                                 │ │
│  │  • signup(email, password, name)                          │ │
│  │  • logout()                                               │ │
│  │  • clearError()                                           │ │
│  └───────────────────────────────────────────────────────────┘ │
└────────────────────────┬────────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
   [LoginPage]      [SignupPage]     [HomePage]
   • Email input    • Name input     • Notebook grid
   • Password       • Email input    • Create button
   • Login btn      • Password       • Logout btn
   • Toggle signup  • Confirm pwd    • User profile
   • Demo account   • Strength bar
                    • Terms link

        └────────────────┬────────────────┘
                         │
                         ▼
        ┌────────────────────────────────┐
        │    Authenticated State          │
        │    ✓ user object loaded         │
        │    ✓ isAuthenticated = true     │
        └────────────────────────────────┘
                         │
                         ▼
        ┌────────────────────────────────┐
        │   [NotebookView]               │
        │   • Sources management         │
        │   • Chat interface             │
        │   • Notes creation             │
        │   • Export functionality       │
        └────────────────────────────────┘
```

---

## 🔄 Authentication Flow

### Complete User Journey:

```
START
  │
  ▼
┌─────────────────────────────┐
│ Check localStorage for user │
│ (AuthContext initialization)│
└─────────────────────────────┘
        │
        ├─ User found? ─┐
        │               │
        No              Yes
        │               │
        ▼               ▼
┌──────────────┐  ┌──────────────────┐
│ Show Login   │  │ Load user state  │
│ / Signup     │  │ isAuth = true    │
│ Pages        │  │ Show HomePage    │
└──────────────┘  └──────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  User clicks "Sign up"      │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  Fill signup form:          │
│  • Name                     │
│  • Email                    │
│  • Password (6+ chars)      │
│  • Confirm password         │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  Validate inputs:           │
│  • Email format ✓           │
│  • Password length ✓        │
│  • Passwords match ✓        │
│  • No duplicates ✓          │
└─────────────────────────────┘
        │
        ├─ All valid? ─┐
        │              │
       Yes             No
        │              │
        ▼              ▼
    Create       Show Error
    Account
        │              │
        ├──────┬───────┘
        │      │
        ▼      ▼
    ┌──────────────────┐
    │ Save to storage: │
    │ cognoir_users    │
    └──────────────────┘
        │
        ▼
    ┌──────────────────┐
    │ Show "Success!"  │
    │ Redirect to Login│
    └──────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  User clicks "Login"        │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  Fill login form:           │
│  • Email                    │
│  • Password                 │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  Validate credentials:      │
│  • Email exists? ✓          │
│  • Password correct? ✓      │
└─────────────────────────────┘
        │
        ├─ Match? ─┐
        │          │
       Yes         No
        │          │
        ▼          ▼
    Load       Show Error
    User
        │          │
        ├──────┬───┘
        │      │
        ▼      ▼
    ┌──────────────────────┐
    │ Save to storage:     │
    │ cognoir_auth_user    │
    │ Update lastLogin     │
    └──────────────────────┘
        │
        ▼
    ┌──────────────────┐
    │ Update context:  │
    │ user = userData  │
    │ auth = true      │
    └──────────────────┘
        │
        ▼
    ┌──────────────────┐
    │ Show HomePage    │
    │ with user info   │
    └──────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  User creates notebook      │
│  Adds sources              │
│  Chats with AI             │
│  Creates notes             │
│  (All stored per user)     │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  Page refresh / Close tab   │
│  Notebook data saved in:    │
│  cognoir_notebooks (global) │
│  User session saved in:     │
│  cognoir_auth_user          │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  User returns to app        │
│  AuthContext loads user     │
│  from storage               │
│  Auto-logged in! ✓          │
└─────────────────────────────┘
        │
        ▼
┌─────────────────────────────┐
│  User clicks Logout         │
│  Clear auth_user from       │
│  localStorage               │
│  Set user = null            │
│  isAuthenticated = false    │
└─────────────────────────────┘
        │
        ▼
    Show Login Page
    (Cycle repeats)

END
```

---

## 💾 Data Flow - Local Storage

```
BEFORE LOGIN:
┌──────────────────────────────────────┐
│        browser localStorage          │
│                                      │
│  cognoir_users: [                    │
│    {id, email, password, name, ...}  │ ← All users
│  ]                                   │
│                                      │
│  cognoir_notebooks: [                │
│    {id, title, sources, ...}         │ ← Shared
│  ]                                   │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│      App State (AuthContext)         │
│                                      │
│  user: null                          │
│  isAuthenticated: false              │
│  loading: true (checking storage)    │
└──────────────────────────────────────┘


DURING LOGIN (email: user@test.com, password: pass123):
┌──────────────────────────────────────┐
│  Search cognoir_users for match:     │
│  ✓ email exists: user@test.com       │
│  ✓ password matches: pass123         │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│  Extract user from storage:          │
│  {                                   │
│    id: "uuid-123",                   │
│    email: "user@test.com",           │
│    name: "John Researcher",          │
│    createdAt: "2024-01-15...",       │
│    lastLogin: "2024-01-15..."        │
│  }                                   │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│  Save to cognoir_auth_user:          │
│  (Only current user's data)          │
│                                      │
│  Update lastLogin timestamp          │
│  Save JSON to localStorage           │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│      App State (AuthContext)         │
│                                      │
│  user: {                             │
│    id: "uuid-123",                   │
│    email: "user@test.com",           │
│    name: "John Researcher",          │
│    createdAt: new Date(...),         │
│    lastLogin: new Date(...)          │
│  }                                   │
│  isAuthenticated: true               │
│  loading: false                      │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│   Render HomePage with:              │
│   • User profile (name, email)       │
│   • Create notebook button           │
│   • Notebook grid                    │
│   • Logout button                    │
└──────────────────────────────────────┘


AFTER LOGOUT:
┌──────────────────────────────────────┐
│  cognoir_auth_user: DELETED          │
│  (cleared from localStorage)         │
│                                      │
│  cognoir_users: [...]                │
│  (unchanged - other users stored)    │
│                                      │
│  cognoir_notebooks: [...]            │
│  (unchanged - user's data preserved) │
└──────────────────────────────────────┘
        │
        ▼
┌──────────────────────────────────────┐
│      App State (AuthContext)         │
│                                      │
│  user: null                          │
│  isAuthenticated: false              │
│  loading: false                      │
│  error: null                         │
└──────────────────────────────────────┘
        │
        ▼
    Show Login Page
```

---

## 🔐 State Management Flow

```
AuthContext
│
├── State
│   ├── user: User | null
│   ├── loading: boolean
│   ├── error: string | null
│   └── isAuthenticated: boolean (derived)
│
├── State Setters
│   ├── setUser()
│   ├── setLoading()
│   └── setError()
│
├── Methods
│   ├── login(email, password)
│   │   ├── validate inputs
│   │   ├── search users
│   │   ├── verify password
│   │   ├── save to storage
│   │   └── update state
│   │
│   ├── signup(email, password, name)
│   │   ├── validate inputs
│   │   ├── check duplicates
│   │   ├── create new user
│   │   ├── save to storage
│   │   └── update state
│   │
│   ├── logout()
│   │   ├── clear auth_user
│   │   ├── reset state
│   │   └── update UI
│   │
│   └── clearError()
│       └── set error to null
│
└── Context Provider
    └── Wraps App with AuthProvider
        └── Makes useAuth() available to all components
```

---

## 🎯 Component Hierarchy

```
main.tsx
  │
  └── AuthProvider (AuthContext.tsx)
       │
       └── App.tsx
            │
            ├── LoginPage.tsx (if not authenticated)
            │   ├── LoginForm
            │   ├── Email Input
            │   ├── Password Input
            │   ├── Error Display
            │   └── Link to SignupPage
            │
            ├── SignupPage.tsx (if not authenticated)
            │   ├── SignupForm
            │   ├── Name Input
            │   ├── Email Input
            │   ├── Password Input
            │   ├── Confirm Password Input
            │   ├── Password Strength
            │   ├── Error Display
            │   └── Link to LoginPage
            │
            ├── HomePage.tsx (if authenticated)
            │   ├── Header
            │   │   ├── Logo
            │   │   ├── User Profile
            │   │   └── Logout Button
            │   │
            │   ├── Create Notebook Section
            │   │   ├── Emoji Picker
            │   │   ├── Title Input
            │   │   └── Create Button
            │   │
            │   └── Notebook Grid
            │       ├── Notebook Card 1
            │       ├── Notebook Card 2
            │       └── Notebook Card N
            │
            └── NotebookView.tsx (if notebook selected)
                ├── Notebook Header
                ├── Sources Panel
                ├── Chat Interface
                └── Notes Section
```

---

## 📦 localStorage Structure

```javascript
// After user signup/login:

localStorage = {
  // All registered users
  "cognoir_users": [
    {
      "id": "uuid-1",
      "email": "demo@cognoir.ai",
      "password": "demo123",
      "name": "Demo User",
      "createdAt": "2024-01-15T10:30:00Z",
      "lastLogin": "2024-01-15T14:45:00Z"
    },
    {
      "id": "uuid-2",
      "email": "user@example.com",
      "password": "password123",
      "name": "John Researcher",
      "createdAt": "2024-01-16T09:15:00Z",
      "lastLogin": "2024-01-16T14:20:00Z"
    }
  ],

  // Currently logged in user
  "cognoir_auth_user": {
    "id": "uuid-2",
    "email": "user@example.com",
    "name": "John Researcher",
    "createdAt": "2024-01-16T09:15:00Z",
    "lastLogin": "2024-01-16T14:20:00Z"
    // Note: password NOT stored here (only in cognoir_users)
  },

  // User's notebooks (shared across sessions)
  "cognoir_notebooks": [
    {
      "id": "nb-1",
      "title": "Machine Learning",
      "description": "",
      "emoji": "🧠",
      "createdAt": "2024-01-16T10:00:00Z",
      "updatedAt": "2024-01-16T14:20:00Z",
      "sources": [...],
      "messages": [...],
      "notes": [...]
    },
    {
      "id": "nb-2",
      "title": "Quantum Computing",
      "description": "",
      "emoji": "⚛️",
      "createdAt": "2024-01-16T11:00:00Z",
      "updatedAt": "2024-01-16T12:30:00Z",
      "sources": [],
      "messages": [],
      "notes": []
    }
  ]
}
```

---

## 🔀 Conditional Rendering Logic

```
App.tsx Logic:

┌─ Is user loading?
│  ├─ YES → Show loading spinner
│  └─ NO  → Continue
│
├─ Is user authenticated?
│  ├─ NO
│  │  ├─ On login page?
│  │  │  ├─ YES → Show LoginPage
│  │  │  └─ NO  → Show SignupPage
│  │  │
│  │  └─ User clicks switch
│  │     └─ Toggle between login/signup
│  │
│  └─ YES (authenticated)
│     ├─ Is notebook selected?
│     │  ├─ YES → Show NotebookView
│     │  └─ NO  → Show HomePage
│     │
│     └─ User clicks logout
│        └─ Clear auth → Show login page
```

---

## ⏱️ Timing Diagram

```
User Action Timeline:

1. Load App
   │
   └─► AuthContext checks localStorage (50ms)
       ├─► User found → setLoading(false), showApp
       └─► No user → setLoading(false), showLogin
       
2. Signup Form Submission
   │
   ├─► setLoading(true)
   ├─► Validate inputs (synchronous)
   ├─► Simulate API delay (500ms)
   ├─► Save to localStorage (synchronous)
   ├─► setLoading(false)
   └─► showSuccess → redirect to login (1500ms)
   
3. Login Form Submission
   │
   ├─► setLoading(true)
   ├─► Simulate API delay (500ms)
   ├─► Verify password (synchronous)
   ├─► Save to storage (synchronous)
   ├─► setLoading(false)
   └─► showApp (immediate)
   
4. Page Refresh (while logged in)
   │
   ├─► AuthContext loads from storage (50ms)
   ├─► Restore user object (synchronous)
   ├─► Convert Date strings → Date objects
   ├─► Set state
   └─► Show App (seamless)
   
5. Logout
   │
   ├─► clearAuthUser from storage (synchronous)
   ├─► setUser(null)
   ├─► setLoading(false)
   └─► Show LoginPage (immediate)
```

---

## 🎓 Key Concepts Explained

### 1. **React Context API**
```
Context = Global State Container
│
├── AuthContext.Provider (top level)
│   │
│   └── All child components can access auth state
│       via useAuth() hook
│
└── No prop drilling needed
```

### 2. **useCallback Hook**
```
useCallback = Memoized function
│
├── Prevents unnecessary re-renders
├── Maintains reference equality
├── Dependencies array controls when function updates
│
└── Used for: login, signup, logout, etc.
```

### 3. **localStorage**
```
localStorage = Browser Storage
│
├── Survives page refreshes
├── Survives browser restarts
├── Domain-specific (one per domain)
├── ~5-10MB per domain
│
└── Perfect for session persistence
```

### 4. **Type Safety with TypeScript**
```
TypeScript = Runtime Errors → Compile Errors
│
├── Catch bugs before runtime
├── Better IDE autocomplete
├── Self-documenting code
│
└── authContext.user?.email (safe access)
    authContext.user.email (required)
```

---

## 📈 Scalability Considerations

### Current (localStorage):
- ✅ Fine for single user per device
- ✅ Fine for ~1000 user accounts
- ❌ Not shared across devices
- ❌ No real-time sync

### With Backend (MongoDB):
- ✅ Multi-device support
- ✅ Unlimited users
- ✅ Real-time sync
- ✅ Backup & recovery
- ✅ Security improvements

### Migration Path:
```
Phase 1: Current (localStorage only)
         ↓
Phase 2: localStorage + Backend API
         ↓
Phase 3: Full Backend (remove localStorage)
         ↓
Phase 4: Add Real-time (WebSockets)
         ↓
Phase 5: Add Advanced Features
         (2FA, OAuth, Social Login, etc.)
```

---

## 🎯 Summary

```
┌──────────────────────────────────────────────────┐
│         Complete Login System Flow               │
├──────────────────────────────────────────────────┤
│                                                  │
│  1. User visits app                              │
│     → AuthContext loads from localStorage        │
│                                                  │
│  2a. No user found                               │
│      → Show Login/Signup forms                   │
│                                                  │
│  2b. User found                                  │
│      → Auto-login & show HomePage               │
│                                                  │
│  3. User signup/login                            │
│     → Validate → Save to storage → Update state │
│                                                  │
│  4. User uses app                                │
│     → Create/edit notebooks                     │
│     → Data auto-saves to localStorage           │
│                                                  │
│  5. User logout                                  │
│     → Clear storage → Reset state → Show login  │
│                                                  │
│  6. Page refresh                                 │
│     → Repeat step 1 → Seamless experience       │
│                                                  │
└──────────────────────────────────────────────────┘
```

Perfect for understanding the complete system! 🚀
