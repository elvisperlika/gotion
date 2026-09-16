## Functional Requirements

Expressed as user stories. Non-functional constraints on these behaviours are
specified separately as [Quality Attribute Scenarios](#quality-attribute-scenarios).

### Identity and Workspace

- **US-01 — Sign up and sign in**: As a user, I want to register and authenticate with my email address and password so that I can access Gotion.
- **US-02 — Manage workspaces**: As a user, I want to create and manage multiple workspaces so that I can keep different projects or teams separate.
- **US-03 — Manage workspace members**: As a workspace administrator, I want to add, remove, and assign Admin, Member, or Guest roles to workspace members so that I can control access to the workspace.

### Pages and Hierarchy

- **US-04 — Manage pages and blocks**: As a user, I want to create, read, update, and delete pages and organize their blocks in a tree so that I can structure content hierarchically.
- **US-05 — Customize page metadata**: As a user, I want to set a page title, icon, and cover so that pages are easy to recognize and personalize.
- **US-06 — Restore deleted pages**: As a user, I want to move deleted pages to a trash area, restore them, or permanently delete them so that I can recover from mistakes and manage unwanted content.
- **US-07 — Review page history**: As a user, I want to view a page's revision history and see who made each change so that I can trace and understand updates.

### Editor and Blocks

- **US-08 — Edit Markdown blocks**: As a user, I want to work with an ordered, nestable list of blocks so that content remains modular and structured.
- **US-09 — Use native Markdown block types**: As a user, I want to create formatted text, headings, bulleted, numbered, and to-do lists, and code blocks so that I can write rich content with Markdown.
- **US-10 — Reuse synchronized content**: As a user, I want to create reusable and synchronized blocks so that I can maintain shared content in one place.

### Permissions and Collaboration

- **US-11 — Apply granular permissions**: As a workspace administrator, I want to assign granular access permissions that inherit from the workspace so that users have the appropriate level of access.
- **US-12 — Collaborate in real time**: As a collaborator, I want to see other connected users' avatars and cursors and co-edit the same page so that we can work together.

### Discussions, Search, and Notifications

- **US-13 — Discuss content in context**: As a collaborator, I want to add comment threads to pages or individual blocks, react to comments, and mention users with `@username` so that conversations stay connected to the relevant content.
- **US-14 — Search the workspace**: As a user, I want to run full-text searches across workspace titles and content so that I can find information.
- **US-15 — Receive activity notifications**: As a user, I want to receive in-app or email notifications about mentions, shares, and important changes so that I can stay informed.
