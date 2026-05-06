# 🔥 03 - Firebase + Capacitor

> **The Backend-as-a-Service Pair.** Firebase eliminates server coding; Capacitor wraps your web app into a native binary. Together, you deploy to the App Store without writing Swift or Kotlin.

## 🔮 Firebase: What It Provides

```
┌─────────────────────────────────────────────────────────────────┐
│                         FIREBASE                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Firestore   │  │  Auth        │  │  Storage     │          │
│  │  (Database)  │  │  (Users)     │  │  (Files)     │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Hosting     │  │  Functions   │  │  Messaging   │          │
│  │  (Web)       │  │  (Serverless)│  │  (Push)      │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## ⚡ Capacitor: The Bridge

```
┌─────────────────────────────────────────────────────────────────┐
│                      YOUR CODE (TypeScript)                     │
│                                                                 │
│                    Web App (Angular + Ionic)                    │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                      Capacitor Runtime                          │
│                    (Native Bridge)                              │
├──────────────────────┬──────────────────────┬─────────────────┤
│    iOS Package        │   Android Package    │  Web Bundle     │
│    (.ipa)             │   (.apk)             │  (Firebase)     │
└───────────────────────┴──────────────────────┴─────────────────┘
```

## 🔥 Firestore Data Model

### Collections vs Documents

```
┌─────────────────────────────────────────────────────────┐
│  restaurants (collection)                                │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────┐     │
│  │  restaurant-1 (document)                        │     │
│  │  ├── name: "Pizza Palace"                        │     │
│  │  ├── rating: 4.5                                 │     │
│  │  └── dishes (subcollection)                     │     │
│  │      ├── dish-1: { name: "Margherita", price: 12 } │
│  │      └── dish-2: { name: "Pepperoni", price: 14 } │
│  └─────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────┘
```

---

## 🔐 Firebase Auth (User Management)

```typescript
import { Auth } from '@angular/fire/auth';

@Injectable({ providedIn: 'root' })
export class AuthService {
  constructor(private auth: Auth) {}

  async signIn(email: string, password: string) {
    return signInWithEmailAndPassword(this.auth, email, password);
  }

  async signOut() {
    return signOut(this.auth);
  }

  get currentUser() {
    return this.auth.currentUser;
  }
}
```

| Provider | Best For |
|---------|----------|
| Email/Password | Traditional apps |
| Google | Quick onboarding |
| Apple | iOS apps (required for App Store) |
| Anonymous | Temporary data |

---

## 📸 Capacitor: Accessing Native Features

### Camera Plugin
```typescript
import { Camera, CameraResultType } from '@capacitor/camera';

async function takePhoto() {
  const image = await Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Base64,
  });
  return image.base64String;
}
```

### Available Plugins
- **Camera** — Photo capture
- **Geolocation** — GPS location
- **Push Notifications** — Firebase Cloud Messaging
- **Haptics** — Vibration feedback
- **Filesystem** — Local file access

---

## ✅ What to Do / What NOT to Do

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Firestore security rules | Open to everyone | Data protection |
| Indexes for queries | Unindexed collection scans | Performance |
| Offline persistence | Ignore offline mode | UX |
| Capacitor build before release | Debug build only | Performance, store compliance |
| Sign out on token expiry | Keep expired sessions | Security |

---

## 🚀 Deployment Pipeline

```
┌──────────┐    ┌──────────────┐    ┌───────────────┐    ┌──────────┐
│  Commit  │───►│  GitHub      │───►│  Build (npm)  │───►│  Deploy  │
│          │    │  Actions     │    │  + Capacitor  │    │  Stores  │
└──────────┘    └──────────────┘    └───────────────┘    └──────────┘
                                                    │
                              ┌─────────────────────┼─────────────────────┐
                              ▼                     ▼                     ▼
                        ┌──────────┐          ┌──────────┐          ┌──────────┐
                        │  iOS     │          │ Android  │          │  Web     │
                        │  TestFlight│        │  Play Store│        │ Firebase│
                        └──────────┘          └──────────┘          └──────────┘
```

---

## 🗂️ Project Structure with Firebase + Capacitor

```
src/
├── environments/
│   ├── environment.ts         # Dev Firebase config
│   └── environment.prod.ts    # Prod Firebase config
├── services/
│   ├── auth.service.ts        # Firebase Auth
│   ├── restaurant.service.ts  # Firestore
│   └── storage.service.ts     # Firebase Storage
└── app.module.ts              # AngularFire imports
```

---

## 🔗 Related Topics

- **[⬅️ Previous: Ionic UI](../02-Ionic-UI/README.md)** | **[⬅️ Back to Parent](../README.md)**

---

*Principles applied: BaaS architecture, Security by default, Cross-platform deployment*
