# MongoDB Setup Guide for Music Spot

## Important: Music Files are NOW Stored in MongoDB! 

I've updated the code to use **MongoDB GridFS** to store music files directly in the database. This means:
- ✅ Music files are in MongoDB, not local filesystem
- ✅ Works across different servers and devices
- ✅ When you deploy to cloud, music files move with the database
- ✅ Users can access their music from anywhere

---

## Option 1: MongoDB Atlas (Cloud) - RECOMMENDED for Production

This is the best option for your project since you want to deploy on a server.

### Step 1: Create MongoDB Atlas Account

1. Go to https://www.mongodb.com/cloud/atlas
2. Click "Try Free"
3. Sign up with your email or Google account
4. Choose the **FREE tier** (M0 Sandbox - 512MB storage)

### Step 2: Create a Cluster

1. After login, click "Build a Database"
2. Choose **FREE** shared cluster
3. Choose a cloud provider and region (choose one close to you):
   - **AWS** - Hong Kong or Singapore (good for Asia)
   - Or choose any free region
4. Cluster Name: `MusicSpot` (or any name you like)
5. Click "Create Cluster" (takes 3-5 minutes)

### Step 3: Create Database User

1. Go to "Database Access" in left sidebar
2. Click "Add New Database User"
3. Authentication Method: **Password**
4. Username: `musicspot_user` (or any name)
5. Password: Click "Autogenerate Secure Password" and **SAVE IT**
6. Database User Privileges: **Read and write to any database**
7. Click "Add User"

### Step 4: Whitelist IP Address

1. Go to "Network Access" in left sidebar
2. Click "Add IP Address"
3. For development: Click "Allow Access from Anywhere" (0.0.0.0/0)
   - This is okay for learning projects
   - For production, you should add specific IPs
4. Click "Confirm"

### Step 5: Get Connection String

1. Go back to "Database" in left sidebar
2. Click "Connect" button on your cluster
3. Choose "Connect your application"
4. Driver: **Node.js**, Version: **4.1 or later**
5. Copy the connection string. It looks like:
   ```
   mongodb+srv://musicspot_user:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
6. Replace `<password>` with your actual password
7. Add database name before the `?`. Final string should look like:
   ```
   mongodb+srv://musicspot_user:yourpassword@cluster0.xxxxx.mongodb.net/musicspot?retryWrites=true&w=majority
   ```

### Step 6: Configure Your Application

1. In your project folder, create a file named `.env`:
   ```
   MONGODB_URI=mongodb+srv://musicspot_user:yourpassword@cluster0.xxxxx.mongodb.net/musicspot?retryWrites=true&w=majority
   SESSION_SECRET=your-random-secret-key-12345
   PORT=3000
   ```

2. Replace with your actual connection string from Step 5

---

## Option 2: Local MongoDB on Ubuntu (Development Only)

Use this if you want to test locally on your Ubuntu server first.

### Step 1: Install MongoDB on Ubuntu

```bash
# Import MongoDB public key
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor

# Add MongoDB repository
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

# Update package list
sudo apt update

# Install MongoDB
sudo apt install -y mongodb-org

# Start MongoDB service
sudo systemctl start mongod

# Enable MongoDB to start on boot
sudo systemctl enable mongod

# Check status
sudo systemctl status mongod
```

### Step 2: Configure Your Application

Create a `.env` file in your project folder:
```
MONGODB_URI=mongodb://localhost:27017/musicspot
SESSION_SECRET=your-random-secret-key-12345
PORT=3000
```

---

## How to Run Your Application

### Step 1: Transfer Files to Ubuntu

From your Windows machine, transfer your project to Ubuntu:

```bash
# On Ubuntu, create project directory
mkdir -p ~/musicspot
cd ~/musicspot

# Transfer files (use WinSCP, FileZilla, or scp command)
# Or clone from git if you're using version control
```

### Step 2: Install Node.js (if not installed)

```bash
# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Verify installation
node --version
npm --version
```

### Step 3: Install Project Dependencies

```bash
cd ~/musicspot

# Install all dependencies from package.json
npm install
```

### Step 4: Create .env File

```bash
# Create .env file
nano .env

# Add these lines (paste your MongoDB connection string):
MONGODB_URI=mongodb+srv://your-connection-string-here
SESSION_SECRET=music-spot-secret-key-2025
PORT=3000

