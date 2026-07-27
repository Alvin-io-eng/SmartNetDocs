# SmartNet Web App Implementation Checklist

This checklist is written so that another developer or LLM can follow it directly without needing extra context. It is specific to the SmartNet project and assumes the web app is a Firebase-backed React dashboard for viewing nets, telemetry, and recovery actions.

## Project context

The SmartNet dashboard is the web client that receives telemetry data from the Boat Gateway through Firebase. The app should allow authenticated users to view nets on a map and in a list, inspect telemetry history, and perform recovery actions for nets. The web app must be scoped by organization, must display stale-data status clearly, and must avoid exposing sensitive information or using gateway credentials.

## 1. Foundation
- [ ] Create the React/Vite web app structure for the dashboard
  - Create a modern React web app using Vite and organize it around SmartNet’s domain features rather than generic boilerplate.
  - Set up the dashboard as a multi-page app with routes for authentication, overview, net detail, devices, and settings.
  - Use a folder structure such as src/app, src/features/auth, src/features/nets, src/features/recovery, src/components, src/map, and src/services/firebase.
  - Keep the UI focused on the MVP: map/list views, status cards, telemetry history, and recovery controls.
- [ ] Set up Firebase project configuration for Auth, Firestore, Cloud Functions, and Hosting
  - Create or connect the web app to the same Firebase project used by the backend and gateway integration.
  - Enable Firebase Authentication, Firestore, Cloud Functions, and Hosting.
  - Make sure the dashboard uses the Firebase web SDK, not backend credentials or firmware secrets.
  - Prepare the project so the dashboard can later connect to callable functions for recovery and alert actions.
- [ ] Add environment variables for Firebase config and API endpoints
  - Use environment variables for Firebase API key, auth domain, project id, storage bucket, messaging sender id, app id, and any Cloud Function base URL.
  - Keep secrets out of the client source code and use a documented .env pattern for local development.
  - Make sure the app can run in development and production environments without hardcoded configuration.
- [ ] Define the app folder structure for auth, nets, recovery, map, services, and shared UI
  - Create clear modules for authentication, net-related data, recovery actions, map rendering, and Firebase service wrappers.
  - Keep reusable components in a shared UI layer so pages stay simple and consistent.
  - Prepare the structure so future features like device diagnostics and settings can be added without major refactoring.

## 2. Data and backend contract
- [ ] Implement the Firestore data model for users, organizations, nets, telemetry, gateways, and audit events
  - Build support for the collections and paths defined by the database design, including users/{uid}, organizations/{orgId}, organizations/{orgId}/members/{uid}, organizations/{orgId}/nets/{netId}, and nested telemetry events.
  - Ensure the app can read the latest net state and the telemetry history for each net.
  - Model the latest state using the same fields as the database spec, including status, latest, lastReceivedAt, and updatedAt.
  - Represent telemetry timestamps and location values in a way that matches Firestore expectations and the dashboard UI requirements.
- [ ] Create Firestore security rules for organization-scoped access
  - Write rules so that a user can only access data belonging to their organization.
  - Make sure members can read data for their organization, but cannot read data from unrelated organizations.
  - Restrict writes to trusted server-side paths and ensure only permitted users can change recovery status or other sensitive state.
  - Keep rules aligned with the project’s security model and tenant-isolation requirement.
- [ ] Implement Cloud Functions for telemetry ingestion, recovery actions, and alert acknowledgment
  - Build a Cloud Function for gateway ingestion that accepts telemetry batches and writes them safely to Firestore.
  - Build a callable function for recovery so the dashboard can mark a net as recovered with server-side validation.
  - Build a callable function for alert acknowledgment so the UI can confirm alerts without directly writing to Firestore from the client.
  - Ensure each server-side action returns a clear response, audit id, or error code.
- [ ] Confirm the data contract for latest state, history, timestamps, and error handling
  - Define exactly what data the dashboard expects from Firestore for each screen.
  - Make sure the app handles server timestamps, stale-data calculations, and optional fields consistently.
  - Support predictable error responses from the backend so the dashboard can show friendly messages and retry behavior.
  - Keep the contract aligned with the API design document so frontend and backend stay consistent.

## 3. Authentication and app shell
- [ ] Build the sign-in flow using Firebase Auth
  - Implement sign-in with the authentication method chosen for the MVP, such as email/password or provider-based sign-in.
  - Show clear loading and error states during authentication.
  - Store user identity in the app state and use Firebase Auth as the entry point for all protected actions.
  - Redirect unauthenticated users to the sign-in page and prevent access to protected routes.
- [ ] Add protected routes and organization context
  - Create route guards so only signed-in users can access dashboard pages.
  - Load the user’s active organization and pass it into the app context so all queries and UI actions are scoped correctly.
  - Ensure the dashboard can handle a user with no active organization or an invalid organization context.
  - Keep organization switching logic explicit and easy to reason about.
- [ ] Create the main layout with navigation for Overview, Net Detail, Devices, and Settings
  - Build a shell layout with navigation for the main dashboard pages defined in the architecture.
  - Make sure the layout works well on desktop and mobile widths.
  - Add page-level loading and empty states so users always understand what is happening.
  - Keep navigation consistent with the app’s role-based access and the MVP feature set.
- [ ] Add loading, empty, and error states for all major screens
  - Implement skeleton loaders or placeholders while Firestore data is loading.
  - Show empty states when there are no nets, no telemetry history, or no devices to display.
  - Render friendly error states when data fetches fail or when permission issues occur.
  - Make these states visible and understandable without relying on color alone.

