# ERS - Event Rental System

A modern event rental platform built with Next.js 15 and Firebase, allowing users to rent and share party supplies and event equipment.

## 🚀 Features

- **User Authentication**: Email/password and Google sign-in
- **Item Management**: List, edit, and manage rental items
- **Search & Filter**: Find items by category, location, and price
- **Booking System**: Request and manage rentals
- **Reviews & Ratings**: Build trust through community feedback
- **Secure Payments**: Integration with payment providers
- **Real-time Updates**: Instant notifications for bookings

## 🛠️ Tech Stack

- **Frontend**: Next.js 15, React 19, TypeScript
- **Styling**: Tailwind CSS, Framer Motion
- **Backend**: Firebase (Authentication, Firestore, Storage)
- **State Management**: Zustand, React Query
- **Forms**: React Hook Form

## 📋 Prerequisites

- Node.js 18+ and npm
- Firebase account
- Git

## 🔧 Installation

1. Clone the repository:
```bash
git clone https://github.com/gitCarrot/ers-event-rental.git
cd ers-event-rental
```

2. Install dependencies:
```bash
npm install
```

3. Create a Firebase project:
   - Go to [Firebase Console](https://console.firebase.google.com)
   - Create a new project
   - Enable Authentication (Email/Password and Google)
   - Create a Firestore database
   - Enable Storage

4. Set up environment variables:
   - Copy `.env.local.example` to `.env.local`
   - Add your Firebase configuration:
```env
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_auth_domain
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_storage_bucket
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id
```

5. Run the development server:
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.

## 📁 Project Structure

```
ers/
├── app/                    # Next.js 15 app directory
│   ├── (auth)/            # Authentication pages
│   ├── items/             # Item browsing and management
│   ├── profile/           # User profile pages
│   └── layout.tsx         # Root layout
├── components/            # Reusable UI components
│   └── ui/               # Base UI components
├── contexts/             # React contexts
├── lib/                  # Utilities and configurations
│   └── firebase/         # Firebase configuration
├── types/                # TypeScript type definitions
└── public/               # Static assets
```

## 🔥 Firebase Setup

### Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read their own profile
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Anyone can read published items
    match /items/{itemId} {
      allow read: if resource.data.isPublished == true;
      allow write: if request.auth != null && request.auth.uid == resource.data.hostId;
    }
    
    // Bookings are private to involved parties
    match /bookings/{bookingId} {
      allow read: if request.auth != null && 
        (request.auth.uid == resource.data.renterId || 
         request.auth.uid == resource.data.hostId);
      allow create: if request.auth != null;
      allow update: if request.auth != null && 
        (request.auth.uid == resource.data.renterId || 
         request.auth.uid == resource.data.hostId);
    }
  }
}
```

### Storage Rules

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /items/{itemId}/{fileName} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /users/{userId}/{fileName} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## 🚀 Deployment

1. Build the application:
```bash
npm run build
```

2. Deploy to Firebase Hosting:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

## 📱 Features Roadmap

- [ ] Mobile app (React Native)
- [ ] Advanced search filters
- [ ] Instant messaging
- [ ] Payment processing integration
- [ ] Host analytics dashboard
- [ ] Multi-language support

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License.

## 👨‍💻 Author

Created with ❤️ by gitCarrot

---

**Note**: This is a demonstration project. For production use, ensure proper security measures, payment integration, and legal compliance.