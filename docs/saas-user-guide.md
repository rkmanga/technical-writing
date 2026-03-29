<section class="section" markdown="1">

<h2 class="section-title"><span class="icon-holder"><i class="fas fa-book-open"></i> SaaS Application User Guide</span></h2>

<div class="annotation-block" style="background: #11-0602; border: 0px solid #c3ddd5; border-radius: 4px; padding: 18px 22px; margin-bottom: 32px;">
  <h4 style="margin-top: 0; color: #02ffff; font-size: .75rem;">About This Sample</h4>
  <table style="width: 100%; font-size: 0.88rem; border-collapse: collapse;">
    <tr>
      <td style="width: 130px; font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Product</td>
      <td style="padding: 4px 0; color: #11-0602;">Notion — a real, widely used SaaS productivity and collaboration platform that combines notes, databases, project management, and wikis in a single workspace.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Audience</td>
      <td style="padding: 4px 0; color: #11-0602;">Business and operations users (non-technical) who are new to Notion and transitioning from spreadsheets or shared document folders for team tracking and record-keeping.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Tools Used</td>
      <td style="padding: 4px 0; color: #11-0602;">Framemaker (for PDF version), MkDocs (for online Help Center pages), Notion itself (for screen-by-screen validation).</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Challenge Solved</td>
      <td style="padding: 4px 0; color: #11-0602;">Notion is intentionally open-ended — it can be used in dozens of ways, which makes it difficult for new users to know where to start. The challenge was writing a task-oriented guide that moved users from setup to a working shared database without overwhelming them with every feature available on the interface.</td>
    </tr>
  </table>
</div>

---

## Getting Started with Notion: Build Your First Shared Workspace

This guide helps you set up a Notion workspace, create a shared database for your team, and configure views so everyone sees the information they need. You'll go from a blank workspace to a functional team tracker in about 30 minutes.

---

### What Is Notion?

Notion is a collaborative workspace where your team can store notes, manage projects, and organize data — all in one place. Think of it as a combination of a shared document editor, a spreadsheet, and a project board, with the flexibility to connect them together.

Before you begin, sign up at [notion.so](https://www.notion.so) and confirm your email address.

---

### Section 1: Set Up Your Workspace

When you first log in to Notion, you are placed in a personal workspace. For team use, you'll want to create a **Team Workspace**.

**To create a team workspace:**

1. Click your workspace name in the upper-left corner of the sidebar.
2. Select **Create or join a workspace** from the dropdown menu.
3. Choose **For my team**.
4. Enter your workspace name (e.g., your company or team name) and click **Continue**.
5. Select your team size and click **Continue**.
6. When prompted, choose **Start for free** or enter billing details for a paid plan.

Your new team workspace now appears in the sidebar. All pages and databases you create here are shared with members you invite.

---

### Section 2: Invite Your Team

**To invite teammates:**

1. Click **Settings** (gear icon) in the lower-left corner of the sidebar.
2. Select **Members** from the left menu.
3. Click **Add members**.
4. Enter the email addresses of the people you want to invite, separated by commas.
5. Choose a role: **Member** (can edit all content) or **Guest** (limited access to specific pages).
6. Click **Invite**.

Invitees receive an email with a link to join your workspace. They can access the workspace from any browser or the Notion desktop/mobile app.

---

### Section 3: Create a Database

Notion databases are the foundation for organizing structured information — like a project tracker, contact list, or content calendar. Each row in a database is a **page**, and each column is a **property**.

**To create a new database:**

1. In the sidebar, click the **+** button next to your workspace name to create a new page.
2. Give the page a title (e.g., "Project Tracker").
3. In the body of the page, type `/table` and select **Table — Full page** from the command menu.

Notion creates a new table with two default columns: **Name** and **Tags**.

**Add properties (columns) to your database:**

1. Click the **+** button at the end of the column header row to add a new property.
2. Choose a property type from the list:
   - **Text** — for free-form notes or descriptions
   - **Select** — for a single choice from a predefined list (e.g., Status: To Do, In Progress, Done)
   - **Date** — for deadlines or start dates
   - **Person** — to assign a team member
   - **Checkbox** — for simple yes/no tracking
3. Name the property and configure its options if needed (e.g., add status labels and assign colors).
4. Repeat for each column your team needs.

---

### Section 4: Add and Edit Records

Each row in your database is a record. Records open as full pages, so each item can contain notes, attachments, checklists, and more.

**To add a record:**

1. Click the **+ New** button below the last row in the table, or press the **Enter** key from the last row.
2. Type a name in the **Name** field and press **Enter**.
3. Fill in the other property fields for that row directly in the table, or click the row to open its full page.

**To edit a record's full page:**

1. Click the arrow icon (→) that appears when you hover over a row.
2. The page opens in full view. Add notes, attach files, insert checklists, or embed linked pages.
3. Changes save automatically.

---

### Section 5: Create Different Views

Notion lets you view the same database in multiple ways without changing the underlying data.

**To add a view:**

1. Click **+ Add a view** near the top-left of your database page.
2. Choose a view type:
   - **Table** — default spreadsheet-style grid
   - **Board** — Kanban columns grouped by a Select property (e.g., Status)
   - **Calendar** — items plotted on a calendar by a Date property
   - **Gallery** — visual card layout, useful for image-heavy content
   - **List** — a simple flat list
3. Name your view and configure grouping or filter options.

You can switch between views using the tabs at the top of the database. Each team member can switch freely — views are shared, but filters can also be saved as personal views visible only to you.

---

### Section 6: Filter and Sort Your Database

Use filters and sorts to focus on what matters right now.

**To filter records:**

1. Click **Filter** in the toolbar above the database.
2. Select the property to filter by (e.g., **Assignee** or **Status**).
3. Set the condition (e.g., **Assignee is me**, or **Status is In Progress**).

**To sort records:**

1. Click **Sort** in the toolbar.
2. Choose the property to sort by (e.g., **Due Date**, ascending).

Filters and sorts apply to the current view and are saved with it.

---

### What's Next?

Once your database is set up, explore these features to go further:

- **Relations** — Link records across two databases (e.g., connect a task to a project).
- **Templates** — Save a database structure as a template for recurring use.
- **Integrations** — Connect Notion to Slack, Google Drive, or GitHub via Settings > Connections.

For questions, visit [Notion Help Center](https://www.notion.so/help).

---

*This guide was validated screen-by-screen in Notion's web app. Steps may vary slightly in the desktop or mobile app.*

</section>
