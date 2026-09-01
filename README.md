# TickTick MCP Server

MCP Server for the TickTick API, enabling task management, project organization, habit tracking, and more.

### Features

- ✅ **Task Management**: Create, read, update, and delete tasks with all available properties
- 📊 **Project Management**: Create, read, update, and delete projects with customizable views
- 📋 **Subtask Support**: Full support for managing subtasks within parent tasks
- 🔄 **Complete Task Control**: Set priorities, due dates, reminders, and recurring rules
- 🔐 **OAuth Authentication**: Full OAuth2 implementation for secure API access
- ⚠️ **Comprehensive Error Handling**: Clear error messages for common issues

## Tools

1. `get_task_by_ids`

   - Get a specific task by project ID and task ID
   - Inputs:
     - `projectId` (string): Project identifier
     - `taskId` (string): Task identifier
   - Returns: Task object matching `TickTickTaskSchema`

2. `create_task`

   - Create a new task in a project
   - Inputs:
     - `title` (string): Task title
     - `projectId` (string): Project id
     - `content` (optional string): Task content
     - `desc` (optional string): Task description
     - `isAllDay` (optional boolean): Is all day task
     - `startDate` (optional string): Task start date in "yyyy-MM-dd'T'HH:mm:ssZ" format
     - `dueDate` (optional string): Task due date in "yyyy-MM-dd'T'HH:mm:ssZ" format
     - `timeZone` (optional string): Task time zone (e.g., "America/Los_Angeles")
     - `reminders` (optional string[]): List of reminder triggers in iCalendar format
     - `repeatFlag` (optional string): Task repeat flag in iCalendar format
     - `priority` (optional number): Task priority (None: 0, Low: 1, Medium: 3, High: 5)
     - `sortOrder` (optional string): Task sort order
     - `items` (optional array): List of subtasks with:
       - `title` (string): Subtask item title
       - `startDate` (optional string): Subtask date in "yyyy-MM-dd'T'HH:mm:ssZ" format
       - `isAllDay` (optional boolean): Is all day subtask item
       - `sortOrder` (optional number): Subtask item sort order
       - `timeZone` (optional string): Subtask timezone
       - `status` (optional number): Completion status (Normal: 0, Completed: 1)
       - `completedTime` (optional string): Completion time in "yyyy-MM-dd'T'HH:mm:ssZ" format
   - Returns: Created task object matching `TickTickTaskSchema`

3. `update_task`

   - Update an existing task
   - Inputs:
     - `taskId` (string): Task identifier - Path
     - `id` (string): Task identifier - Body
     - `projectId` (string): Project id
     - All optional fields from `create_task`
   - Returns: Updated task object matching `TickTickTaskSchema`

4. `complete_task`

   - Mark a task as completed
   - Inputs:
     - `taskId` (string): Task identifier
     - `projectId` (string): Project identifier
   - Returns: void

5. `delete_task`

   - Delete a task from a project
   - Inputs:
     - `taskId` (string): Task identifier
     - `projectId` (string): Project identifier
   - Returns: void

6. `get_user_projects`

   - Get all projects for the authenticated user
   - Inputs: None
   - Returns: Array of project objects matching `TickTickProjectSchema`

7. `get_project_by_id`

   - Get a specific project by ID
   - Inputs:
     - `projectId` (string): Project identifier
   - Returns: Project object matching `TickTickProjectSchema`

8. `get_project_with_data`

   - Get project details along with tasks and columns
   - Inputs:
     - `projectId` (string): Project identifier
   - Returns: Object containing:
     - `project`: Project object matching `TickTickProjectSchema`
     - `tasks`: Array of task objects matching `TickTickTaskSchema`
     - `columns`: Optional array of column objects with:
       - `id` (optional string)
       - `projectId` (optional string)
       - `name` (optional string)
       - `sortOrder` (optional number)

9. `create_project`

   - Create a new project
   - Inputs:
     - `name` (string): Project name
     - `color` (optional string): Project color (default: '#4772FA')
     - `viewMode` (optional string): View mode ('list', 'kanban', 'timeline') (default: 'list')
     - `kind` (optional string): Project kind ('TASK', 'NOTE') (default: 'TASK')
   - Returns: Created project object matching `TickTickProjectSchema`

10. `update_project`

    - Update an existing project
    - Inputs:
      - `projectId` (string): Project identifier
      - `name` (optional string): Project name
      - `color` (optional string): Project color
      - `sortOrder` (optional number): Project sort order
      - `viewMode` (optional string): View mode ('list', 'kanban', 'timeline')
      - `kind` (optional string): Project kind ('TASK', 'NOTE')
    - Returns: Updated project object matching `TickTickProjectSchema`

11. `delete_project`
    - Delete a project
    - Inputs:
      - `projectId` (string): Project identifier
    - Returns: void

## Schema References

