```markdown
# Detailed Implementation Plan

This plan outlines the modifications and additions needed to build a Next.js–based e-commerce web application featuring an automatic shipping cost calculation and a COD payment system modeled after Tokopedia. It covers API endpoints, UI pages, new components, utility functions, and error handling best practices.

---

## 1. API Endpoints

### 1.1. Products API
- **File:** `src/app/api/products/route.ts`
  - **Changes:**
    - Implement a GET handler that returns a list of sample products (fields: id, name, description, price, imageURL).
    - Use try/catch for error handling; return a 500 status with an error message on failure.
  - **Note:** Use mock data for development and later integrate with a real database.

### 1.2. Orders API
- **File:** `src/app/api/orders/route.ts`
  - **Changes:**
    - Implement a POST handler to receive order data (cart items, shipping address, postal code, shipping cost, payment method “COD”).
    - Optionally implement a GET handler to list orders for order management.
    - Apply comprehensive error handling (try/catch blocks) to return proper error statuses and messages.

---

## 2. Utility Functions

### 2.1. Shipping Calculation
- **File:** `src/lib/utils.ts`
  - **Changes:**
    - Add a new function `calculateShippingCost(postalCode: string, weight: number, distanceFactor?: number): number` that simulates shipping cost calculation. For example, use a flat base rate augmented by distance or weight.
    - Ensure the function validates inputs and uses default values if parameters are missing.

---

## 3. UI Pages

### 3.1. Home Page – Product Catalog
- **File:** `src/app/page.tsx`
  - **Changes:**
    - Fetch product data from `/api/products` and render a list using a modern, responsive grid.
    - Use the new `ProductCard` component (detailed below) to display each product.
    - Show a loading state and error notifications (using existing UI components like `alert.tsx`).

### 3.2. Product Detail Page
- **File:** `src/app/products/[id]/page.tsx`
  - **Changes:**
    - Capture the dynamic product `id` from the URL.
    - Fetch detailed product data (using the GET API or from local mock data).
    - Render product information (images, description, price) and include an “Add to Cart” button.

### 3.3. Cart Page
- **File:** `src/app/cart/page.tsx`
  - **Changes:**
    - Display items stored in the cart (use local state or a React context).
    - List each item using a new `CartItem` component.
    - Show subtotal, update quantity options, and include a “Proceed to Checkout” button.

### 3.4. Checkout Page
- **File:** `src/app/checkout/page.tsx`
  - **Changes:**
    - Construct a form with fields for shipping address, postal code, and order notes.
    - On postal code input change, call the `calculateShippingCost` function (from `utils.ts`) and display dynamic shipping costs.
    - Default the payment method to “COD.”
    - On form submission, call the POST `/api/orders` endpoint and handle responses/errors with user-friendly alerts using UI components.

### 3.5. Order Management Page
- **File:** `src/app/orders/page.tsx`
  - **Changes:**
    - If user authentication is implemented, use this page to list order history.
    - Fetch order data from the Orders API and display order statuses, shipping information, and COD payment details.
    
### 3.6. Authentication (Optional)
- **Files:** 
  - `src/app/auth/signin/page.tsx`
  - `src/app/auth/signup/page.tsx`
  - **Changes:**
    - Create basic sign-in and sign-up forms styled with Tailwind CSS.
    - Validate form inputs and display error states. Use mock authentication logic for initial development.

---

## 4. New UI Components

### 4.1. ProductCard Component
- **File:** `src/components/store/ProductCard.tsx`
  - **Features:**
    - Render product image (use `<img>` tags with a placeholder URL like `https://placehold.co/400x300?text=Product+Image+Placeholder` with descriptive alt text).
    - Display product name, price, and an “Add to Cart” button.
    - Use modern typography, spacing, and layout without external icon libraries.

### 4.2. CartItem Component
- **File:** `src/components/store/CartItem.tsx`
  - **Features:**
    - Display each cart item with product details, quantity controls, and price.
    - Include error handling for missing data and update totals dynamically.

### 4.3. ShippingCost Component (Optional)
- **File:** `src/components/store/ShippingCost.tsx`
  - **Features:**
    - Render the calculated shipping cost based on user input.
    - Refresh cost dynamically as postal code or weight changes.

---

## 5. Global Styling and Configurations

### 5.1. Global Styles
- **File:** `src/app/globals.css`
  - **Changes:**
    - Update or add Tailwind CSS classes to support modern, responsive layouts for forms, lists, and grids.
    - Ensure spacing, typography, and colors meet modern UI design guidelines.

### 5.2. Next.js Configuration
- **File:** `next.config.ts`
  - **Changes:**
    - Optionally add/revise environment variable settings (e.g., API endpoints, shipping parameters) if needed.

### 5.3. README.md
- **Changes:**
    - Update with instructions to run the development server, test API endpoints (include sample curl commands), and describe the new features and folder structure.

---

## 6. Error Handling & Best Practices

- Implement try/catch blocks in all API endpoints and utility functions.
- Display user-friendly error alerts on UI pages using existing UI components (e.g., `alert.tsx`).
- Validate all form inputs (shipping address, postal code, etc.) and maintain loading states.
- Use React Suspense or fallback loading components where data fetching is asynchronous.
- Ensure graceful degradation if images fail to load by adding `onerror` handlers to `<img>` tags.

---

## Summary

- Created API endpoints for products and orders with robust error handling.
- Added a shipping cost calculation function in `utils.ts` for automatic cost updates.
- Developed modern, responsive pages for Home, Product Details, Cart, Checkout, and Order Management.
- Built new UI components (`ProductCard`, `CartItem`, and optionally `ShippingCost`) using clean typography and layout.
- Updated global styles and Next.js configuration to support the new features.
- Provided clear mechanisms for user authentication and order processing with a COD payment method.
- Detailed error handling measures and form validations are integrated throughout the system.
- All changes follow best practices and component reusability while maintaining consistency with the existing codebase.
