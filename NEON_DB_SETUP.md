# Neon DB Connection Setup Guide

## Your Neon Project
- **Project URL**: https://console.neon.tech/app/projects/long-hill-90158403/branches/br-lingering-cell-ai19xt06/tables
- **Branch**: br-lingering-cell-ai19xt06

## Step 1: Get Your Connection String

1. Go to your Neon console: https://console.neon.tech/app/projects/long-hill-90158403/branches/br-lingering-cell-ai19xt06/tables
2. Click the **"Connect"** button (top right of the page)
3. Select **"Database"** from the dropdown menu
4. Copy the connection string. It will look like:
   ```
   postgresql://username:password@ep-xxxxx.us-east-1.aws.neon.tech/dbname?sslmode=require
   ```

## Step 2: Configure Environment Variables

### Option A: Local Development (.env.local)

Create a file named `.env.local` in your project root:

```env
DATABASE_URL=postgresql://your-username:your-password@ep-xxxxx.us-east-1.aws.neon.tech/your-db?sslmode=require
```

### Option B: Vercel Deployment

1. Go to [Vercel Dashboard](https://vercel.com/dashboard)
2. Select your project
3. Go to **Settings** → **Environment Variables**
4. Add a new variable:
   - **Name**: `DATABASE_URL`
   - **Value**: Your connection string from Step 1
5. Click **Save**
6. Redeploy your application

## Step 3: Verify Connection

### Test API Endpoint
Visit: `http://localhost:3000/api/debug/db-counts`

You should see a JSON response with table counts:
```json
{
  "success": true,
  "data": {
    "tables": [
      { "table": "vehicles", "count": 123 },
      { "table": "users", "count": 5 },
      ...
    ]
  }
}
```

### Data Explorer Page
Visit: `http://localhost:3000/debug/data-list`

You should see all your database tables with data displayed in a tabbed interface.

## Troubleshooting

### Connection Errors

If you see "DATABASE_URL not configured":
- Make sure `.env.local` exists in your project root
- Restart your Next.js dev server after creating the file

If you see "Connection refused" or "Timeout":
- Check that your connection string is correct
- Verify your Neon project is active (not suspended)
- Check if your IP is allowed (Neon → Project Settings → IP Allow)

### SSL Mode Issues

If you get SSL-related errors, ensure your connection string includes:
```
?sslmode=require
```

## Tables in Your Database

Based on your Neon console, the following tables should be available:
- `vehicles` - Vehicle inventory data
- `users` - User accounts and authentication
- `lms_categories` - Learning management categories
- `lms_lessons` - Training lessons content
- `lms_staff` - Staff training records
- `lms_lesson_completions` - Lesson completion tracking

## Security Notes

⚠️ **Never commit your `.env.local` file to git!** It's already in `.gitignore`.

⚠️ **Use Connection Pooling in Production**: For serverless deployments, consider using Neon's pooled connection string (add `-pooler` to the hostname).

## Support

If you continue to have issues:
1. Check Neon's status page: https://neon.tech/status
2. Review Neon docs: https://neon.tech/docs
3. Test your connection string using: `psql "your-connection-string"`