# Save: Ctrl+O, Enter, Ctrl+X
```

### Step 5: Run the Application

```bash
# Start the server
npm start
```

You should see:
```
MongoDB connected successfully
Music Spot server running on port 3000
Visit: http://localhost:3000
```

### Step 6: Access from Browser

- **On Ubuntu:** Open browser and go to `http://localhost:3000`
- **From another computer:** Go to `http://YOUR_UBUNTU_IP:3000`
  - Find Ubuntu IP: `ip addr show` or `hostname -I`

---

## Deploying to Cloud Server (Production)

### Recommended Platforms:

1. **Render.com** (Free tier available)
2. **Railway.app** (Free tier with GitHub)
3. **Heroku** (Paid now)
4. **AWS/Azure** (More complex)

### Quick Deploy to Render:

1. Go to https://render.com and sign up
2. Click "New +" → "Web Service"
3. Connect your GitHub repository (or upload project)
4. Settings:
   - **Name:** music-spot
   - **Environment:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
5. Add Environment Variables:
   - `MONGODB_URI` = your MongoDB Atlas connection string
   - `SESSION_SECRET` = your secret key
6. Click "Create Web Service"
7. Wait 2-5 minutes for deployment
8. Your app URL: `https://music-spot.onrender.com` (or similar)

---

## Testing MongoDB Connection

Create a test file `test-mongo.js`:

```javascript
const mongoose = require('mongoose');
require('dotenv').config();

const mongoURI = process.env.MONGODB_URI;

mongoose.connect(mongoURI)
  .then(() => {
    console.log('✅ MongoDB connected successfully!');
    console.log('Database:', mongoose.connection.name);
    mongoose.connection.close();
  })
  .catch(err => {
    console.error('❌ MongoDB connection error:', err);
  });
```

Run test:
```bash
node test-mongo.js
```

---

## Important Notes

### Music File Storage (GridFS)

- Music files are stored in MongoDB using **GridFS**
- GridFS splits large files into chunks (255KB each)
- Files are stored in two collections:
  - `uploads.files` - file metadata
  - `uploads.chunks` - file data chunks
- Maximum file size: 10MB (you can increase this in code)
- Supported formats: mp3, wav, ogg, m4a

### Database Collections

Your MongoDB will have these collections:
- `users` - User accounts
- `songs` - Song metadata (title, artist, etc.)
- `playlists` - User playlists
- `posts` - Forum posts
- `sessions` - User sessions
- `uploads.files` - Music file metadata (GridFS)
- `uploads.chunks` - Music file data (GridFS)

### Free Tier Limits (MongoDB Atlas)

- Storage: 512MB
- RAM: Shared
- No credit card required
- Should be enough for 100-200 songs (depending on file sizes)

---

## Troubleshooting

### Problem: "MongooseServerSelectionError"
**Solution:** Check your connection string and IP whitelist in MongoDB Atlas

### Problem: "Authentication failed"
**Solution:** Double-check your username and password in connection string

### Problem: "Cannot connect to localhost"
**Solution:** Make sure MongoDB service is running: `sudo systemctl start mongod`

### Problem: "File upload fails"
**Solution:** Make sure MongoDB connection is established before uploading

### Problem: "Audio won't play"
**Solution:** Check browser console for errors. Make sure file was uploaded successfully.

---

## Quick Start Checklist

- [ ] Create MongoDB Atlas account (or install local MongoDB)
- [ ] Create database cluster
- [ ] Create database user
- [ ] Whitelist IP address (0.0.0.0/0 for testing)
- [ ] Get connection string
- [ ] Create `.env` file with MongoDB URI
- [ ] Install Node.js on Ubuntu
- [ ] Transfer project files to Ubuntu
- [ ] Run `npm install`
- [ ] Run `npm start`
- [ ] Test in browser
- [ ] Register a user account
- [ ] Upload a test song
- [ ] Verify music plays from database

---

## Need Help?

Common MongoDB Atlas regions for Hong Kong students:
- AWS ap-east-1 (Hong Kong)
- AWS ap-southeast-1 (Singapore)
- GCP asia-east2 (Hong Kong)

Your MongoDB connection string should look like:
```
mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/musicspot?retryWrites=true&w=majority
```

Remember to replace:
- `username` - your database username
- `password` - your database password
- `cluster0.xxxxx` - your actual cluster address
- `musicspot` - your database name

Good luck with your project! 🎵

