# Vendor Onboarding Frontend - Vue 3 + TypeScript + Vite

This is the frontend application for the Vendor Onboarding portal built with Vue 3 using TypeScript and Vite.

## Prerequisites

- Node.js 16+ installed
- npm or yarn package manager

## How to Run

1. Navigate to the frontend directory:
   ```
   cd frontend
   ```

2. Install dependencies:
   ```
   npm install
   ```
   or with yarn:
   ```
   yarn install
   ```

3. Run the development server:
   ```
   npm run dev
   ```
   or with yarn:
   ```
   yarn dev
   ```

4. For production, build and preview:
   ```
   npm run build
   npm run preview
   ```
   or with yarn:
   ```
   yarn build
   yarn preview
   ```

## Changing the Backend

The application can work with either the Java or Node.js backend. To change which backend is used:

1. Open the file `src/services/VendorService.ts`
2. Locate the following line:
   ```typescript
   const useNodeBackend = true; // Set to true if using Node backend, false for Java backend
   ```
3. Change the value to:
   - `true` to use the Node.js backend (running on port 3000)
   - `false` to use the Java backend (running on port 3001)
4. Save the file and refresh your application

## Backend URLs

The backend URLs are configured in the `VendorService.ts` file:
- Node.js backend: http://localhost:3000/api
- Java backend: http://localhost:3001/api

Make sure the corresponding backend server is running before trying to use the frontend application.

## Running Tests

To run the tests:
```
npm run test
```

For watch mode:
```
npm run test:watch
```

For coverage report:
```
npm run test:coverage
```

### 1. Frontend UI Polish
-  Arrange the form and vendor list in a responsive layout that presents as a single column on mobile and a tidy multi-column layout on desktop using modern CSS (flexbox and/or grid).

   - Added flexbox and media query as breakpoints to show responsive layout

- Enhance the vendor list with hover/focus states, zebra striping, and an accessible empty state.

  - Added aria labels and roles along side css hover and focus to enhance list structure

- Introduce a lightweight design system by defining CSS variables (colours, spacing, typography) in `src/style.css` and apply them across components.
- Add a small visual flourish such as a light/dark theme toggle (or similar motif) handled with CSS-first techniques.


### 2. Delete vendor
- Implement a delete functionality to allow users to remove vendor entries from the system
- Include a confirmation dialog before deletion to prevent accidental removal.

      - Created a delete function on the frontend which accepts vendor id as param and deletes the vendor
      - Created a custom dialog box which on click of delete moves with the deletion whereas on cancel click closes the dialog. This prevents the accidental deletion.

### 3. Fix the UI bug
- Currently, clicking the "Add" button multiple times before the form resets can result in duplicate vendor entries.

      - On add vendor, loading is set to true. While submitting duplicate record if the loading is already true prevent multiple click of add button

### 4. Unique Emails
- Ensure that vendor emails are unique across the system. If a user tries to register a vendor with a duplicate email, they should be informed of the conflict. 

   - On Form submit, check-email api is called which validates whether the entered email already exists. If so further addition of the form data is prevented with an error message.If the email doesn't exists, vendor data is added and shown on the list. The api accepts email as encoded query param which is decoded on server side and run through db for existence.