—

**Name:** RedNote
**Description:** “Headless-browser-based CLI skill for Xiaohongshu (小红书, RedNote, XHS) to search notes, read posts, browse profiles, like, favorite, comment, and publish from the terminal.”
**Author:** AJ
**Version:** “2.1”
**Tags:**
  - RedNote
  - xiaohongshu
  - 小红书
  - rednote
  - social-media
  - cli


# xhs-cli Skill

This CLI tool allows you to interact with Xiaohongshu (小红书). You can search notes, read details, browse user profiles, and perform various interactions such as liking, favoriting, and commenting.

## Prerequisites

```bash
# Install (requires Python 3.8+)
uv tool install xhs-cli
# Or: pipx install xhs-cli
```

## Authentication

All commands require valid cookies to function.

```bash
xhs status                     # Check saved login session (no browser extraction)
xhs login                      # Automatically extract Chrome cookies
xhs login —cookie “a1=…”    # Alternatively, provide cookies manually
```

Authentication first attempts to use saved local cookies. If these are unavailable, it automatically detects local Chrome cookies using browser-cookie3. If extraction fails, QR code login is available as an alternative.

## Command Reference

### Search

```bash
xhs search “咖啡”              # Search notes and display them in a rich table format.
To search for “咖啡” in JSON format, use the command:

```bash
xhs search “咖啡” —json
```

### Reading Notes

To view a note, use the command:

```bash
xhs read <note_id>
```

To view a note with comments, use the command:

```bash
xhs read <note_id> —comments
```

To view a note using a manual token, use the command:

```bash
xhs read <note_id> —xsec-token <token>
```

To view a note in JSON format, use the command:

```bash
xhs read <note_id> —json
```

### User Information

To look up a user’s profile by internal user ID in hex format, use the command:

```bash
xhs user <user_id>
```

To view a user’s profile in JSON format, use the command:

```bash
xhs user <user_id> —json
```

To list a user’s published notes, use the command:

```bash
xhs user-posts <user_id>
```

To list a user’s published notes in JSON format, use the command:

```bash
xhs user-posts <user_id> —json
```

To view a user’s followers, use the command:

```bash
xhs followers <user_id>
```

To view a user’s following, use the command:

```bash
xhs following <user_id>
```

### Discovery

To explore the recommended feed, use the command:

```bash
xhs feed
```

To view the recommended feed in JSON format, use the command:

```bash
xhs feed —json
```

To search for topics or hashtags, use the command:

```bash
xhs topics “旅行”
```

To view the search results for “旅行” in JSON format, use the command:

```bash
xhs topics “旅行” —json
```

### Interactions (Requires Login)

To like or unlike a note, use the command:

```bash
xhs like <note_id>
```

To undo a like, use the command:

```bash
xhs like <note_id> —undo
```

To favorite or unfavorite a note, use the command:

```bash
xhs favorite <note_id>
```

To undo a favorite, use the command:

```bash
xhs favorite <note_id> —undo
```

To comment on a note, use the command:

```bash
xhs comment <note_id> “好棒！”
```

To delete your own note, use the command:

```bash
xhs delete <note_id>
```

### Favorites

To list your favorites, use the command:

```bash
xhs favorites
```

To limit the number of favorites displayed, use the command:

```bash
xhs favorites —max 10
```

To view your favorites in JSON format, use the command:

```bash
xhs favorites —json
```

### Posting

To post a note with a title, images, and content, use the command:

```bash
xhs post “标题” —image photo1.jpg —image photo2.jpg —content “正文”
```

To post a note with a title, images, and content in JSON format, use the command:

```bash
xhs post “标题” —image photo1.jpg —content “正文” —json
```

### Account Information

To check your saved session status, use the command:

```bash
xhs status
```

To view your full profile information, use the command:

```bash
xhs whoami
```

To view your full profile information in JSON format, use the command:

```bash
xhs whoami —json
```

To log in to your account, use the command:

```bash
xhs login
```
Clear cookies by logging out:

```bash
xhs logout
```

## JSON Output

Major query commands support `—json` for machine-readable output:

```bash
xhs search “咖啡” —json | jq ‘.[0].id’           # First note ID
xhs whoami —json | jq ‘.userInfo.userId’          # Your user ID
xhs favorites —json | jq ‘.[0].displayTitle’      # First favorite title
```

## Common Patterns for AI Agents

```bash
# Obtain your user ID for further queries
xhs whoami —json | python3 -c “import sys,json; d=json.load(sys.stdin); print(d.get(‘userInfo’,{}).get(‘userId’,’’))”

# Search and retrieve note IDs (xsec_token is automatically cached for future use)
xhs search “topic” —json | python3 -c “import sys,json; [print(n[‘id’]) for n in json.load(sys.stdin)[:3]]”

# Ensure login before executing actions
xhs status && xhs like <note_id>

# Read a note with comments for summarization
xhs read <note_id> —comments —json
```

## Error Handling

- Commands return a code of 0 on success and a non-zero code on failure.
- Error messages are prefixed with ❌.
- Login-required commands provide clear instructions to execute `xhs login`.
- `xsec_token` is automatically resolved from the cache; a manual `—xsec-token` option is available as a fallback.

## Safety Notes

- Avoid asking users to share raw cookie values in chat logs.
- Prefer automatic extraction via `xhs login` over manual cookie input.
- If authentication fails, prompt the user to re-login using `xhs login`.

