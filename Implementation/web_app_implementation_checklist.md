# SmartNet Web App Implementation Checklist

This checklist captures the recommended implementation order for building the web application portion of the SmartNet project.

## 1. Foundation
- [ ] Create the React/Vite web app structure for the dashboard
- [ ] Set up Firebase project configuration for Auth, Firestore, Cloud Functions, and Hosting
- [ ] Add environment variables for Firebase config and API endpoints
- [ ] Define the app folder structure for auth, nets, recovery, map, services, and shared UI

## 2. Data and backend contract
- [ ] Implement the Firestore data model for users, organizations, nets, telemetry, gateways, and audit events
- [ ] Create Firestore security rules for organization-scoped access
- [ ] Implement Cloud Functions for telemetry ingestion, recovery actions, and alert acknowledgment
- [ ] Confirm the data contract for latest state, history, timestamps, and error handling

## 3. Authentication and app shell
- [ ] Build the sign-in flow using Firebase Auth
- [ ] Add protected routes and organization context
- [ ] Create the main layout with navigation for Overview, Net Detail, Devices, and Settings
- [ ] Add loading, empty, and error states for all major screens

## 4. Core dashboard features
- [ ] Build the Overview page with a net list, search/filter/sort controls, and map markers
- [ ] Build the Net Detail page with latest status, battery and signal information, telemetry history, and stale-data indicators
- [ ] Implement the recovery action with a confirmation flow and success-based optimistic UI

## 5. Map and real-time behavior
- [ ] Integrate a map view using Leaflet/OpenStreetMap
- [ ] Connect Firestore listeners for live net updates
- [ ] Display stale data visually based on the last received time
- [ ] Ensure the app behaves correctly when offline or when data is cached

## 6. UX and accessibility
- [ ] Make the layout responsive for mobile and desktop
- [ ] Add keyboard navigation and accessible labels
- [ ] Ensure status is not communicated by color alone
- [ ] Provide a map/list alternative for users who cannot use the map comfortably

## 7. Quality and deployment
- [ ] Add unit and integration tests for auth, data services, and recovery flows
- [ ] Add end-to-end tests for the main user journeys
- [ ] Review the implementation against the acceptance scenarios in the requirements documentation
- [ ] Deploy the dashboard and validate the production setup

## Source documents
- [docs/01_System_Architecture.md](../docs/01_System_Architecture.md)
- [docs/02_Requirements.md](../docs/02_Requirements.md)
- [docs/05_Database_Design.md](../docs/05_Database_Design.md)
- [docs/06_API_Design.md](../docs/06_API_Design.md)
- [dashboard/ARCHITECTURE.md](../dashboard/ARCHITECTURE.md)
