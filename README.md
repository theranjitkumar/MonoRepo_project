#  MonoRepo project in 8 steps

### 1. Create the Root Directory
    mkdir my-monorepo
    cd my-monorepo
    pnpm init -y

### 2. Set Up PNPM Workspaces
    Add this to the package.json:
    {
    "name": "my-monorepo",
    "private": true,
    "version": "1.0.0",
    "workspaces": [
        "apps/*",
        "packages/*"
     ]
    }

### 3. Create Folder Structure
    mkdir -p apps/web
    mkdir -p apps/admin
    mkdir -p apps/api

### 4. Install Angular CLI and Create Apps
    Install Angular CLI globally if not already installed:
    pnpm add -g @angular/cli

    Now scaffold Angular projects inside apps:
    cd apps/web
    ng new web --directory . --skip-install
    pnpm install

    cd ../admin
    ng new admin --directory . --skip-install
    pnpm install

    Update apps/web/angular.json and apps/admin/angular.json with different outputPath if needed to avoid conflicts.

### 5. Create Express API App
    Go to apps/api and initialize:
    cd ../api
    pnpm init -y
    pnpm add express

    Create a simple index.js:

    // apps/api/index.js
    const express = require('express');
    const app = express();
    const PORT = process.env.PORT || 3000;

    app.get('/api/hello', (req, res) => {
    res.json({ message: 'Hello from Express API' });
    });

    app.listen(PORT, () => {
    console.log(`API running at http://localhost:${PORT}`);
    });

###  6. Add Start Scripts in Root
    Update root package.json with scripts:

    "scripts": {
    "dev:web": "pnpm --filter web start",
    "dev:admin": "pnpm --filter admin start",
    "dev:api": "pnpm --filter api start",
    "dev:all": "concurrently \"pnpm dev:web\" \"pnpm dev:admin\" \"pnpm dev:api\""
    },
    "devDependencies": {
    "concurrently": "^8.0.0"
    }

    Install concurrently:
    pnpm add -D concurrently
### 7. (Optional) Shared Packages Folder
    If you need shared TypeScript interfaces or services, create packages/shared:
    mkdir -p packages/shared
    pnpm init -y

    Add a shared file like interfaces.ts, and reference it via tsconfig.paths in Angular and Express apps.

### 8.Run All Projects
    pnpm dev:all

### Runng URL's
    http://localhost:4200 - Web Application
    http://localhost:4201 - Admin Portal
    http://localhost:3000 - API

### Recap Folder Structure
    ├── apps/
    │   ├── web/         # Angular public website
    │   ├── admin/       # Angular admin panel
    │   └── api/         # Express API backend
    ├── packages/
    │   └── shared/      # (Optional) shared code like interfaces
    ├── package.json     # Root with PNPM workspaces


### Benefits
    Fast installs with PNPM

    Single node_modules at root

    Code sharing via packages/

    Dev script control per app or all at once
