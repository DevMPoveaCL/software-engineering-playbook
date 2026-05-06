# 🎨 02 - Ionic UI

> **The Pretty Face.** Ionic provides the mobile look-and-feel. It wraps Angular components with platform-adaptive styling. On iOS, it looks iOS-native. On Android, Material Design. Same code, both platforms.

## 📱 Ionic vs Raw HTML

```
┌─────────────────────────┐    ┌─────────────────────────┐
│     Standard HTML       │    │        Ionic UI          │
├─────────────────────────┤    ├─────────────────────────┤
│  <div> Click </div>     │    │  <ion-button>          │
│                         │    │    Click                │
│  Always looks like web  │    │  </ion-button>         │
│  on mobile              │    │                        │
│                         │    │  Adapts to platform    │
│                         │    │  automatically         │
└─────────────────────────┘    └─────────────────────────┘
```

## ⚡ Ionic Components (Golden Rule)

> **❌ DON'T use standard HTML tags like `<button>` or `<div>` unless necessary.**
> **✅ DO use Ionic components like `<ion-button>` or `<ion-card>`.**

These components automatically respect each platform's design standards:
- **iOS**: Apple Human Interface Guidelines
- **Android**: Material Design

---

## 🧱 Ionic Grid System

```
┌────────────────────────────────────────────────────────────────┐
│                        ion-grid                               │
├─────────────┬─────────────┬─────────────┬─────────────┬────────┤
│  ion-col   │  ion-col   │  ion-col   │  ion-col   │  ...    │
│  size="3"  │  size="3"  │  size="3"  │  size="3"  │        │
│  (25%)     │  (25%)     │  (25%)     │  (25%)     │        │
└─────────────┴─────────────┴─────────────┴─────────────┴────────┘
```

### Responsive Grid
```html
<ion-grid>
  <ion-row>
    <ion-col size-xs="12" size-sm="6" size-md="4">
      <ion-card>Card 1</ion-card>
    </ion-col>
    <ion-col size-xs="12" size-sm="6" size-md="4">
      <ion-card>Card 2</ion-card>
    </ion-col>
  </ion-row>
</ion-grid>
```

---

## 🔘 Common Ionic Components

| Component | Purpose | Platform Adaptation |
|-----------|---------|---------------------|
| `ion-button` | Primary action | iOS: filled rounded, Android: filled with ripple |
| `ion-card` | Content container | Shadow, rounded corners |
| `ion-input` | Text entry | Keyboard type, validation styling |
| `ion-list` | Scrollable items | iOS: inset, Android: full-width |
| `ion-toggle` | On/off switch | Native toggle feel |
| `ion-spinner` | Loading indicator | Platform-specific animation |

---

## 🎯 Forms in Ionic

```typescript
// Reactive Forms (recommended)
@Component({})
export class LoginPage implements OnInit {
  form = this.fb.group({
    email: ['', [Validators.required, Validators.email]],
    password: ['', [Validators.required, Validators.minLength(6)]],
  });

  constructor(private fb: FormBuilder) {}

  onSubmit() {
    if (this.form.valid) {
      // Handle submission
    }
  }
}
```

```html
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <ion-item>
    <ion-label position="floating">Email</ion-label>
    <ion-input formControlName="email" type="email"></ion-input>
  </ion-item>

  <ion-item>
    <ion-label position="floating">Password</ion-label>
    <ion-input formControlName="password" type="password"></ion-input>
  </ion-item>

  <ion-button expand="block" type="submit" [disabled]="!form.valid">
    Login
  </ion-button>
</form>
```

---

## 🔄 Ionic Lifecycle

| Lifecycle Hook | When It Fires | Use Case |
|---------------|--------------|----------|
| `ngOnInit` | Component initialized | Fetch initial data |
| `ionViewWillEnter` | About to become active | Refresh data each visit |
| `ionViewDidEnter` | Animation complete | Focus inputs, start timers |
| `ionViewWillLeave` | About to leave | Cancel subscriptions |
| `ngOnDestroy` | Component destroyed | Cleanup |

---

## ✅ What to Do / What NOT to Do

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Use Ionic components | Use raw `<div>` for UI | Platform adaptation |
| Platform-specific logic with `Platform` service | Ignore the platform | iOS/Android differences |
| Safe area padding for notches | Hardcode offsets | Different devices |
| Loading spinners during API calls | Silent fetches | UX |
| Pull-to-refresh for lists | Static data only | Native feel |

---

## 🎨 Theming and CSS Variables

```css
/* src/theme/variables.scss */
:root {
  --ion-color-primary: #3880ff;
  --ion-color-secondary: #3dc2ff;
  --ion-font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI';
}
```

---

## 🔗 Related Topics

- **[⬅️ Previous: Angular Architecture](../01-Angular-Architecture/README.md)** | **[⬅️ Back to Parent](../README.md)** | **[➡️ Next: Firebase + Capacitor](../03-Firebase-Capacitor/README.md)**

---

*Principles applied: Platform adaptation, UX best practices*
