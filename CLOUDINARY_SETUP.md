# Cloudinary Upload Fix

## Missing Config (Confirmed)
- CLOUDINARY_CLOUD_NAME: MISSING
- CLOUDINARY_API_KEY: MISSING  
- CLOUDINARY_API_SECRET: MISSING
- NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME: MISSING (client direct upload)
- NEXT_PUBLIC_CLOUDINARY_UPLOAD_PRESET: MISSING (client unsigned)

## Create .env.local
```
CLOUDINARY_CLOUD_NAME=your_cloud
CLOUDINARY_API_KEY=123456789012345
CLOUDINARY_API_SECRET=your_secret
NEXT_PUBLIC_CLOUDINARY_CLOUD_NAME=your_cloud
NEXT_PUBLIC_CLOUDINARY_UPLOAD_PRESET=ml_default  # Create UNSIGNED preset in dashboard
```

## Get Credentials
1. cloudinary.com/console → Dashboard → Account
2. Settings → Upload → Upload presets → Add unsigned preset

## Test
1. `npm run dev:reset`
2. curl http://localhost:3000/api/upload  
3. Browser: localhost:3000 → AddVehicleModal upload test

## Client Flow (AddVehicleModal.tsx)
Direct to Cloudinary (unsigned preset) - needs NEXT_PUBLIC_*

## Server Flow (/api/upload)
Server proxy (signed) - needs CLOUDINARY_*

Both now fixed once env set.

