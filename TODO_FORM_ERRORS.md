# FORM ERROR DEBUGGING PLAN
## Status: ACTIVE

### Phase 1: Fix Build Blockers (IN PROGRESS)
- [x] Create this TODO  
- [ ] Fix src/lib/lms-cache.ts syntax (\n corruption)
- [ ] Restart dev server  
- [ ] Verify /lms loads without 500

### Phase 2: Deploy Error Capture (Next)
```
1. Enhance src/app/components/ErrorBoundary.tsx 
   - Add form context capture
   - Form field state snapshot
   - Replay button for errors

2. Create src/lib/formDebugger.ts
   - Intercept console.error/warn for forms  
   - Form validation overlay toggle
   - Network request error capture

3. Wrap Priority Forms:
   - src/app/components/dashboard/FiltersBar.tsx
   - src/app/(app)/settings/SettingsContent.tsx 
   - src/app/components/lms/*Form.tsx files
   - src/app/components/vehicles/*Form.tsx
```

### Phase 3: Test Forms (After Phase 2)
```
[X] Vehicles (useVehicleForm hook)
[X] LMS Lessons (YouTube validation)  
[X] Settings Users (password mismatch)
[ ] Dashboard Filters  
[ ] Any new forms user opens
```

### Known Error Patterns:
```
Vehicle Forms:
- Brand/Model/Category required
- Year: 1900-current year  
- PriceNew: positive number
- Plate: 1A-1234 format
- Image: 5MB max, image/* type

LMS Forms:
- CategoryId !== 0
- Valid YouTube URL regex  
- Title required

Settings:
- Password mismatch
- Username exists
```

### Commands to Run After Each Phase:
```
npm run dev  # After syntax fix
```

To test form errors:
1. Run ?debugForms=true
2. Try LMS AddLessonForm (invalid YouTube URL)
3. Vehicle form missing required fields
4. Settings create user w/ password mismatch
5. Check red button top-right for errors