- [`TickTickTaskSchema`](src/common/types.ts#L22): Defines the structure for task objects including:

  - Basic task properties (id, title, description)
  - Dates and time settings
  - Priority and status
  - Checklist items and subtasks

- [`TickTickProjectSchema`](src/common/types.ts#L3): Defines the structure for project objects including:
  - Project identification and naming
  - Display settings (color, view mode)
  - Permissions and organization

## Tasks Properties

When creating or updating tasks, you can include these properties:

- **Priority Levels**:
  - `0`: None
  - `1`: Low
  - `3`: Medium
  - `5`: High
- **Status Values**:
  - `0`: Normal (not completed)
  - `2`: Completed
- **Reminder Format**:
  - Example: `["TRIGGER:P0DT9H0M0S", "TRIGGER:PT0S"]`
  - Follows iCalendar TRIGGER format
- **Recurring Rules (repeatFlag)**:
  - Example: `"RRULE:FREQ=DAILY;INTERVAL=1"`
  - Uses RFC 5545 recurrence rules
- **Date Format**:
  - ISO 8601 format: `"yyyy-MM-dd'T'HH:mm:ssZ"`
  - Example: `"2019-11-13T03:00:00+0000"`

## Projects Properties

When creating or updating projects, you can use these properties:

- **View Modes**:
  - `"list"`: Standard list view
  - `"kanban"`: Kanban board view
  - `"timeline"`: Timeline view
- **Project Kinds**:
  - `"TASK"`: Task-oriented project
  - `"NOTE"`: Note-oriented project

## Setup

### OAuth Authentication

To enable OAuth authentication with TickTick, you'll need to register your app and obtain API credentials:

- Create an account on the [TickTick Developer Portal](https://developer.ticktick.com/manage)
- Register a new application
- Set the OAuth redirect URL to: http://localhost:8000/callback
- Copy the generated Client ID (TICKTICK_CLIENT_ID) and Client Secret (TICKTICK_CLIENT_SECRET)

### First-Time Authorization Flow

When using the TickTick MCP server for the first time:

1. You'll be prompted to authorize the application
2. A browser window will open with the TickTick login page
3. After login, you'll be asked to grant permissions
4. The access token will be displayed in the page
5. Copy this token and set it as the TICKTICK_ACCESS_TOKEN environment variable

### Generate Access Token

When you need to generate a new access token (either for first-time setup or when the token expires), follow these steps:

1. Configure your credentials using one of these methods:

   #### Option 1: .env file (Recommended)

   Create a `.env` file in your project root:

   ```bash
   TICKTICK_CLIENT_ID="<YOUR_CLIENT_ID>"
   TICKTICK_CLIENT_SECRET="<YOUR_CLIENT_SECRET>"
   ```

   Then load it:

   ```bash
   source .env
   ```

   This method is recommended because:

   - Credentials persist between terminal sessions
   - Easier to manage multiple configurations
   - Less prone to shell history leaks
   - Can be easily backed up (remember to exclude from version control)

   #### Option 2: Terminal Environment Variables

   Use single quotes if your credentials contain special characters. Note that these variables will only persist in your current terminal session:

   ```bash
   export TICKTICK_CLIENT_ID='<YOUR_CLIENT_ID>'
   export TICKTICK_CLIENT_SECRET='<YOUR_CLIENT_SECRET>'
   ```

2. Run the authentication command:

   If using the published package:

   ```bash
   npx @alexarevalo.ai/mcp-server-ticktick ticktick-auth
   ```

   If running the MCP server locally:

   ```bash
   npm run start:auth
   ```

   The process will:

   - Launch your default browser
   - Direct you to TickTick's login page
   - Request necessary permissions
   - Generate and display your access token

3. Save the access token:
   ```bash
   echo "TICKTICK_ACCESS_TOKEN=\"<GENERATED_TOKEN>\"" >> .env
   source .env
   ```

> **Security Tips**:
>
> - Add `.env` to your `.gitignore` file
> - Never commit credentials to version control
> - Access tokens expire after 180 days - you'll need to regenerate them

### Usage with Claude Desktop

To use this with Claude Desktop, add the following to your `claude_desktop_config.json`:

### NPX

```json
{
  "mcpServers": {
    "ticktick": {
      "command": "npx",
      "args": ["-y", "@alexarevalo.ai/mcp-server-ticktick"],
      "env": {
        "TICKTICK_CLIENT_ID": "<YOUR_CLIENT_ID>",
        "TICKTICK_CLIENT_SECRET": "<YOUR_CLIENT_SECRET>",
        "TICKTICK_ACCESS_TOKEN": "<YOUR_ACCESS_TOKEN>"
      }
    }
  }
}
```

#### Docker

```json
{
  "mcpServers": {
    "ticktick": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "TICKTICK_CLIENT_ID",
        "-e",
        "TICKTICK_CLIENT_SECRET",
        "-e",
        "TICKTICK_ACCESS_TOKEN",
        "mcp/ticktick"
      ],
      "env": {
        "TICKTICK_CLIENT_ID": "<YOUR_CLIENT_ID>",
        "TICKTICK_CLIENT_SECRET": "<YOUR_CLIENT_SECRET>",
        "TICKTICK_ACCESS_TOKEN": "<YOUR_ACCESS_TOKEN>"
      }
    }
  }
}
```
### Installing via Smithery

To install ticktick-mcp-server for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@alexarevalo9/ticktick-mcp-server):

```bash
npx -y @smithery/cli install @alexarevalo9/ticktick-mcp-server --client claude
```

### Build

Docker build:

```bash
docker build -t mcp/ticktick -f src/ticktick/Dockerfile .
```

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
