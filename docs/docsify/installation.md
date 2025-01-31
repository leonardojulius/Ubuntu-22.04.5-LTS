## Step 1: Install Node.js and npm

Docsify requires Node.js and npm. Install them using the following commands:

```bash
sudo apt update
```

```bash
sudo apt install nodejs npm -y
```
Verify the installation:


```bash
node -v
```

```bash
npm -v
```


## Step 2: Install Docsify

Use npm to install Docsify globally:

```bash
npm install -g docsify-cli
```

Verify the installation:

```bash
docsify -v
```


## Step 3: Create a Docsify Project

- 1. Create a new folder for your project

```bash
mkdir my-docs
```

```bash
cd my-docs
```

- 2. Initialize Docsify

```bash
docsify init ./docs
```

This will create a docs folder with the following files:

- `index.html`
- `README.md`

## Step 4: Run Docsify Server

Start the server:

```bash
docsify serve docs
```

By default, Docsify runs on http://localhost:3000. Open your browser and visit:

```bash
http://localhost:3000
```