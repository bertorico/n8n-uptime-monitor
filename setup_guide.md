# Setup Guide — Website Uptime Monitor

## Step 1: Create a Telegram Bot

1. Open Telegram and search for `@BotFather`
2. Send `/newbot` and follow the prompts to create a bot
3. Copy the **bot token** (looks like `123456789:ABCdefGhIJKlmNoPQRsTUVwxyz`)
4. Find your Chat ID by messaging `@userinfobot` on Telegram

## Step 2: Create a Google Sheet

1. Create a new Google Sheet
2. Name the first sheet "Foglio1" (or update the workflow node accordingly)
3. Add these column headers in row 1:

| A | B | C | D | E |
|---|---|---|---|---|
| timestamp | url | status_code | response_time_ms | is_up |

4. Note the Sheet ID from the URL:
   `https://docs.google.com/spreadsheets/d/THIS_IS_THE_SHEET_ID/edit`

## Step 3: Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Select `workflows/website-uptime-monitor.json`

## Step 4: Configure Credentials

### Google Sheets
1. In n8n, go to **Credentials → Add Credential → Google Sheets OAuth2 API**
2. Follow the OAuth2 flow to connect your Google account
3. Select this credential in the "Uptime Monitor Log" node

### Telegram
1. In n8n, go to **Credentials → Add Credential → Telegram API**
2. Enter the bot token from Step 1
3. Select this credential in all three Telegram nodes

## Step 5: Update Workflow Values

Open the workflow and update these values:

### HTTP Request nodes (both "HTTP Request" and "HTTP Request1")
- **URL**: Replace with your target website URL

### Telegram nodes (all three: alert, confirmed down, recovered)
- **Chat ID**: Replace with your Telegram Chat ID from Step 1

### Uptime Monitor Log node
- **Document ID**: Select your Google Sheet from the dropdown, or enter the Sheet ID

## Step 6: Test

1. Click **Execute Workflow** to run a manual test
2. Check that a row appears in your Google Sheet
3. To test alerts: temporarily change the URL to a non-existent domain
4. Verify Telegram alerts arrive correctly

## Step 7: Activate

1. Toggle the workflow to **Active** (top-right switch in n8n)
2. The monitor will now run automatically every 5 minutes

## Troubleshooting

| Problem | Solution |
|---|---|
| No alerts received | Check the Telegram Chat ID and bot token |
| Google Sheets not updating | Re-authenticate the Google Sheets credential |
| Workflow stops on error | Verify "On Error: Continue" is set in both HTTP Request nodes |
| False positives | Increase the timeout value (default 10,000ms) |
| Too many alerts | Increase the check interval in the Schedule Trigger |