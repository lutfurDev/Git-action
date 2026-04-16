# Google Drive Upload Setup Guide (EASY METHOD)

## 🎯 Quick Setup (5 minutes)

### Step 1: Create Google Cloud Project

1. Go to https://console.cloud.google.com/
2. Click **Create Project** or select existing project
3. Name it anything (e.g., "GitHub Actions Upload")
4. Click **Create**

### Step 2: Enable Google Drive API

1. In your project, go to **APIs & Services** → **Library**
2. Search for "Google Drive API"
3. Click on it and click **Enable**

### Step 3: Create Service Account

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **Service Account**
3. Name: `github-actions-upload` (or anything)
4. Click **Create and Continue**
5. Role: Select **Basic** → **Editor** (or **Google Drive** → **Drive File Editor**)
6. Click **Continue** then **Done**

### Step 4: Create Service Account Key

1. Click on the service account you just created
2. Go to **Keys** tab
3. Click **Add Key** → **Create new key**
4. Select **JSON**
5. Click **Create** - a JSON file will download
6. **Keep this file safe!**

### Step 5: Share Google Drive Folder

1. Open your Google Drive folder: https://drive.google.com/drive/folders/1hhGyEjYhj5-P0gEP26YsVBCvr1nEsavu
2. Click **Share** button
3. Copy the **email address** from the JSON file (look for `client_email` field)
   - It looks like: `github-actions-upload@project-id.iam.gserviceaccount.com`
4. Paste this email in the Share dialog
5. Give it **Editor** permissions
6. Click **Share** (uncheck "Notify people" if prompted)

### Step 6: Add Credentials to GitHub

1. Open the downloaded JSON file
2. Copy **ALL the content** (the entire JSON)
3. Go to your GitHub repository
4. Click **Settings** → **Secrets and variables** → **Actions**
5. Click **New repository secret**
6. Name: `GDRIVE_CREDENTIALS_DATA`
7. Value: Paste the entire JSON content
8. Click **Add secret**

### Step 7: Test It!

Push a change to master branch:
```bash
git add .
git commit -m "Test Google Drive upload"
git push origin master
```

Check the Actions tab - the APK should upload to your Google Drive folder!

## ✅ Verification

1. Go to Actions tab on GitHub
2. Click on the latest workflow run
3. Check the "Upload APK to Google Drive" step
4. Should see: ✅ Success
5. Check your Google Drive folder - APK should be there!

## 📂 Your Google Drive Folder

https://drive.google.com/drive/folders/1hhGyEjYhj5-P0gEP26YsVBCvr1nEsavu

## 🐛 Troubleshooting

**"GDRIVE_CREDENTIALS_DATA secret is not set"**
- Make sure secret name is exactly: `GDRIVE_CREDENTIALS_DATA`
- Check that you pasted the entire JSON content

**"Permission denied" or "File not found"**
- Verify you shared the folder with the service account email
- Check the folder ID is correct in the workflow file

**"Invalid credentials"**
- Make sure you copied the entire JSON file content
- Check that the Google Drive API is enabled

## 🔒 Security Note

- Never commit the JSON credentials file to your repository
- Only store it in GitHub Secrets
- The service account only has access to folders you explicitly share with it
