# Google Forms MCP Server

This MCP server uses the Google Forms API to provide functions such as creating and editing forms, and retrieving responses.

## Build Instructions

### Initial Setup
After cloning the repository, install dependencies:
```bash
cd google-forms-server
npm install
```

### Build the Server
```bash
# Build the main MCP server
npm run build
```

### Build the Refresh Token Script
```bash
# Build the refresh token acquisition script
npm run build:token
```

### Running in Development Environment
```bash
# Run the server directly
node build/index.js

# Or, use the npm script
npm run start
```


## Setup Instructions

1.  Create a project in the Google Cloud Console and enable the Google Forms API.
    *   https://console.cloud.google.com/
    *   Search for "Google Forms API" in API & Services > Library and enable it.

2.  Obtain an OAuth 2.0 Client ID and Secret.
    *   API & Services > Credentials > Create Credentials > OAuth client ID
    *   Application type: Select "Desktop app"

3.  Set environment variables and obtain a refresh token.
    ```bash
    export GOOGLE_CLIENT_ID="YOUR_CLIENT_ID"
    export GOOGLE_CLIENT_SECRET="YOUR_CLIENT_SECRET"
    cd google-forms-server
    npm run build
    node build/get-refresh-token.js
    ```

    Note: If you encounter an error when running `get-refresh-token.js`, execute the following commands:
    ```bash
    cd google-forms-server
    npm run build:token
    node build/get-refresh-token.js
    ```

4.  Copy the displayed refresh token.

5.  Update the Claude desktop app's configuration file.
    *   Open `~/Library/Application Support/Claude/claude_desktop_config.json`.
    *   Add environment variables to the `google-forms-server` in the `mcpServers` section:
    ```json
    "google-forms-server": {
      "command": "node",
      "args": [
        "/path/to/your/mcp-google-form/google-forms-server/build/index.js" // Update this path
      ],
      "env": {
        "GOOGLE_CLIENT_ID": "YOUR_CLIENT_ID",
        "GOOGLE_CLIENT_SECRET": "YOUR_CLIENT_SECRET",
        "GOOGLE_REFRESH_TOKEN": "YOUR_REFRESH_TOKEN"
      }
    }
    ```
    *Note: Ensure the path in `args` points to the correct location of `index.js` on your system.*

6.  Restart the Claude desktop app.

## Available Tools

This MCP server provides the following tools:

1.  `create_form` - Creates a new Google Form
2.  `add_text_question` - Adds a text question to the form
3.  `add_multiple_choice_question` - Adds a multiple-choice question to the form
4.  `get_form` - Retrieves the details of a form
5.  `get_form_responses` - Retrieves the responses for a form

## Usage Example

```
I want to guage my dev team's familiarity and comfort level integrating AI in their workflow.
Use your google forms tools to create a 2-4 minute multiple-choice survey!
```

Claude will use MCP tools like the following to create the form:

1.  Use the `create_form` tool to create a new form
2.  Use the `add_text_question` or `add_multiple_choice_question` tools to add questions
3.  Display the URL of the created form
