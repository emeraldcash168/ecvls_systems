## OOAD Refactor Plan for LmsService

**Goal**: Make LmsService extend BaseService for consistency.

**Benefits**:
- Automatic caching (30s default)
- Metrics tracking
- Standardized ServiceResult
- SQL injection protection
- SSR-safe POJOs

**File Changes**:
1. `src/services/LmsService.ts` → extend BaseService
2. Define LmsEntity/LmsDB interfaces
3. Implement `toEntity()`, `applyFilters()`
4. CRUD calls `super.getAll()`, `super.getById()` etc.

**Timeline**: 15min implementation.

Approve to implement?

