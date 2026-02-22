# 🚀 Summer Camp Sports Website

A multi-page React application built with Create React App, with automated CI/CD deployment using GitHub Actions + Firebase Hosting.

---

## 📌 Project Overview

This project is a modern React website for a Summer Camp Sports program with:
- Home page
- Sports page
- Contact/Enquiry page
- Clean UI + routing
- Auto-deploy on every push to `main`

---

## 🛠 Tech Stack

- **Frontend:** React (Create React App)
- **Routing:** react-router-dom
- **CI/CD:** GitHub Actions
- **Hosting:** Firebase Hosting
- **Version Control:** Git & GitHub

---

## 📂 Project Structure

summercamp_update/
- public/
- src/
- firebase.json
- .firebaserc
- package.json
- package-lock.json
- .github/
  └── workflows/
      └── deploy.yml
- README.md

---

## ✅ Full Build + Deploy Flow

Local changes → push to `main` → GitHub Actions runs → `npm ci` → `npm run build` → deploy to Firebase Hosting → live site updates.

---

# 1) Create React App & Setup Pages

## 1.1 Create the project
npx create-react-app summercamp_update  
cd summercamp_update  

## 1.2 Install router
npm install react-router-dom  

## 1.3 Create pages & routing
Create pages inside `src/pages/`:
- Home.js
- Sports.js
- Contact.js

Add routing in `src/App.js` using `BrowserRouter`, `Routes`, `Route`.

---

# 2) Firebase Hosting Setup (Manual Hosting Setup)

## 2.1 Install Firebase CLI
npm install -g firebase-tools  

## 2.2 Login (local machine only)
firebase login  

## 2.3 Build the React app (creates `build/` folder locally)
npm run build  

## 2.4 Initialize Firebase Hosting in the project folder
firebase init hosting  

Choose these options carefully:
- **Use an existing project** (select your Firebase project)
- **Public directory:** build
- **Configure as a single-page app (rewrite all URLs to /index.html):** Yes
- **Set up automatic builds and deploys with GitHub?** No (we do it manually via workflow)
- **Overwrite files?** NO (never overwrite your React files)

## 2.5 Verify firebase.json (must deploy `build`)
{
  "hosting": {
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      { "source": "**", "destination": "/index.html" }
    ]
  }
}

## 2.6 First-time manual deploy (optional but recommended once)
firebase deploy --only hosting  

This confirms Firebase Hosting works before CI/CD.

---

# 3) CI/CD Setup (GitHub Actions → Firebase Hosting)

## 3.1 Generate Firebase CI token (local machine)
firebase login:ci  

Copy the token printed in terminal.

## 3.2 Add token as GitHub Secret
GitHub Repo → Settings → Secrets and variables → Actions → New repository secret
- Name: FIREBASE_TOKEN
- Value: (paste token)

## 3.3 Create workflow file
Create this file in your repo:
.github/workflows/deploy.yml

Paste and update PROJECT_ID:

name: Deploy React App to Firebase

on:
  push:
    branches: [ "main" ]

jobs:
  build_and_deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: "npm"

      - name: Install Dependencies
        run: npm ci

      - name: Build React App
        run: npm run build

      - name: Deploy to Firebase Hosting
        run: npx firebase-tools deploy --only hosting --project summer-camp-499ea-b1c8b --token "${{ secrets.FIREBASE_TOKEN }}"



## 3.4 Push workflow to main
git add .github/workflows/deploy.yml firebase.json .firebaserc  
git commit -m "Add Firebase CI/CD deploy workflow"  
git push origin main  

---

# 4) Verify Deployment

1) GitHub → Actions → confirm latest workflow run is green ✅  
2) Open hosting URL:

https://summer-camp-499ea-b1c8b.web.app/



## 🌍 Live Demo
https://summer-camp-499ea-b1c8b.web.app/
