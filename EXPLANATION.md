# Google Drive Photo Picker Gallery — Code Explanation

This HTML file implements a photo gallery web app that allows users to log in with their Google account, pick a folder from their Google Drive, and display all images from that folder in a card-based gallery layout. The interface is in Thai, but the logic is straightforward and suitable for internationalization.

## Key Features

- **Google OAuth Login:**  
  Users authenticate via Google Sign-In to grant access to their Drive photos.

- **Google Picker Integration:**  
  Users can browse and select a folder from their Google Drive using the Google Picker UI.

- **List and Display Images:**  
  Once a folder is selected, the app fetches image files inside that folder and displays them as cards (with image preview and file name).

- **Logout Support**  
  Users can easily sign out, which clears gallery and UI state.

- **Responsive Layout**  
  Uses CSS grid and styling for a modern, clean, mobile-friendly look.

---

## Main Components

### 1. **Config Section**

```js
const CLIENT_ID = '...';
const API_KEY = '...';
const SCOPES = 'https://www.googleapis.com/auth/drive.readonly';
```
Replace these with your Google API credentials.

### 2. **Google API & Picker Loading**

- Loads Google API scripts (`gapi` for Drive, plus Picker).
- Waits for them to be ready before enabling login.

### 3. **Authentication & Session**

- Handles login via Google OAuth (`google.accounts.oauth2.initTokenClient`).
- Stores the OAuth `accessToken` for Drive API and Picker.
- Handles logout (clears tokens, resets UI).

### 4. **Folder Selection with Picker**

- Uses Google Picker to allow the user to select a Drive folder.
- When a folder is picked, gets its ID, and passes it to the image listing function.

### 5. **List Images in the Folder**

- Calls Drive API:  
  `gapi.client.drive.files.list` with a query for image files within the picked folder.
- For each image file, gets a thumbnail or content link, builds a card with the image and its name.

### 6. **Notifications**

- Shows user feedback for login, logout, errors, etc., as floating notifications.

---

## Notable Code Snippets

**Google Picker Setup:**
```js
let view = new google.picker.DocsView(google.picker.ViewId.FOLDERS)
  .setMimeTypes('application/vnd.google-apps.folder')
  .setSelectFolderEnabled(true);
let picker = new google.picker.PickerBuilder()
  .addView(view)
  .setOAuthToken(accessToken)
  .setDeveloperKey(API_KEY)
  .setTitle("เลือกโฟลเดอร์ภาพจาก Google Drive")
  .setCallback(pickerCallback)
  .build();
picker.setVisible(true);
```

**List Image Files:**
```js
let res = await gapi.client.drive.files.list({
  q: `'${folderId}' in parents and mimeType contains 'image/' and trashed = false`,
  fields: "files(id,name,thumbnailLink,webContentLink)",
  pageSize: 100,
  supportsAllDrives: true,
  includeItemsFromAllDrives: true,
});
```

---

## Customization and Deployment

- To use this app, you must set up your own Google OAuth credentials and API keys (Google Cloud Console).
- The project is client-side only (no backend required).
- You may internationalize by changing the Thai UI text to your language of choice.

---

**Security Note:**  
Never expose sensitive API keys in public repositories or production code without proper security measures.