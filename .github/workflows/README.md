# CI/CD Setup for Flutter APK Build

This workflow automatically builds your Flutter Android APK when code is merged to the master branch and sends notifications to Microsoft Teams.

## 🚀 Features

- ✅ Automatic APK build on master branch push/merge
- ✅ Runs tests before building
- ✅ Uploads APK as GitHub artifact
- ✅ Sends build notification to Microsoft Teams channel
- ✅ Provides build summary with APK details

## 📋 Setup Instructions

### 1. Get Microsoft Teams Webhook URL

1. Open your Microsoft Teams channel
2. Click on the three dots (...) next to the channel name
3. Select **Connectors** or **Workflows**
4. Search for **Incoming Webhook**
5. Click **Add** or **Configure**
6. Give it a name (e.g., "GitHub CI/CD")
7. Copy the webhook URL provided

### 2. Add Webhook URL to GitHub Secrets

1. Go to your GitHub repository
2. Click on **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `TEAMS_WEBHOOK_URL`
5. Value: Paste your Teams webhook URL
6. Click **Add secret**

### 3. Commit and Push

The workflow file is already created at `.github/workflows/build_and_deploy.yml`

```bash
git add .github/workflows/
git commit -m "Add CI/CD workflow for APK builds"
git push origin master
```

## 🔧 Workflow Triggers

The workflow runs when:
- Code is pushed directly to master branch
- A pull request is merged into master branch

## 📦 Build Artifacts

After each successful build:
- APK file is available in the **Actions** tab under **Artifacts**
- Download link is valid for 90 days
- Notification is sent to Teams with build details

## 📱 Teams Notification

The Teams notification includes:
- Repository name
- Branch name
- Commit SHA and author
- Commit message
- APK size and name
- Links to view the build and download APK

## 🛠 Customization

### Change Flutter Version

Edit line 25 in `build_and_deploy.yml`:
```yaml
flutter-version: '3.24.0'  # Change to your desired version
```

### Change Build Configuration

Edit line 36 in `build_and_deploy.yml`:
```yaml
flutter build apk --release  # Options: --debug, --profile, --release
```

### Add Signing Configuration

To sign your APK, add these secrets to GitHub:
- `KEYSTORE_BASE64` - Base64 encoded keystore file
- `KEY_ALIAS` - Key alias
- `KEY_PASSWORD` - Key password
- `STORE_PASSWORD` - Keystore password

Then add signing steps before the build step in the workflow.

## 🐛 Troubleshooting

**Teams notification not working?**
- Verify the webhook URL is correct
- Check if the secret `TEAMS_WEBHOOK_URL` is set in GitHub
- Ensure the webhook is active in Teams

**Build failing?**
- Check the Actions tab for detailed logs
- Ensure all dependencies are in `pubspec.yaml`
- Verify Flutter version compatibility

**APK not found?**
- Check the Actions tab → Select the workflow run → Artifacts section
- APK will be named `app-release`

## 📄 Workflow File Location

`.github/workflows/build_and_deploy.yml`
