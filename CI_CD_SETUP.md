# CI/CD Setup Instructions

This project has a GitHub Actions CI/CD pipeline that automatically builds the Android APK when code is merged to the `master` branch and sends notifications to Microsoft Teams.

## Features

- ✅ Automatic build on merge to master branch
- ✅ APK versioning based on `pubspec.yaml`
- ✅ Automated testing before build
- ✅ APK artifacts stored in GitHub Actions
- ✅ Microsoft Teams notifications with build details
- ✅ Success and failure notifications

## Setup Steps

### 1. Create Microsoft Teams Incoming Webhook

1. Open your Microsoft Teams channel where you want to receive notifications
2. Click on the three dots (•••) next to the channel name
3. Select **Connectors** or **Workflows**
4. Search for **Incoming Webhook**
5. Click **Add** or **Configure**
6. Give it a name (e.g., "Git Action Builds")
7. Optionally upload an image
8. Click **Create**
9. **Copy the webhook URL** (you'll need this in the next step)

### 2. Add GitHub Secret

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `TEAMS_WEBHOOK_URL`
5. Value: Paste the webhook URL you copied from Teams
6. Click **Add secret**

### 3. Configure Your App Version

Make sure your `pubspec.yaml` has a proper version number:

```yaml
version: 1.0.0+1
```

Format: `version_name+build_number`

### 4. Test the Workflow

1. Make a change to your code
2. Commit and push to a branch:
   ```bash
   git checkout -b test-branch
   git add .
   git commit -m "Test CI/CD pipeline"
   git push origin test-branch
   ```
3. Create a pull request to `master`
4. Merge the pull request
5. The workflow will automatically run and send a notification to Teams

## Workflow Triggers

The workflow runs when:
- Code is directly pushed to `master` branch
- A pull request is merged into `master` branch

## What Gets Sent to Teams

### On Success ✅
- App version and build number
- APK file size
- Commit SHA and author
- Commit message
- Links to download APK and view commit

### On Failure ❌
- Failure notification
- Commit author and message
- Link to view build logs

## Accessing Built APKs

1. Go to your GitHub repository
2. Click on **Actions** tab
3. Click on the workflow run
4. Scroll down to **Artifacts** section
5. Download the APK file

## Customization

### Change Build Trigger
Edit [.github/workflows/build-and-deploy.yml](.github/workflows/build-and-deploy.yml):

```yaml
on:
  push:
    branches:
      - master  # Change this to your preferred branch
```

### Change Flutter Version
Edit the workflow file:

```yaml
- name: Set up Flutter
  uses: subosito/flutter-action@v2
  with:
    flutter-version: '3.24.0'  # Change to your Flutter version
    channel: 'stable'
```

### Add Build Variants
To build different variants (debug, profile, release):

```yaml
- name: Build APK
  run: flutter build apk --release --split-per-abi  # Builds separate APKs for each architecture
```

## Troubleshooting

### Build Fails
- Check the Actions tab for detailed logs
- Ensure all dependencies in `pubspec.yaml` are valid
- Verify Flutter version compatibility

### Teams Notification Not Received
- Verify the webhook URL is correct in GitHub Secrets
- Check if the webhook is still active in Teams
- Review the workflow logs for curl errors

### APK Not Generated
- Check if the build step completed successfully
- Verify Java and Flutter versions are compatible
- Look for errors in the build logs

## Security Notes

- Never commit the Teams webhook URL directly to your code
- Always use GitHub Secrets for sensitive information
- Regularly rotate webhook URLs if compromised
- Limit repository access to trusted contributors

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Flutter Build Documentation](https://docs.flutter.dev/deployment/android)
- [Microsoft Teams Webhooks](https://docs.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/add-incoming-webhook)
