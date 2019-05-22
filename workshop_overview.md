# Making GitHub Actions actionable workshop overview

In this workshop, we’ll use GitHub Actions to allow you to notify team members in Slack when a GitHub issue requires their attention.

First, we’ll create a new Action that will parse your issues and comments for a special "/cc Slack" string. Then, we’ll combine the Action we created with an existing Action from the GitHub Marketplace that posts messages to Slack. Finally, we’ll verify that our workflow works as intended by creating some test issues and comments.

## Create your Action

1. Create a new public repository for your GitHub Action.
1. Create a [`Dockerfile`](https://github.com/krider2010/cc-slack-action/blob/master/Dockerfile) at the root of the repository. Give it access to JavaScript using the `node:slim` as the base image. Set the `ENTRYPOINT` to `/index.js`.
1. Create a [`package.json`](https://github.com/krider2010/cc-slack-action/blob/master/package.json) at the root of the repository. Include the dependencies your Action will use.
1. Create a [`index.js`](https://github.com/krider2010/cc-slack-action/blob/master/index.js) at the root of the repository. `require` the `actions-toolkit` library to get access to some great helpers for writing an Action in JavaScript. Specify which events you want the Action to respond to, and implement the logic to identify "/cc Slack" in issue bodies and comments and persist message data to the shared workspace.

## Create your workflow

1. Create a new [Slack App](https://api.slack.com/apps?new_app=1).
1. Find or create a new repository from which you’d like to CC team members in Slack.
1. Click the "Actions" tab and then the "Create a new workflow" button to create a new `.github/main.workflow` file.
1. Create a new workflow triggered by the `issues` event. Add the Action you created above as the first Action in the new workflow. Add the `slack-bot-action` as the second action. Using the information from your Slack App, create `CONVERSATION_ID` and `SLACK_BOT_TOKEN` secrets and expose them to the `slack-bot-action`.
1. Create another workflow triggered by the `issue_comment` event and repeat the previous step.

## Test your workflow

1. Create a new issue with "/cc Slack" in the body. Click the Actions tab to monitor your workflow and view Action logs. Verify that you receive a message in Slack referencing the new issue.
2. Comment on the issue with "/cc Slack" in the body. Verify that you receive a message in Slack referencing the new issue.
3. Comment on the issue without "/cc Slack" in the body. Verify that you don’t receive any new messages in Slack.
