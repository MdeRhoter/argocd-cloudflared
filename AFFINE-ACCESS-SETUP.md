# Affine Cloudflare Access Setup

## Overview
This setup protects `affine.mooir.uk` with Cloudflare Access authentication, requiring users to authenticate before accessing the Affine service.

## Architecture
```
Internet → Cloudflare Access (Auth) → Cloudflare Tunnel → https://affine.local.lan
```

## Security Features
- ✅ **Transport Encryption**: HTTPS end-to-end
- ✅ **Authentication**: Cloudflare Access (email OTP, social login, etc.)
- ✅ **MFA Support**: Through email OTP or social provider MFA
- ✅ **Session Management**: Configurable session timeouts
- ✅ **Access Logging**: Full audit trail in Cloudflare dashboard

## Configuration Steps

### 1. Cloudflare Zero Trust Setup
1. Go to Cloudflare dashboard → Zero Trust
2. Create team (free tier: up to 50 users)
3. Configure authentication methods

### 2. Access Application
- **Name**: Affine
- **URL**: `affine.mooir.uk`
- **Policy**: Allow with email/social authentication

### 3. Deploy Updated Configuration
```bash
# If using ArgoCD, commit and push changes
git add templates/deployment.yaml
git commit -m "Add Cloudflare Access protection for Affine"
git push

# Or apply directly with helm/kubectl
kubectl apply -f templates/deployment.yaml
```

## Authentication Methods Available

### Email OTP (Recommended for personal use)
- One-time codes sent to your email
- No additional setup required
- Works immediately

### Social Login + MFA
- Google, GitHub, Microsoft, etc.
- Enable MFA in your social provider
- More convenient for regular use

### Custom OIDC/SAML
- Available on paid plans
- Enterprise identity providers

## Testing Access
1. Visit `https://affine.mooir.uk`
2. Should redirect to Cloudflare Access login
3. Complete authentication
4. Get redirected to Affine application

## Session Management
- Default session: 24 hours
- Configurable in Access policies
- Can require re-auth for sensitive operations

## Troubleshooting
- Check Access logs in Cloudflare dashboard
- Verify DNS points to tunnel: `dig affine.mooir.uk`
- Test tunnel health: `cloudflared tunnel info hrn`
