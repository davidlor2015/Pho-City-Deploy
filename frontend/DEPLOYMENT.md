# SPA Routing Fix - Deployment Guide

This project now includes fallback configuration for client-side routing. The following files have been added to fix the "Page not found" issue when navigating to routes like `/about`, `/menu`, etc.

## Files Added:

- `public/_redirects` - For Netlify, Render, and similar platforms
- `public/vercel.json` - For Vercel
- `public/404.html` - Fallback for GitHub Pages and generic hosts
- Catch-all route in `App.tsx` - Safety fallback in React Router

## Deployment Instructions by Platform:

### **Netlify**

✅ Already configured via `_redirects` file

- Just deploy the `dist/` folder
- No additional configuration needed

### **Vercel**

✅ Already configured via `vercel.json` file

- Deploy the `dist/` folder or connect your repo
- No additional configuration needed

### **GitHub Pages**

✅ Configured via `404.html` file

- Deploy the `dist/` folder
- In Settings > Pages, set source to your branch
- Note: May have a slight delay on first load due to 404 -> redirect pattern

### **Render / Railway**

✅ Already configured via `_redirects` file

- Deploy as a static site
- Set publish directory to `dist`

### **Firebase Hosting**

Add to `firebase.json`:

```json
{
  "hosting": {
    "public": "dist",
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

### **Nginx**

Add to your nginx config:

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

### **Apache (.htaccess)**

Create `.htaccess` in the root:

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

## Testing

After deployment:

1. Navigate to your root URL (e.g., `https://yoursite.com`)
2. Click on navigation links to test routes
3. Try refreshing the page on `/about` or `/menu`
4. Try directly accessing `https://yoursite.com/about` in a new tab

All routes should now work correctly!
