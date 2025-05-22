# Structure of the project

```
react-app-template/
├── public/ # Contains static files (index.html, favicon, manifest,...)
│ ├── index.html
│ └── ...
├── src/ # Contains all application source code
│ ├── App.tsx # Root component of the application
│ ├── main.tsx # Entry point of the application, render App.ts
│ ├── reportWebVitals.ts
│ ├── setupTests.ts
│ │
│ ├── assets/ # Contains static resources such as images, fonts,...
│ │ ├── images/
│ │ └── fonts/
│ │
│ ├── components/ # Contains reusable UI components, no complex business logic
│ │ ├── common/
│ │ │ ├── Button/
│ │ │ │ ├── Button.ts
│ │ │ │ └── Button.module.css # Or Button.scss, Button.styles.ts
│ │ │ └── ...
│ │ ├── content/
│ │ │ ├── Header/
│ │ │ │ ├── Header.ts
│ │ │ │ └── Header.module.css # Or Header.scss, Header.styles.ts
│ │ │ └── ...
│ │ ├── layouts/ # Contains common layout structures (e.g., MainLayout, AdminLayout)
│ │ │ ├── MainLayout/
│ │ │ │ ├── MainLayout.ts
│ │ │ │ └── MainLayout.module.css
│ │ │ └── ...
│ │ └── ...
│ │
│ ├── constants/ # Contains application constants (e.g., action types, routes paths)
│ │ ├── products.ts
│ │ ├── ...
│ │ ├── actionTypes.ts
│ │ ├── routes.ts
│ │ └── index.ts
│ │
│ ├── contexts/ # (optional) For state management with React Context API
│ │ ├── AuthContext.ts
│ │ └── ThemeContext.ts
│ │
│ ├── hooks/ # Contains reusable global custom React Hooks
│ │ ├── useAuth.ts
│ │ ├── useForm.ts
│ │ └── api/ # Hooks for API calls using react-query
│ │ ├── useAuthApi.ts
│ │ ├── useProductsApi.ts
│ │ └── ...
│ │
│ ├── services/ (or apis/) Contains API configuration and external connection functions
│ │ ├── instances/
│ │ │ ├── testClient.ts
│ │ │ ├── axiosClient.ts # Axios instance configuration
│ │ ├── helpers/
│ │ │ ├── users.ts
│ │ │ ├── products.ts
│ │ │ └── ...
│ │ └── ...
│ │
│ ├── pages/ # (Alternative approach) Contains components representing pages/routes
│ │ ├── products/
│ │ │ ├── components/
│ │ │ ├── ProductDetail.tsx
│ │ │ ├── ProductList.tsx
│ │ │ ├── ProductCreate.tsx
│ │ │ ├── ProductEdit.tsx
│ │ │ └── ...
│ │ └── AboutPage.tsx
│ │ └── ...
│ │
│ ├── routes/ # Manages application routing
│ │ ├── AppRoutes.ts
│ │ ├── ProtectedRoutes.ts - optional
│ │
│ ├── stores/
│ │ ├── slices/
│ │ │ ├── authStore.ts
│ │ │ ├── cartStore.ts
│ │ │ └── ...
│ │ ├── hooks/
│ │ │ ├── useAuthStore.ts
│ │ │ ├── useCartStore.ts
│ │ │ └── ...
│ │ └── index.ts
│ │
│ ├── styles/
│ │ ├── global.css
│ │ ├── variables.css
│ │ └── ...
│ │
│ ├── theme/
│ │ ├── colors.ts
│ │ ├── fonts.ts
│ │ ├── typography.ts
│ │ └── index.ts
│ │
│ ├── types/
│ │ ├── index.ts
│ │ └── products.d.ts
│ │
│ ├── utils/
│ │ ├── number.ts
│ │ ├── string.ts
│ │ └── ...
│
├── .env # Environment variables
├── .eslintignore
├── .eslintrc.ts # ESLint configuration
├── .gitignore
├── .prettierrc.ts # Prettier configuration
├── package.tson
├── README.md
└── jsconfig.tson # (Or tsconfig.tson if using TypeScript) Absolute path configuration, etc.
```

## Packages không khuyến khích:

- Styled Components: khách hàng MSS đang có xu hướng không muốn sử dụng CSS-in-JS (viết CSS trong JavaScript) mà muốn sử dụng utility-first CSS như tailwindcss kết hợp với ant design để sử dụng cho spacing, color, typography, sizing,...

- Hạn chế: style inline

```tsx
<div style={{ color: "red", fontSize: "16px" }}>Hello</div>
```

## Packages cần cài thêm và khuyến khích:

- Husky: Git hooks
- @tanstack/react-query: data fetching management
- dotenv: environment variables

## Ngoài structure, nên viết thêm nhiều code base hơn cho project, ví dụ:

### 1. main.tsx: config sẵn các provider cần thiết cho app

- React Query Provider
- React Router Provider
- React Zustand Provider
- Ant Design Provider
- Error Boundary
- ...

### 2. services/

- config các instance của axios: error handling, interceptor request, interceptor response, authentication, ...
- tạo example cho helper:

```ts
const exampleApi = {
  getList: async () => {
    const response = await axiosInstance.get("/api/example");
  },
  getDetail: async (id: string) => {
    const response = await axiosInstance.get(`/api/example/${id}`);
  },
};
```

### 3. hooks/

- config các custom hooks để call api:

```ts
export const useProductApi = () => {
  const useProductList = () => {
    const { data, isLoading, error } = useQuery({
      queryKey: ["products"],
      queryFn: productApi.getList,
    });
  };

  const useProductDetail = (id: string) => {
    const { data, isLoading, error } = useQuery({
      queryKey: ["products", id],
      queryFn: () => productApi.getDetail(id),
    });
  };

  return { useProductList, useProductDetail };
};
```

### 4. types/

- config các type cho project:

```ts
export namespace Product {
  export type ProductListRes = ProductListResponse;
  export type ProductDetailRes = ProductDetailResponse;
}

interface ProductListResponse {
  data: Product[];
  total: number;
}

interface Product {
  id: string;
  name: string;
  price: number;
}

interface ProductDetailResponse {
  data: Product;
}
```

### 5. pages/

- Configure error pages and other static pages:
  - 404 (Not Found)
  - 500 (Internal Server Error)
  - ...
  - /dev page: nơi viết những example code cho các components
- prefix folder name: [name]-page: ví dụ: product-page, user-page, ...

### 6. stores/

...

### 7. theme/

...
