# Abreez-Stock

# Abreez Platform Manual

## 1. Overview

**Abreez** is a B2B e-commerce platform built specifically for the promotional products and corporate gifts industry. You can visit the live site at **[abreezstock.com](https://abreezstock.com)**.

Unlike standard retail stores, this market has unique requirements:
1.  **High Volume:** Orders are often in bulk.
2.  **Complex Variants:** A single pen might come in 10 colors; a t-shirt might have 5 sizes and 6 colors.
3.  **Visual Discovery:** Customers often have a physical sample or a photo of what they want but don't know the product name.

Abreez solves these problems with a specialized inventory system and a custom-built **Visual Search** engine.

## 2. Key Features

### Visual Search
This is the platform's core differentiator. Customers can upload a photo of a product (e.g., a specific style of mug or notebook), and the system will find visually similar items in the catalog.
-   **Privacy:** Images are processed securely.
-   **Speed:** Results appear in under 2 seconds.
-   **Accuracy:** It matches shape, color, and texture, not just keywords.

### Smart Inventory
-   **Color Variants:** Track stock levels for every color option.
-   **Size Grids:** For apparel, manage stock across size matrices (S, M, L, XL, etc.).
-   **Real-time Updates:** Stock levels adjust immediately after orders.

### Localization
The platform is fully bilingual:
-   **English:** Left-to-Right (LTR) layout.
-   **Arabic:** Right-to-Left (RTL) layout with native typography.

## 3. User Guide

### Finding Products

**Method A: Text Search**
Use the search bar at the top. It supports fuzzy matching, so "blu pen" will still find "Blue Ballpoint Pen".

**Method B: Image Search**
1.  Click the **Camera Icon** 📷 in the search bar.
2.  **Upload** a photo from your device or **Take a Photo** if you are on mobile.
3.  The system will analyze the image and display the closest matches from the catalog.
    *   *Tip: Use a clear background for best results.*

### Managing Products (Admin)

1.  Log in to the Admin Dashboard.
2.  Navigate to **Products**.
3.  **Adding a Product:**
    *   Fill in the basic details (Name, Description, Base Price).
    *   **Variants:** Add color options. For each color, you can upload specific images.
    *   **Stock:** Enter the quantity available for each variant.

## 4. Technical Architecture

We designed the system to be robust, scalable, and cost-effective.

### The Stack
-   **Frontend:** Next.js 14. We use Server Components for performance and SEO.
-   **Database:** PostgreSQL with Prisma ORM. This ensures data integrity.
-   **Authentication:** Clerk. Provides secure, passwordless login and session management.
-   **Styling:** Tailwind CSS.

### How Image Search Works
We didn't use a third-party API for this; we built a custom engine to keep costs low and privacy high.

1.  **Vectorization (OpenCLIP):** When an image is uploaded, it passes through a machine learning model called OpenCLIP. This model converts the image into a mathematical vector (a list of numbers) that represents its visual features.
2.  **Similarity Search (Qdrant):** We store these vectors in Qdrant, a specialized vector database. When a user searches, we compare their image's vector against our database to find the "nearest neighbors"—the products that look the most similar.

## 5. Repository Structure

This section explains where to find key components in the codebase.

### Core Directories
-   **`app/`**: The main application code (Next.js App Router).
    -   `api/`: Backend API endpoints (e.g., `/api/image-search/upload`).
    -   `[lng]/`: All pages are inside this dynamic route for internationalization (English/Arabic).
-   **`components/`**: Reusable UI elements.
    -   `image-search/`: Specific components for the visual search feature (Upload, Results).
    -   `ui/`: Generic UI components (Buttons, Inputs) built with Radix UI.
-   **`lib/`**: Utility functions and business logic.
    -   `services/`: Contains the core logic for Image Search (`openclip-service.js`, `qdrant-service.js`).
    -   `prisma.js`: Database client instance.
-   **`prisma/`**: Database configuration.
    -   `schema.prisma`: Defines the data model (Products, Variants, Stock).
-   **`public/`**: Static assets (images, fonts).
-   **`scripts/`**: Maintenance scripts.
    -   `index-product-images.js`: The script used to generate vectors for all product images.

## 6. Deployment & Operations

### Requirements
-   **Node.js:** Version 18 or higher.
-   **Docker:** Required for running the Qdrant vector database.

### Quick Start
1.  **Clone the repository.**
2.  **Install dependencies:**
    ```bash
    npm install
    ```
3.  **Start the infrastructure:**
    ```bash
    docker-compose up -d
    ```
    *This starts the Qdrant vector database.*
4.  **Run the application:**
    ```bash
    npm run dev
    ```

### Production Recommendations
-   **Hosting:** Vercel is recommended for the Next.js frontend.
-   **Database:** Use a managed PostgreSQL provider (e.g., Supabase, Neon, or AWS RDS).
-   **Search Engine:** For production, you can self-host Qdrant on a VPS (DigitalOcean/Hetzner) to save significant costs compared to managed vector services.
