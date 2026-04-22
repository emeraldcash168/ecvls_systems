# Vehicle Count Fix TODO

## Problem
- Dashboard shows "Showing 10 of 1000 vehicles" instead of "Showing 10 of 1218"
- Cars count shows incorrect value instead of 1041

## Root Cause
Stale cache in `VehicleService.getVehicleStats()` with cache key `"vehicle:stats:v5"` and 5-minute TTL

## Fix Plan - ALL COMPLETED ✅

### 1. Update VehicleService.ts ✅ COMPLETED
- [x] Change cache key from `"vehicle:stats:v5"` to `"vehicle:stats:v6"` to bust stale cache
- [x] Reduce cache TTL from 5 minutes to 30 seconds for more frequent updates

### 2. Update /api/vehicles/stats/route.ts ✅ COMPLETED
- [x] Add `no-store` cache control header to prevent caching
- [x] Add debug logging to verify correct counts

### 3. Update /api/dashboard/stats/route.ts ✅ COMPLETED
- [x] Force refresh by passing `true` to `getVehicleStats()`
- [x] Add no-cache headers to prevent response caching

### 4. Update page.tsx - NOT NEEDED ✅
- [x] Verified correct data mapping from stats to dashboardMeta is already in place

## Files Modified
1. `src/services/VehicleService.ts` - Updated cache key to v6, reduced TTL to 30s
2. `src/app/api/vehicles/stats/route.ts` - Added no-cache headers
3. `src/app/api/dashboard/stats/route.ts` - Force refresh, no-cache headers

## Summary of Changes

### VehicleService.ts
- Cache key changed: `"vehicle:stats:v5"` → `"vehicle:stats:v6"` (busts stale cache)
- Cache TTL reduced: 5 minutes → 30 seconds (fresher data)

### /api/vehicles/stats/route.ts
- Cache headers changed to: `no-store, no-cache, must-revalidate, proxy-revalidate`

### /api/dashboard/stats/route.ts
- Force refresh enabled: `getVehicleStats(true)` instead of `getVehicleStats(false)`
- Cache headers changed to: `no-store, no-cache, must-revalidate, proxy-revalidate`

## Expected Result
- Dashboard should now show "Showing 10 of 1218 vehicles" (actual total from DB)
- Cars count should show 1041 (actual count from DB)
- Stats will refresh every 30 seconds max (or on every request for dashboard)

## Additional Fix - TukTuk Icon ✅ COMPLETED
- Fixed TukTuk category card showing truck icon instead of proper tuk-tuk icon
- Created custom `TukTukIcon` SVG component in `EnhancedDashboard.tsx`
- Replaced `Truck` icon with `TukTukIcon` in both VEHICLE_CATEGORIES config and Tuk Tuks Card

### Files Modified for Icon Fix
1. `src/app/components/dashboard/EnhancedDashboard.tsx` - Added TukTukIcon component and replaced Truck icon
