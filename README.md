<div align="center">
    <h2>
        ⚙ <img src="./public/vite.svg" className="logo" alt="Vite logo" />
        V I T E &nbsp; D E P L O Y 
        <img src="./public/vite.svg" className="logo" alt="Vite logo" /> ⚙
    </h2>
    <a href="https://andredavedovicz.github.io/vite-deploy/" target="_blank">🔗 Working in GitHub Pages</a>
    <h3>How add a vite project(deploy) to GitHub Pages? </h3>
    <h3>Whenever you push to GitHub, it will deply automatically</h3>
    <h4>React: <a href="https://github.com/andredavedovicz/react-deploy" target="_blank">If you want to deploy a react app see this repository!</a></h4>
</div>



<br>

### Follow the steps below on how to deploy a vite react app:

#### 01. Create a vite react app
```npm
npm create vite@latest
```

#### 02. Create a new repository on GitHub and initialize GIT
```git
git init 
git add . 
git commit -m "add: initial files" 
git branch -M main 
git remote add origin https://github.com/[USER]/[REPO_NAME] 
git push -u origin main
```

#### 03. Setup base on *vite.config*
```js
base: "/."
```

#### 04. Create ./github/workflows/deploy.yml and add the code bellow
```yml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

#### 05. Push
```git
git add . 
git commit -m "add: deploy workflow" 
git push
```

#### 06. Active workflow
```js
Settings -> Actions -> General -> Workflow permissions -> Read and Write permissions 
Settings -> Pages -> gh-pages -> save
(if gh-pages it's not showing something is wrong!) 
Actions -> failed deploy -> re-run-job failed jobs 

```

#### 06. For code changes
```git
git add . 
git commit -m "fix: some bug" 
git push
```
<h3>Remember:Whenever you push to GitHub, it will deply automatically</h3>
