# 🧩 System Architecture — MemoryVault for WhatsApp

## 🏗️ Overview

MemoryVault is a cloud-based web application that integrates with the **WhatsApp Cloud API** to collect, categorize, and store messages and media securely.  
It uses a **modular architecture**, separating the frontend, backend, and database layers to ensure scalability, maintainability, and performance.

---

## 🎨 Frontend

- **Framework:** React + TypeScript
- **Styling:** Tailwind CSS
- **Routing:** React Router
- **State Management:** React Context API
- **API Communication:** Axios (for RESTful API calls)
- **Real-Time Updates:** Convex real-time sync
- **UI Design:** Built using Figma wireframes and responsive layouts

### Responsibilities

- User authentication & connection with WhatsApp
- Display saved messages, media, and folders
- Manage search, filters, and tags
- Handle export actions (triggering data download from backend)
- Real-time updates of vault content

---

## ⚙️ Backend

- **Environment:** Node.js
- **Framework:** Express.js
- **Integration:** WhatsApp Cloud API for message retrieval
- **Data Storage:** Convex DB (NoSQL, real-time)
- **Authentication:** JWT (JSON Web Token)
- **Deployment:** Render / Vercel (for APIs)

### Core API Endpoints

| Endpoint      | Method | Description                                                                                    |
| ------------- | ------ | ---------------------------------------------------------------------------------------------- |
| `/api/save`   | POST   | Save WhatsApp message or media to vault                                                        |
| `/api/fetch`  | GET    | Retrieve saved messages/media for a user                                                       |
| `/api/search` | GET    | Filter content by tags, type, or keyword                                                       |
| `/api/export` | GET    | **Export the entire vault** (messages, media, and metadata) as a downloadable ZIP or JSON file |
| `/api/delete` | DELETE | Remove specific content from the user vault                                                    |

---

## 🗃️ Database (Convex DB)

- **Type:** NoSQL (real-time)
- **Collections:**
  - `users`: { id, name, whatsappId, createdAt }
  - `vaultItems`: { userId, type, content, mediaURL, tags, createdAt }
  - `folders`: { userId, name, items[] }
  - `exports`: { userId, exportType, fileURL, createdAt }

### Features

- Real-time data sync across devices
- Secure, isolated data per user
- Scalable for large message and media volumes

---

## 📤 Export Feature Workflow

When a user triggers **Export Vault**:

1. Frontend sends a request to `/api/export`.
2. Backend authenticates the user using JWT.
3. Backend retrieves all user data (messages, media, folders) from Convex DB.
4. Data is bundled using a Node.js library (e.g., `archiver` for ZIP or JSON conversion).
5. A secure, one-time download link is generated.
6. The user receives a downloadable file (ZIP or JSON).

