# .github/workflows/node-deploy.yml
name: 🚀 Node.js CI/CD

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: 📥 Checkout code
      uses: actions/checkout@v3

    - name: 🟢 Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: 📦 Install dependencies
      run: npm install

    - name: ✅ Run tests
      run: npm test

    - name: 🧹 Run lint
      run: npm run lint

    - name: 🔐 Setup SSH
      uses: webfactory/ssh-agent@v0.8.0
      with:
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

    - name: 🚚 Deploy to server
      run: |
        ssh user@your-server.com "cd /var/www/my-app && git pull && npm install && pm2 restart all"
