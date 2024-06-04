## scss conversion

scss conversion

```powershell
npm init -y
npm install sass --save-dev

```

in `package.json`

```json
"scripts": {
  "build-css": "sass sass/main.scss assets/css/style.css --source-map",
  "watch-css": "sass --watch sass/main.scss:assets/css/style.css --source-map"
}
```

```powershell
npm run build-css
npm run watch-css
```
