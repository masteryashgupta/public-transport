# 🧹 Git History Cleanup Guide

## ⚠️ CRITICAL: Remove Exposed Secrets from GitHub History

Your MongoDB credentials and JWT secrets were committed to GitHub. They need to be removed from the git history to prevent unauthorized access.

---

## 🚨 Step-by-Step Git Cleanup

### Option 1: Using BFG Repo-Cleaner (Recommended - Faster)

```bash
# 1. Install BFG (one-time setup)
npm install -g bfg
# Or download from: https://rtyley.github.io/bfg-repo-cleaner/

# 2. Clone a fresh copy if you have a local one to backup
cd ..
git clone --mirror https://github.com/masteryashgupta/public-transport.git

# 3. Remove .env files from history
cd public-transport.git
bfg --delete-files .env
bfg --delete-files ".env*"

# 4. Clean up git metadata
git reflog expire --expire=now --all
git gc --aggressive --prune=now

# 5. Force push the cleaned history (WARNING: This rewrites history!)
cd ../public-transport
git push --force-with-lease origin main
```

### Option 2: Using git-filter-branch (Alternative)

```bash
# WARNING: This rewrites entire git history!
git filter-branch --tree-filter 'rm -f .env .env.* || true' --prune-empty -f HEAD

# Force push to remote
git push --force-with-lease origin main
```

### Option 3: Manual GitHub UI (Last Resort)

If you cannot use command line:
1. Go to GitHub repository settings
2. Under "Danger Zone", click "Delete this repository"
3. Create a new repository
4. Push the cleaned code (current state)
5. Update any links/references

---

## ✅ Verification After Cleanup

### 1. Check if secrets are still in history
```bash
# Search for MongoDB in all commits
git log -S "mongodb+srv://yashgupta271" --oneline

# Search for JWT secret in all commits
git log -S "17436e96b1b0ab8c76e9cfff3a8b6ae69d7597410489a1a1ef79f5ce2ce62d60" --oneline
```

**Expected**: No results (empty output means secrets are removed)

### 2. Verify .gitignore is correct
```bash
cat .gitignore | grep -E "\.env|secrets"
```

**Expected**: Should show .env, .env.*, etc.

### 3. List what will be committed
```bash
git status
```

**Should NOT show**: `.env` or `.env.production`

---

## 🔄 Additional Security Steps

### 1. Enable Branch Protection
- Go to Settings → Branches
- Add rule for "main" branch
- Enable "Require status checks to pass"
- Enable "Require code reviews before merging"
- Enable "Dismiss stale pull request approvals"

### 2. Enable Secret Scanning
```
Settings → Security & analysis → Secret scanning → Enable
```

GitHub will now scan for exposed secrets in future commits!

### 3. Set up a Pre-commit Hook
Create `.git/hooks/pre-commit`:

```bash
#!/bin/bash
# Prevent .env files from being committed

if git diff --cached --name-only | grep -E '\.env'; then
    echo "❌ ERROR: .env file detected in staging area!"
    echo "This file contains secrets and should not be committed."
    echo "Please remove it from staging and ensure it's in .gitignore"
    exit 1
fi

exit 0
```

Make it executable:
```bash
chmod +x .git/hooks/pre-commit
```

### 4. Enable Status Checks
Add to `.github/workflows/security.yml`:

```yaml
name: Security Checks
on: [push, pull_request]

jobs:
  secrets:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      
      - name: Scan for secrets
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: ${{ github.event.repository.default_branch }}
          head: HEAD
```

---

## 📋 Immediate Actions (Next 24 hours)

- [ ] Run git cleanup to remove secrets from history
- [ ] Generate new MongoDB credentials
- [ ] Generate new JWT secret
- [ ] Update `.env` with new credentials
- [ ] Restart backend server
- [ ] Enable GitHub secret scanning
- [ ] Enable branch protection
- [ ] Add pre-commit hook
- [ ] Test that everything works
- [ ] Verify no secrets in git history

---

## 🚨 Signs of Compromise to Monitor

Monitor for:
- Unexpected MongoDB database changes
- Multiple failed login attempts
- Unusual API usage patterns
- Strange location data
- Database connection from unknown IPs

---

## 📞 GitHub Support

If you need help with GitHub secret scanning:
1. Visit: [GitHub Secret Scanning Docs](https://docs.github.com/en/code-security/secret-scanning)
2. Contact: [GitHub Support](https://support.github.com)

---

**Important**: Complete these steps within 24 hours of discovering the exposure!

**Last Updated**: November 10, 2025
