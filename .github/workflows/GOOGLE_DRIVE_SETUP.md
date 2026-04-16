# Google Drive Upload Setup Guide

## 📋 Prerequisites
- Google Account
- Access to the target Google Drive folder

## 🔧 Setup Steps

### Step 1: Install rclone on your local machine

**macOS:**
```bash
brew install rclone
```

**Linux:**
```bash
curl https://rclone.org/install.sh | sudo bash
```

**Windows:**
Download from https://rclone.org/downloads/

### Step 2: Configure rclone with Google Drive

1. Run the configuration wizard:
```bash
rclone config
```

2. Follow these prompts:
   - Type `n` for "New remote"
   - Name: `gdrive` (must be "gdrive")
   - Storage: Type the number for "Google Drive" (usually `15`)
   - Client ID: Press Enter (leave blank)
   - Client Secret: Press Enter (leave blank)
   - Scope: Type `1` for "Full access"
   - Root folder ID: Press Enter (leave blank)
   - Service Account File: Press Enter (leave blank)
   - Advanced config: Type `n`
   - Auto config: Type `y` (or `n` if on a remote server)
   - A browser window will open - **Sign in with your Google account** and allow access
   - Choose "This account is a team drive": Type `n`
   - Type `y` to confirm
   - Type `q` to quit config

3. Verify the configuration:
```bash
rclone listremotes
```
You should see `gdrive:` in the list.

### Step 3: Test the upload

Test if you can upload to your folder:
```bash
echo "test" > test.txt
rclone copy test.txt gdrive:1hhGyEjYhj5-P0gEP26YsVBCvr1nEsavu
```

Check your Google Drive folder to verify the test file was uploaded.

### Step 4: Get rclone configuration

Get your rclone config file content:
```bash
cat ~/.config/rclone/rclone.conf
```

Copy the **entire output**. It should look like:
```
[gdrive]
type = drive
scope = drive
token = {"access_token":"ya29...","token_type":"Bearer",...}
team_drive = 
```

### Step 5: Add to GitHub Secrets

1. Go to your GitHub repository
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `RCLONE_CONFIG`
5. Value: Paste the entire content from Step 4
6. Click **Add secret**

### Step 6: Test the workflow

Push a commit to master branch:
```bash
git add .
git commit -m "Test Google Drive upload"
git push origin master
```

Check the Actions tab to see if the APK is uploaded to Google Drive.

## 📂 Target Folder

Your APKs will be uploaded to:
https://drive.google.com/drive/folders/1hhGyEjYhj5-P0gEP26YsVBCvr1nEsavu

## 🔒 Security Notes

- The rclone config contains OAuth tokens - keep it secret!
- Tokens expire and refresh automatically
- Never commit the rclone.conf file to your repository
- Use GitHub Secrets to store sensitive data

## 🎯 File Naming

APK files are uploaded with this format:
```
app-release-{commit-sha}-{timestamp}.apk
```

Example: `app-release-abc123-20260416_143025.apk`

## 🐛 Troubleshooting

**"RCLONE_CONFIG secret is not set"**
- Make sure you created the secret with exact name: `RCLONE_CONFIG`

**"Failed to upload"**
- Verify the folder ID is correct
- Check if the Google account has write access to the folder
- Re-run the rclone config and update the secret

**Token expired**
- Re-run `rclone config` to refresh the token
- Update the `RCLONE_CONFIG` secret with new content

## ✅ Verification

After successful upload, you should see in the workflow logs:
```
✅ APK uploaded to Google Drive successfully!
```

And the Teams notification will show:
```
Status: ✅ Uploaded to Google Drive
```