## 4. Core dashboard features
- [ ] Build the Overview page with a net list, search/filter/sort controls, and map markers
  - Create a page that shows all nets available to the current organization.
  - Add a searchable and sortable list of nets, with filtering by status or freshness where appropriate.
  - Render map markers for each net and make each marker clickable so it can open the corresponding detail view.
  - Use the latest state from Firestore so the overview reflects the most recent known information.
- [ ] Build the Net Detail page with latest status, battery and signal information, telemetry history, and stale-data indicators
  - Create a detail screen that shows the selected net’s name, current status, last known location, and last received time.
  - Display battery and signal details using the values available in the telemetry payloads.
  - Show a paginated telemetry history list and optionally a map of the latest fix.
  - Clearly label the data as stale when it exceeds the freshness threshold defined by the system requirements.
- [ ] Implement the recovery action with a confirmation flow and success-based optimistic UI
  - Add a recovery button on the net detail page that opens a confirmation modal.
  - Call the recovery Cloud Function only after the user confirms the action.
  - Update the UI after a successful response and show a clear success or failure result.
  - Avoid making recovery changes directly in the client without server-side validation.

## 5. Map and real-time behavior
- [ ] Integrate a map view using Leaflet/OpenStreetMap
  - Add a map library and render net locations with markers and tooltips or popups.
  - Make the map responsive and usable in both list-and-map and map-first layouts.
  - Keep the map view aligned with the project requirement that it should not be the only way to understand net status.
  - Use the same data source as the list view so the UI stays consistent.
- [ ] Connect Firestore listeners for live net updates
  - Subscribe to the relevant Firestore collections so the dashboard updates automatically when telemetry arrives.
  - Use listeners for current net state and update the UI in near real time.
  - Be careful with query scope so you do not accidentally read too much data from the free-tier setup.
  - Ensure listeners are cleaned up correctly when users navigate away.
- [ ] Display stale data visually based on the last received time
  - Compute freshness from lastReceivedAt or the latest telemetry timestamp.
  - Add visible labels or badges such as “stale” or “fresh” so users can quickly understand the state.
  - Make the stale indicator independent of browser cache state and based on server-provided timestamps.
  - Keep the logic consistent with the acceptance criteria for location freshness.
- [ ] Ensure the app behaves correctly when offline or when data is cached
  - Let Firestore persistence cache data locally where appropriate and show the user when content is cached or stale.
  - Make sure the app still renders read-only views in an offline or degraded network state.
  - Be transparent about what is live data versus cached data.
  - Avoid showing the app as fully current when the data is only from cache.

## 6. UX and accessibility
- [ ] Make the layout responsive for mobile and desktop
  - Design the dashboard so map and list content stack correctly on narrower screens.
  - Keep key actions reachable on smaller viewports and avoid horizontal overflow.
  - Ensure the net detail view and recovery flow remain usable at both compact and wide widths.
- [ ] Add keyboard navigation and accessible labels
  - Make all interactive controls reachable using the keyboard.
  - Add descriptive labels to buttons, links, form inputs, and map controls.
  - Ensure screen readers can understand the page structure and the purpose of each control.
  - Support focus states so navigation is clear and usable.
- [ ] Ensure status is not communicated by color alone
  - Use text labels, icons, and badges along with color to describe status such as healthy, stale, low battery, or recovered.
  - Make sure users can identify state even if color vision is limited.
  - Keep the UI understandable when viewed in grayscale or low-contrast conditions.
- [ ] Provide a map/list alternative for users who cannot use the map comfortably
  - Make sure the app can provide the same core information through a list view when a user cannot interact well with the map.
  - Keep navigation and data access equivalent between these two presentation modes.
  - Avoid making the map the only source of truth for net location or recovery information.

## 7. Quality and deployment
- [ ] Add unit and integration tests for auth, data services, and recovery flows
  - Write tests for authentication guards, Firestore service wrappers, and recovery action flow.
  - Cover success and failure scenarios so the dashboard behaves predictably under common errors.
  - Keep tests focused on real behavior, not just mocked UI state.
- [ ] Add end-to-end tests for the main user journeys
  - Create end-to-end tests for sign-in, net browsing, net detail review, and recovery action.
  - Ensure the main acceptance scenarios are covered, especially location visibility, stale-data display, and recovery status changes.
  - Make the tests reflect the real user experience rather than internal implementation details.
- [ ] Review the implementation against the acceptance scenarios in the requirements documentation
  - Verify the app meets the requirements for map visibility, recovery flow, freshness, and tenant-scoped access.
  - Confirm that the dashboard behaves correctly under the expected operational conditions.
  - Trace the implementation back to the product requirements rather than just finishing the UI.
- [ ] Deploy the dashboard and validate the production setup
  - Deploy the frontend to the chosen hosting provider and confirm it works against the production Firebase configuration.
  - Verify environment variables, auth flows, and Firestore permissions in the production environment.
  - Test a real end-to-end scenario after deployment so the dashboard is usable by an operator.

## Source documents
- [docs/01_System_Architecture.md](../docs/01_System_Architecture.md)
- [docs/02_Requirements.md](../docs/02_Requirements.md)
- [docs/05_Database_Design.md](../docs/05_Database_Design.md)
- [docs/06_API_Design.md](../docs/06_API_Design.md)
- [dashboard/ARCHITECTURE.md](../dashboard/ARCHITECTURE.md)
