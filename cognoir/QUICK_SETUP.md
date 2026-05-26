# 🚀 Quick Setup Checklist

## ✅ Files to Create/Update

### NEW FILES (Create these in src/)
```
[ ] src/auth.types.ts
[ ] src/AuthContext.tsx
[ ] src/components/LoginPage.tsx
[ ] src/components/SignupPage.tsx
```

### UPDATE EXISTING FILES
```
[ ] src/App.tsx (replace with new version)
[ ] src/main.tsx (replace with new version)
[ ] src/components/HomePage.tsx (rename to HomePageUpdated.tsx)
```

### KEEP AS-IS (No changes needed)
```
✓ src/store.ts
✓ src/types.ts
✓ src/index.css
✓ package.json
✓ package-lock.json
✓ tsconfig.json
✓ vite.config.ts
✓ index.html
```

---

## 📂 Final Directory Structure

```
your-project/
├── src/
│   ├── components/
│   │   ├── HomePage.tsx (UPDATED - add logout)
│   │   ├── LoginPage.tsx (NEW)
│   │   ├── SignupPage.tsx (NEW)
│   │   ├── NotebookView.tsx (existing)
│   │   └── ... (other components)
│   ├── AuthContext.tsx (NEW)
│   ├── auth.types.ts (NEW)
│   ├── App.tsx (UPDATED)
│   ├── main.tsx (UPDATED)
│   ├── store.ts (existing)
│   ├── types.ts (existing)
│   └── index.css (existing)
├── index.html
├── package.json
├── tsconfig.json
├── vite.config.ts
└── package-lock.json
```

---

## 🎬 How to Implement (Step by Step)

### 1️⃣ Copy New Files
```bash
# Create new files in src/
src/auth.types.ts (copy content)
src/AuthContext.tsx (copy content)
src/components/LoginPage.tsx (copy content)
src/components/SignupPage.tsx (copy content)
```

### 2️⃣ Replace Existing Files
```bash
# Replace these files with new versions
src/App.tsx (new version)
src/main.tsx (new version)
```

### 3️⃣ Update HomePage
```bash
# Option A: Replace src/components/HomePage.tsx with HomePageUpdated.tsx
# Option B: Manually add logout button to your existing HomePage.tsx
```

### 4️⃣ Verify Dependencies
```bash
# All dependencies already in package.json:
✓ react (19.2.6)
✓ react-dom (19.2.6)
✓ uuid (^14.0.0)
✓ lucide-react (^1.16.0)
✓ tailwindcss (4.1.17)
```

### 5️⃣ Run Dev Server
```bash
npm run dev
```

---

## 🧪 Test Immediately After Setup

### Test 1: Signup
- Go to `http://localhost:5173`
- See signup form
- Create account: `test@example.com` / `password123` / `Test User`
- Should redirect to login ✅

### Test 2: Login
- Click "Already have an account? Login"
- Use credentials from Test 1
- Should see HomePage ✅

### Test 3: Demo Account
- Click "Demo Account" button
- Auto-fills: `demo@cognoir.ai` / `demo123`
- Should login immediately ✅

### Test 4: Logout
- Click logout button (top right)
- Should go back to login page ✅

### Test 5: Persist Session
- Login successfully
- Refresh page (Cmd+R or F5)
- Should still be logged in ✅

---

## 💡 Pro Tips

### For Development:
```typescript
// In browser console, check users:
JSON.parse(localStorage.getItem('cognoir_users'))

// Check current user:
JSON.parse(localStorage.getItem('cognoir_auth_user'))

// Clear everything:
localStorage.clear()
```

### Quick Test Data:
```
Email: demo@cognoir.ai
Password: demo123

OR create your own:
Email: your@email.com
Password: anything (6+ chars)
Name: Your Name
```

### Reset Everything:
```javascript
// Run in browser console:
localStorage.clear()
location.reload()
```

---

## ⚙️ Customization Ideas

### 1. Change Colors
In `LoginPage.tsx` and `SignupPage.tsx`:
```jsx
// Change from gold (#D4AF61) to your color
className="text-[#YOUR_HEX_COLOR]"
```

### 2. Add Remember Me
In `LoginPage.tsx`:
```jsx
<input type="checkbox" id="remember" />
<label htmlFor="remember">Remember me</label>
```

### 3. Add Password Reset
Create `ForgotPasswordPage.tsx` with email form

### 4. Add Email Verification
Add `emailVerified: boolean` to User type

### 5. Add Social Login
Add Google/GitHub OAuth integration

---

## 🐛 Common Issues & Fixes

### Issue: "useAuth is not defined"
```
❌ Component not wrapped with AuthProvider
✅ Check main.tsx has AuthProvider wrapper
```

### Issue: Login page doesn't show
```
❌ App.tsx not updated
✅ Copy new App.tsx file
```

### Issue: Logout button missing
```
❌ HomePage.tsx not updated
✅ Add logout button code or use HomePageUpdated.tsx
```

### Issue: Styles look broken
```
❌ Tailwind CSS not working
✅ Verify index.css imports @import "tailwindcss"
✅ Run npm run build then npm run dev
```

### Issue: Can't create account
```
❌ Validation too strict
✅ Check password length (min 6 chars)
✅ Check email format
✅ Check for duplicate emails
```

---

## 📋 File Checklist Before Testing

- [ ] All 4 new files created in correct locations
- [ ] App.tsx replaced with new version
- [ ] main.tsx replaced with new version
- [ ] HomePage has logout button (or use HomePageUpdated.tsx)
- [ ] No import errors in IDE
- [ ] npm run dev runs without errors
- [ ] Dev server starts on localhost:5173

---

## 🎯 Success Criteria

After setup, you should be able to:

1. ✅ See login page on initial load
2. ✅ Signup with new email
3. ✅ Login with created account
4. ✅ See your notebooks on home page
5. ✅ See logout button
6. ✅ Logout and return to login
7. ✅ Refresh page and stay logged in
8. ✅ Demo account works
9. ✅ See error messages for invalid input
10. ✅ Create/edit/delete notebooks while logged in

---

## 📞 Need Help?

### Common Questions:

**Q: Where do I put the files?**
A: In your `src/` directory, following the structure above

**Q: Do I need to install packages?**
A: No, everything is in package.json already ✓

**Q: Is this secure for production?**
A: No, use real backend + password hashing. See IMPLEMENTATION_GUIDE.md

**Q: Can I change the design?**
A: Yes, modify className values in component files

**Q: How do I connect to a real database?**
A: Replace localStorage calls with API calls in AuthContext.tsx

---

## 🎉 You're All Set!

Once you follow these steps, your Cognoir AI Research Studio will have a complete, beautiful authentication system ready to use!

```
npm run dev
→ http://localhost:5173
→ See login page
→ Signup/Login
→ Access your notebooks
→ 🎊 Done!
```

Happy coding! 🚀
