# 🔐 Security & Secret Management Guide

## ⚠️ Important Security Notice

This project contains sensitive information that should **NEVER** be committed to version control or shared publicly.

---

## 🚨 Secrets That Must Be Protected

### ❌ DO NOT Commit:
- `.env` files with real credentials
- API keys (Google Maps, MongoDB, JWT secrets)
- Database connection strings with passwords
- Private authentication tokens
- Personal IP addresses
- Real test account credentials

### ✅ DO Commit:
- `.env.example` (template with placeholder values)
- `.gitignore` (configured to exclude .env files)
- `SECURITY.md` (this file)
- Documentation about setup process

---

## 📋 Pre-Deployment Checklist

Before deploying to production:

- [ ] **Generate new JWT secret**: `node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"`
- [ ] **Create new MongoDB user** with strong password
- [ ] **Change NODE_ENV** to `production`
- [ ] **Set CORS_ORIGINS** to only your frontend domain
- [ ] **Enable MongoDB IP whitelist** for your server only
- [ ] **Use HTTPS** for all connections
- [ ] **Rotate secrets regularly** (especially JWT_SECRET)
- [ ] **Enable MongoDB backups**
- [ ] **Set up monitoring and logging**
- [ ] **Use environment variables** for all sensitive data
- [ ] **Never commit credentials** to any branch

---

## 🔑 How to Generate Secrets

### JWT Secret
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### MongoDB Connection String
1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Create database user with strong password
4. Click "Connect" → "Connect your application"
5. Copy connection string
6. Replace username and password with your credentials
7. Format: `mongodb+srv://username:password@cluster.mongodb.net/database-name?retryWrites=true&w=majority`

---

## 🛡️ Environment-Specific Configuration

### Development (.env)
```
NODE_ENV=development
CORS_ORIGINS=http://localhost:3000,http://localhost:19006
```

### Staging (.env.staging)
```
NODE_ENV=staging
CORS_ORIGINS=https://staging.yourdomain.com
```

### Production (.env.production)
```
NODE_ENV=production
CORS_ORIGINS=https://yourdomain.com
```

---

## 🔄 Secret Rotation

When secrets are compromised:

1. **Immediately** rotate the secret in your database/service
2. **Update** the `.env` file with new secret
3. **Redeploy** the application
4. **Monitor** logs for any unauthorized access attempts
5. **Review** audit logs for suspicious activity

---

## 📝 If You Accidentally Committed Secrets

If secrets were committed to GitHub:

1. **Immediately rotate** the compromised credentials
2. **Remove from git history** using BFG Repo-Cleaner:
   ```bash
   bfg --delete-files .env
   git reflog expire --expire=now --all
   git gc --aggressive --prune=now
   git push --force
   ```
3. **Force push** to remove from history
4. **Notify** GitHub about the exposure
5. **Monitor** for any unauthorized access

---

## 🔐 Best Practices

### ✅ DO:
- Use strong, randomly generated secrets (32+ characters)
- Store secrets in environment variables only
- Use different secrets for each environment
- Rotate secrets regularly (quarterly minimum)
- Use `.env.example` as documentation template
- Implement secret scanning in CI/CD pipeline
- Use secrets management tools (AWS Secrets Manager, HashiCorp Vault, etc.)
- Enable two-factor authentication (2FA) on MongoDB Atlas
- Use IP whitelist for database access
- Monitor all access logs regularly

### ❌ DON'T:
- Hardcode secrets in source code
- Commit `.env` files to git
- Use default or example secrets in production
- Share secrets via email or chat
- Log sensitive information
- Reuse the same secret across environments
- Use weak or predictable secrets
- Commit API keys or credentials
- Leave `.env.production` in repository
- Disable HTTPS or security measures

---

## 🔍 Monitoring & Auditing

### Set up alerts for:
- Multiple failed login attempts
- Unusual database access patterns
- API rate limit abuse
- Unauthorized location access
- Unexpected token usage

### Regular audits:
- Review MongoDB access logs weekly
- Check API usage patterns monthly
- Audit user permissions quarterly
- Rotate secrets every 90 days

---

## 📚 Additional Resources

- [OWASP Secret Management](https://owasp.org/www-community/Sensitive_Data_Exposure)
- [MongoDB Security Checklist](https://docs.mongodb.com/manual/administration/security-checklist/)
- [JWT Best Practices](https://tools.ietf.org/html/rfc8725)
- [Securing Node.js Applications](https://nodejs.org/en/docs/guides/security/)

---

## 📞 Security Issues

If you discover a security vulnerability:

1. **DO NOT** open a public GitHub issue
2. **Email** the maintainers privately
3. **Allow time** for them to prepare a fix
4. **Avoid** discussing publicly until patch is released

---

**Last Updated**: November 10, 2025  
**Version**: 1.0
