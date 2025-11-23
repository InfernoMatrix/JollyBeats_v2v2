# Music Spot - Quick Start Guide

## What I've Built for You ✅

A complete **Music Spot** website with:

✅ **User Authentication** - Register, Login, Logout with session management  
✅ **Music Upload & Storage in MongoDB** - Music files stored in database using GridFS  
✅ **Song Management** - Upload, view, edit, delete songs with search/filter  
✅ **Playlist System** - Create playlists, add/remove songs  
✅ **Forum/Discussion** - Post about music, recommend songs  
✅ **RESTful APIs** - Full CRUD API endpoints (no auth required)  
✅ **Form Validation** - All inputs validated  
✅ **Beautiful UI** - Modern, responsive design  

## 📁 Project Structure

```
serversidev2/
├── server.js                 # Main server file
├── package.json              # Dependencies
├── .env.example              # Environment variables template
├── README.md                 # Full documentation
├── MONGODB_SETUP.md          # MongoDB setup instructions
├── QUICKSTART.md             # This file
│
├── models/                   # Database schemas
│   ├── User.js
│   ├── Song.js
│   ├── Playlist.js
│   └── Post.js
│
├── routes/                   # Route handlers
│   ├── authRoutes.js         # Login/Register/Logout
│   ├── songRoutes.js         # Song CRUD + file upload
│   ├── playlistRoutes.js     # Playlist CRUD
│   ├── forumRoutes.js        # Forum posts
│   └── apiRoutes.js          # RESTful APIs
│
├── views/                    # EJS templates
│   ├── index.ejs             # Home page
│   ├── register.ejs          # Registration form
│   ├── login.ejs             # Login form
│   ├── songs.ejs             # Browse songs
│   ├── uploadSong.ejs        # Upload song form
│   ├── editSong.ejs          # Edit song form
│   ├── playlists.ejs         # View playlists
│   ├── createPlaylist.ejs    # Create playlist form
│   ├── playlistDetail.ejs    # Playlist details
│   └── forum.ejs             # Forum/discussion
│
├── middleware/
│   └── auth.js               # Authentication middleware
│
└── public/
    └── css/
        └── style.css         # Stylesheet
```

## 🚀 How to Run (3 Simple Steps)

### Step 1: Setup MongoDB

**For Production (Recommended):**
1. Go to https://www.mongodb.com/cloud/atlas
2. Create FREE account
3. Create a cluster (FREE tier)
4. Get your connection string
5. See `MONGODB_SETUP.md` for detailed instructions

**For Local Testing:**
```bash
# Install MongoDB on Ubuntu
sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
```

### Step 2: Install & Configure

```bash
# On your Ubuntu server
cd ~/serversidev2

# Install Node.js (if needed)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install dependencies
npm install

# Create .env file
nano .env
```

Add to `.env`:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/musicspot
SESSION_SECRET=music-spot-random-secret-2025
PORT=3000
```

### Step 3: Run!

```bash
npm start
```

Visit: `http://localhost:3000` or `http://YOUR_SERVER_IP:3000`

## 🎵 Key Features

### 1. Music Storage in MongoDB (GridFS)
- **Music files are stored IN the database**
- Works across different servers and devices
- No need for local file storage
- Users can access music from anywhere

### 2. User Features
- Register with username, email, password
- Login/Logout with sessions
- Only edit/delete your own content

### 3. Song Features
- Upload: mp3, wav, ogg, m4a (max 10MB)
- Search by title, artist, album
- Filter by genre, artist
- Play songs with HTML5 audio player
- Edit/Delete your own songs

### 4. Playlist Features
- Create custom playlists
- Add/remove songs
- View playlist details
- Delete playlists

### 5. Forum Features
- Post about music (max 500 characters)
- Link posts to songs
- Delete your own posts

### 6. RESTful APIs
All return JSON (no login required):
- `GET /api/songs` - Get all songs
- `GET /api/songs/:id` - Get one song
- `POST /api/songs` - Create song
- `PUT /api/songs/:id` - Update song
- `DELETE /api/songs/:id` - Delete song
- `GET /api/playlists` - Get all playlists
- `GET /api/posts` - Get all posts

## 📝 Testing Your Project

### 1. Register a User
1. Go to home page
2. Click "Register"
3. Fill in: username (3+ chars), email, password (6+ chars)
4. Click Register

### 2. Login
1. Enter your username and password
2. Click Login

### 3. Upload a Song
1. Go to "Songs" page
2. Click "Upload New Song"
3. Fill in song details (title & artist required)
4. Select a music file
5. Click "Upload Song"
6. **The music file is now stored in MongoDB!**

