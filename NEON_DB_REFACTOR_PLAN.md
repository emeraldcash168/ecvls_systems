# Neon DB-Style Data Flow Refactoring Plan

## Current Architecture Issues
1. **Too many abstraction layers**: Client → useVehicles → vehicleApi → API Route → VehicleService → db-singleton → Neon
2. **Complex client-side caching**: localStorage-based cache with stale time logic
3. **Synchronous blocking**: Large dataset queries block the main thread
4. **No streaming support**: All data must be loaded before response

## Target Architecture (Neon-Style Serverless)

### 1. Edge-First API Routes
- Use Next.js Edge runtime for instant cold starts
- Direct SQL template literals (Neon style): `sql\`SELECT * FROM vehicles\``
- Streaming JSON responses for large datasets

### 2. Simplified Data Flow
```
Client → useVehicles (SWR) → /api/vehicles (Edge) → sql\`\` → Neon
```

### 3. Server-Side Caching
- Use Next.js `unstable_cache` for ISR-style caching
- Redis for distributed caching (optional)
- Remove complex localStorage caching

### 4. Streaming Support
- NDJSON streaming for large vehicle lists
- Progressive loading with cursor-based pagination

## Implementation Steps

### Phase 1: Create Edge-Compatible API Route
- [ ] Create `/app/api/vehicles/edge/route.ts`
- [ ] Use direct `sql\`\`` template literals
- [ ] Implement cursor-based pagination
- [ ] Add streaming response support

### Phase 2: Simplify Client Hook
- [ ] Refactor `useVehicles` to use SWR or React Query
- [ ] Remove localStorage caching complexity
- [ ] Add optimistic updates
- [ ] Support streaming data

### Phase 3: Update Service Layer
- [ ] Make VehicleService edge-compatible
- [ ] Add streaming query support
- [ ] Implement connection pooling optimizations

### Phase 4: Update UI Components
- [ ] Add streaming UI indicators
- [ ] Implement virtual scrolling for large lists
- [ ] Progressive data loading

## Key Changes

### API Route (Neon Style)
```typescript
// Edge runtime for instant cold starts
export const runtime = 'edge';

// Direct SQL like Neon documentation
const vehicles = await sql`SELECT * FROM vehicles LIMIT ${limit} OFFSET ${offset}`;
```

### Client Hook (SWR Style)
```typescript
// Simple, clean data fetching
const { data, error, isLoading } = useSWR('/api/vehicles', fetcher, {
  refreshInterval: 30000, // 30s refresh
  revalidateOnFocus: true,
});
```

### Benefits
1. **Faster cold starts**: Edge runtime eliminates Node.js boot time
2. **Simpler code**: Remove 3 abstraction layers
3. **Better caching**: Next.js built-in caching vs custom localStorage
4. **Streaming**: Progressive data loading for better UX
5. **Serverless-native**: Designed for Vercel/Neon serverless architecture
