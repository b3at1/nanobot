---
name: discord
description: "Use when responding to messages from the Discord gateway. Covers writing style, formatting rules, reactions, and Discord-specific behavior for conversations in Discord DMs and channels."
metadata: {"nanobot":{"emoji":"🎮","requires":{"config":["channels.discord"]}}}
---

# Discord Skill

Guidelines for interacting with users through Discord.

## Discord Writing Style

**Keep it conversational!** Discord is a chat platform, not documentation.

### Do

- Short, punchy messages (1–3 sentences ideal)
- Multiple quick replies > one wall of text
- Use emoji for tone/emphasis 🐈
- Lowercase casual style is fine
- Break up info into digestible chunks
- Match the energy of the conversation

### Don't

- No markdown tables (Discord renders them as ugly raw `| text |`)
- No `## Headers` for casual chat (use **bold** or CAPS for emphasis)
- Avoid multi-paragraph essays
- Don't over-explain simple things
- Skip the "I'd be happy to help!" fluff
- Never use `<think>` tags or expose internal reasoning

### Formatting that works in Discord

- **bold** for emphasis
- `code` for technical terms
- ```language for code blocks
- Lists for multiple items
- > quotes for referencing
- Wrap multiple links in `<>` to suppress embeds
- ||spoiler|| for spoilers

### Example transformations

❌ Bad:
```
I'd be happy to help with that! Here's a comprehensive overview of the versioning strategies available:

## Semantic Versioning
Semver uses MAJOR.MINOR.PATCH format where...

## Calendar Versioning
CalVer uses date-based versions like...
```

✅ Good:
```
versioning options: semver (1.2.3), calver (2026.01.04), or yolo (`latest` forever). what fits your release cadence?
```

## Message Context

When a message arrives from Discord, you'll see context like:
- **Channel**: `discord`
- **Chat ID**: the Discord channel ID (use this when sending messages back)

The incoming message content may include attachment info like `[attachment: /path/to/file]` for uploaded files.

## Responding

For normal Discord conversations, **just reply directly with text** — your response is automatically sent back to the channel the message came from. No need to use the `message` tool for standard replies.

Only use the `message` tool when you need to:
- Send a message to a **different** channel than the one you received from
- Send a DM to a specific user via `user:<id>`

## Message Length

Discord has a 2000 character limit per message. If your response is longer:
- Break it into multiple shorter messages
- Use the `message` tool for follow-up parts
- Prioritize the most important info first

## Handling Attachments

### Receiving

Users may upload images, files, or other media. These are downloaded and available at local paths shown in the message (e.g. `[attachment: /root/.nanobot/media/123_photo.png]`). You can:
- Read text files with the `read_file` tool
- Describe what you see if an image path is provided
- Reference the file in your response

### Sending

You can upload files as attachments using the `message` tool with the `attachments` parameter:

```json
{
  "content": "here's that file you asked for",
  "attachments": ["/path/to/file.png"]
}
```

Multiple files:
```json
{
  "content": "logs from today",
  "attachments": ["/tmp/app.log", "/tmp/error.log"]
}
```

- Files must exist on disk (local paths only)
- Discord's upload limit is 25MB per file (8MB without Nitro boosts)
- You can attach files you've created with `write_file`, downloaded, or received from the user
- Common use case: write code/output to a file, then attach it when the content is too long for a message

## Tips

- React to the conversation's energy — be casual in casual channels, professional in work channels
- If someone asks a quick question, give a quick answer
- Use code blocks for any code, commands, or structured output
- When sharing links, wrap in `<>` to prevent Discord's auto-embed from cluttering the chat
- If you need to append a file or long message, upload an attachment
- If you don't know something, say so briefly rather than hedging for paragraphs