### 4. Create a Playlist
1. Go to "Playlists" page
2. Click "Create New Playlist"
3. Enter name and description
4. Click "Create Playlist"
5. View details and add songs

### 5. Post in Forum
1. Go to "Forum" page
2. Write your message
3. Optionally select a song
4. Click "Post"

### 6. Test APIs (from terminal)
```bash
# Get all songs
curl http://localhost:3000/api/songs

# Get one song
curl http://localhost:3000/api/songs/SONG_ID

# Create song (replace USER_ID)
curl -X POST http://localhost:3000/api/songs \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Song","artist":"Test Artist","filename":"test.mp3","uploadedBy":"USER_ID"}'
```

## 🌐 Deploy to Production

### Option 1: Render.com (Free)
1. Create account at https://render.com
2. New → Web Service
3. Connect GitHub or upload project
4. Build: `npm install`
5. Start: `npm start`
6. Add environment variables (MONGODB_URI, SESSION_SECRET)
7. Deploy!

### Option 2: Your Own Ubuntu Server
1. Get a server (DigitalOcean, AWS, etc.)
2. Install Node.js and MongoDB
3. Transfer project files
4. Run `npm install`
5. Run `npm start`
6. Use PM2 to keep it running:
```bash
npm install -g pm2
pm2 start server.js --name musicspot
pm2 startup
pm2 save
```

## ⚠️ Important Notes

### Music Files in MongoDB
- I've updated the code to use **MongoDB GridFS**
- Music files are stored directly in MongoDB
- This means your music will work on ANY server
- When you deploy, music moves with the database
- No need to transfer uploaded files separately

### Security
- Simple password storage (no bcrypt - as taught in lectures)
- Session-based authentication (as shown in Lecture 7)
- Input validation on all forms
- File type and size validation

### MongoDB Atlas Free Tier
- 512MB storage
- Should fit 100-200 songs depending on size
- No credit card required
- Perfect for student projects

## 📋 For Your Demo Presentation

### Prepare CURL Commands
```bash
# Save these in a text file for quick copy-paste

# 1. Get all songs
curl -X GET https://your-app.onrender.com/api/songs

# 2. Get one song (replace SONG_ID after you upload)
curl -X GET https://your-app.onrender.com/api/songs/SONG_ID

# 3. Update song
curl -X PUT https://your-app.onrender.com/api/songs/SONG_ID \
  -H "Content-Type: application/json" \
  -d '{"title":"Updated Title","artist":"Updated Artist"}'

# 4. Delete song
curl -X DELETE https://your-app.onrender.com/api/songs/SONG_ID

# 5. Get playlists
curl -X GET https://your-app.onrender.com/api/playlists

# 6. Get forum posts
curl -X GET https://your-app.onrender.com/api/posts
```

## 🎓 Technologies Used (Matches Your Course)

- ✅ Node.js + Express.js (Lecture 5-6)
- ✅ MongoDB with Mongoose (Lecture 3-6)
- ✅ EJS templates (Lecture 7)
- ✅ Session authentication with express-session (Lecture 7)
- ✅ RESTful API routes (Lecture 8)
- ✅ File upload with Multer
- ✅ MongoDB GridFS for file storage
- ✅ MVC structure (models, views, routes)

## 📞 Troubleshooting

**MongoDB connection error?**
- Check your connection string in `.env`
- Make sure IP is whitelisted in MongoDB Atlas
- See `MONGODB_SETUP.md`

**Can't upload songs?**
- Make sure MongoDB is connected
- Check file size (max 10MB)
- Check file type (mp3, wav, ogg, m4a only)

**Audio won't play?**
- Check browser console for errors
- Make sure you're logged in
- Try a different audio file

**Port 3000 already in use?**
- Change PORT in `.env` file
- Or kill existing process: `lsof -ti:3000 | xargs kill`

## ✨ What Makes This Project Special

1. **Music in Database** - Files stored in MongoDB, not filesystem
2. **Search & Filter** - Advanced query functionality
3. **Playlist System** - Users can organize their music
4. **Forum Feature** - Social interaction (like Twitter/Threads)
5. **Complete Validation** - All inputs validated
6. **RESTful APIs** - Full CRUD without authentication
7. **Clean Code** - Simple, readable, matches lecture style
8. **Responsive Design** - Works on mobile and desktop

## 📚 Next Steps

1. Read `MONGODB_SETUP.md` for MongoDB instructions
2. Set up MongoDB Atlas account
3. Install dependencies with `npm install`
4. Create `.env` file with your MongoDB URI
5. Run `npm start`
6. Register a user and upload songs
7. Deploy to Render or your Ubuntu server
8. Prepare your demo presentation

Good luck with your project! 🎵🚀

