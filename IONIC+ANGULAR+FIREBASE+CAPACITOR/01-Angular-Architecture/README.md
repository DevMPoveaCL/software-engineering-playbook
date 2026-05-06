# 🏗️ 01 - Angular Architecture

> **The Engine.** Angular is a full-featured framework—not a library. It has opinions about everything: folder structure, HTTP calls, state management. React is Lego; Angular is a prefab house with plumbing included.

## 🤔 Angular vs React: Key Differences

| Aspect | Angular | React |
|--------|---------|-------|
| **Type** | Full framework (batteries included) | Library (UI only) |
| **Language** | TypeScript (required) | JavaScript/TypeScript |
| **Data Binding** | Two-way binding (`[(ngModel)]`) | One-way data flow |
| **Change Detection** | Zone.js (automatic) | Virtual DOM |
| **Learning Curve** | Steeper | gentler |

## 🏛️ Angular Module Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        AppModule                                 │
├─────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Feature     │  │  Shared     │  │  Core        │          │
│  │  Modules     │  │  Module     │  │  Module      │          │
│  │              │  │             │  │             │          │
│  │  - Dashboard │  │  - Button   │  │  - Auth      │          │
│  │  - Orders    │  │  - Card     │  │  - Guard     │          │
│  │  - Products  │  │  - Pipe     │  │  - Service   │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

### Analogy: The Department Store
- **CoreModule** = Back offices (HR, Accounting—only loaded once)
- **SharedModule** = Standard supplies (printer paper, pens—all departments use)
- **FeatureModule** = Departments (Electronics, Clothing—separate responsibilities)

---

## 📦 Angular Component Structure

```
src/app/features/products/
├── product-list/
│   ├── product-list.component.ts
│   ├── product-list.component.html
│   ├── product-list.component.scss
│   └── product-list.component.spec.ts
├── product-detail/
│   └── ...
└── product.service.ts
```

### Component: The Complete Picture

```typescript
@Component({
  selector: 'app-product-card',
  templateUrl: './product-card.component.html',
  styleUrls: ['./product-card.component.scss'],
})
export class ProductCardComponent implements OnInit, OnDestroy {
  @Input() product!: Product;      // Parent → Child (like props)
  @Output() addToCart = new EventEmitter<Product>(); // Child → Parent

  // Dependency injection (SOLID: Dependency Inversion)
  constructor(private productService: ProductService) {}

  ngOnInit() { /* fetch data */ }
  ngOnDestroy() { /* cleanup */ }
}
```

---

## 🔄 MVC Pattern in Angular

```
┌─────────────────────────────────────────────────────────────────┐
│                         Angular MVC                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Model              View                Controller               │
│  ─────              ────                ───────────              │
│  product.ts    ←──  product.html   ←──  product.component.ts   │
│  (data)             (template)          (logic)                 │
│                                                                 │
│  @Injectable    ←──  *ngFor, {{ }}   ←──  class ProductCtrl {  │
│  service                                    this.products = [] │
│                                              this.load()       │
└─────────────────────────────────────────────────────────────────┘
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Components are thin (logic only) | Logic in templates | Testability |
| Services handle data fetching | Components calling HTTP directly | Reusability |
| Templates are declarative | Complex expressions in templates | Readability |
| One responsibility per component | God components doing everything | SRP |

---

## 💉 Dependency Injection (SOLID: Dependency Inversion)

```typescript
// Service is injected, not instantiated
@Injectable({ providedIn: 'root' })
export class ProductService {
  constructor(private http: HttpClient) {} // Angular injects HttpClient

  getProducts(): Observable<Product[]> {
    return this.http.get<Product[]>('/api/products');
  }
}
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| `providedIn: 'root'` for singletons | Creating services with `new` | Testability, lazy loading |
| Inject abstractions | Inject concrete classes | Swappable implementations |
| Constructor injection | Property injection | Forces dependency declaration |

---

## 🛡️ Guards (Route Protection)

```typescript
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private auth: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.auth.isLoggedIn()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

| What to do ✅ | What NOT to do ❌ | Why |
|--------------|-------------------|-----|
| Protect routes with guards | Client-side hide only | Security |
| Combine with JWT validation | Trust client state alone | Server is truth |
| Handle all auth states | Only logged-in/logged-out | Granular access control |

---

## 🔗 Related Topics

[⬅️ Back to Parent](../README.md) | [➡️ Next: Ionic UI](../02-Ionic-UI/README.md)

---

*Principles applied: MVC, SOLID (DI), SRP, Clean Architecture*
