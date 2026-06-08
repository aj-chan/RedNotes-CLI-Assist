XHS Command Suite
Terminal-based Xiaohongshu Workflow Utility

XHS Command Suite is a terminal-based utility designed for working with Xiaohongshu web content through a controlled browser session. It helps users search public notes, inspect note details, review profile information, manage saved content, perform basic engagement actions, and publish image-based posts directly from the command line.

The tool is intended for users who prefer a fast, scriptable, and structured workflow instead of repeatedly operating through the web interface.

What You Can Do
Discover Content
Search Xiaohongshu notes by keyword, browse recommended content, and explore topics or hashtags from the terminal.

Inspect Notes
Open a note by ID, view its visible content, check engagement statistics, and optionally load comments.

Review User Information
Access user profiles, published notes, followers, and following lists by using the platform’s internal user identifier.

Manage Engagement
Like, unlike, collect, uncollect, comment on notes, and delete your own published notes.

Publish Image Notes
Create a new image-based post with a title, multiple images, and body text.

Export Structured Data
Use JSON output for automation, integration, or further data processing.

Command Groups
Instead of memorizing every command individually, you can think of the tool as six practical modules:

Module

Purpose

Account Session

Login, logout, check account state, view current profile

Content Discovery

Search notes, open feed, search topics

Note Reading

Read note content and comments

User Lookup

View profile, posts, followers, following

Engagement

Like, collect, comment, delete

Publishing

Publish image notes

Requirements
Before installation, make sure your environment has:

Python 3.8+

The tool is designed to run from a normal terminal environment. Some features require a valid Xiaohongshu login session.

Installation
Install with uv
uv tool install xhs-cli

Install with pipx
pipx install xhs-cli

Install from Source
git clone <your-repository-url>
cd xhs-cli
uv sync

Replace <your-repository-url> with your own repository address.

Account Setup
Login Using Local Browser Cookies
The simplest login method is to let the tool read an existing Chrome session:

xhs login

Login with QR Code
Use QR code login when browser cookie extraction is unavailable or unreliable:

xhs login --qrcode

Login with Manual Cookie String
You may also provide a cookie string manually:

xhs login --cookie "a1=xxx; web_session=yyy"

The cookie string must include at least:

a1
web_session

Check Login Status
xhs status

This command only checks the saved local session. It does not trigger browser cookie extraction.

Show Current Account
xhs whoami
xhs whoami --json

Logout
xhs logout

Content Discovery
Search Notes
xhs search "coffee"
xhs search "coffee" --json

Browse Recommended Feed
xhs feed

Search Topics
xhs topics "travel"

Note Reading
Open a Note
xhs read <note_id>

Open a Note with Comments
xhs read <note_id> --comments

Provide Token Manually
Normally, token handling is automatic. If needed, you can specify it manually:

xhs read <note_id> --xsec-token <token>

User Lookup
User lookup requires the internal Xiaohongshu user ID, not the public Red ID.

View Profile
xhs user <user_id>

View Published Notes
xhs user-posts <user_id>

View Followers
xhs followers <user_id>

View Following
xhs following <user_id>

Engagement Actions
Like a Note
xhs like <note_id>

Remove Like
xhs like <note_id> --undo

Collect a Note
xhs favorite <note_id>

Remove Collection
xhs favorite <note_id> --undo

Add Comment
xhs comment <note_id> "Nice post!"

Delete Your Own Note
xhs delete <note_id>

View Collected Notes
xhs favorites
xhs favorites --max 10

Publishing
Publish an Image Note
xhs post "My Title" --image photo1.jpg --image photo2.jpg --content "Post body text"

Publish and Return JSON
xhs post "My Title" --image photo1.jpg --content "Post body text" --json

Some accounts may need to complete additional creator-platform login before publishing works correctly.

Local Test Workflow
If you already have a valid saved session at:

~/.xhs-cli/cookies.json

You can run the local smoke test:

./scripts/smoke_local.sh

Run only selected tests:

./scripts/smoke_local.sh -k whoami

By default, the smoke test avoids actions that modify account data. To include mutation actions such as like, favorite, comment, post, or delete:

XHS_SMOKE_MUTATION=1 ./scripts/smoke_local.sh

Optional test variables:

XHS_SMOKE_COMMENT_TEXT="smoke test comment"
XHS_SMOKE_POST_IMAGES="/abs/a.jpg,/abs/b.jpg"
XHS_SMOKE_POST_TITLE="smoke title"
XHS_SMOKE_POST_CONTENT="smoke content"

Output Mode
Most data-related commands support:

--json

This is useful when you want to:

• integrate with scripts
• pipe data to other tools
• save structured results
• build automation workflows
• inspect raw response data
Example:

xhs search "coffee" --json

Session and Cache Files
Cookie File
~/.xhs-cli/cookies.json

Token Cache
~/.xhs-cli/token_cache.json

These files may contain sensitive session information. Keep them private and avoid sharing them.

How the Tool Operates
The tool works by controlling a browser-like environment and loading real Xiaohongshu web pages. After the page content is available, it extracts structured state data from the loaded page and converts it into terminal-friendly output.

For interaction actions such as liking, collecting, or commenting, the tool performs browser-level page operations rather than relying only on static data extraction.

Simplified flow:

Terminal Command
     ↓
Session Loader
     ↓
Browser Runtime
     ↓
Xiaohongshu Web Page
     ↓
Page Data Extraction
     ↓
Formatted Result / JSON Result

Important Notes
Please review the following before use:

• A valid Xiaohongshu account session is required for most commands.
• Manual cookie login must include both a1 and web_session.
• If the saved session becomes invalid, login again.
• Some publishing features may require creator-platform access.
• User profile lookup requires the internal user ID.
• Engagement commands may modify real account data.
• Use mutation commands carefully.
• Keep cookie and token files private.
• Follow Xiaohongshu platform rules and applicable laws.
Debugging
Show Version
xhs --version

Enable Verbose Logging
xhs -v search "coffee"

Show Help
xhs --help

Using with AI Agents
This command suite can be used as an operational skill for AI agents that need controlled Xiaohongshu browsing, note inspection, topic lookup, or structured content retrieval.

To add it into an agent skills directory:

mkdir -p .agents/skills
git clone <your-repository-url> .agents/skills/xhs-command-suite

Or add only the skill instruction file:

curl -o .agents/skills/xhs-command-suite/SKILL.md \
 <your-skill-file-url>

After installation, compatible agents can call the available terminal commands according to the provided skill instructions.

XHS Command Suite Documentation
