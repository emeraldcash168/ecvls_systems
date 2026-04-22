# Full Stock Management Form - Tracking Update
**Status**: Implementation

**Approved Scope**: Focus ONLY on form page updates (VehicleForm/AddVehicleModal).

**Progress: Step 1 ✅** Hook updated (types, validation, handleChange for SenderId, ReceiverId, HandoverDate, Status, Remarks).

## Breakdown Steps

### 1. Update Form Hook (useVehicleFormUnified.ts)
- Add fields: senderId, receiverId, handoverDate, status ('PENDING'|'ASSIGNED'|'ACCEPTED'|'LOST'|'RETURNED'), remarks
- Validation: required for status/remarks on assign
- Integrate with existing formData

### 2. Update Core Form Component (VehicleFormUnified.tsx)
- New sections: Assignment Tracking (Sender/Receiver dropdowns from users, Status select, Date picker, Remarks textarea)
- User list from UserStaffService.getUsers()
- Responsive layout

### 3. Update AddVehicleModal.tsx
- Pass tracking data on submit
- Success: Close modal, refresh list

### 4. Test & Polish
- Local form validation/submit
- Image + remarks test
- Responsive mobile

### 5. Integration (Future)
- Backend: Extend VehicleService.assignVehicle()
- API: POST assignment payload

**Next Step**: Edit `src/lib/useVehicleFormUnified.ts` to add tracking fields.

Progress: 0/5

