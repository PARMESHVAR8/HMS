#!/bin/bash

echo "🔧 Creating HMS Backend Project Structure..."

# Create main project folder
mkdir hms-backend
cd hms-backend

# Create folders
mkdir config models routes controllers

# Create files
touch server.js
touch package.json
touch README.md
touch .env
touch config/db.js

# Initialize package.json
npm init -y

# Install core dependencies
npm install express mongoose dotenv

# Install dev dependencies
npm install --save-dev nodemon

# Write basic README content
echo "# HMS Backend Project" > README.md

# Write basic server.js content
cat <<EOL > server.js
const express = require('express');
const dotenv = require('dotenv');
const connectDB = require('./config/db');

dotenv.config();
connectDB();

const app = express();
app.use(express.json());
app.get('/', (req, res) => {
  res.send('🏥 HMS API is running...');
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(\`🚀 Server running on port \${PORT}\`);
});
EOL

# Write basic db.js content
cat <<EOL > config/db.js
const mongoose = require('mongoose');
const dotenv = require('dotenv');

dotenv.config();

const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGO_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log(\`✅ MongoDB Connected: \${conn.connection.host}\`);
  } catch (error) {
    console.error(\`❌ Error: \${error.message}\`);
    process.exit(1);
  }
};

module.exports = connectDB;
EOL

# Add .env placeholder
echo -e "PORT=5000\nMONGO_URI=your-mongodb-uri-here" > .env

echo "✅ HMS Backend Setup Completed!"
