# dum

Daily update mail. `dum` starts today's update as a markdown note in the vault
and opens it in Obsidian; you write the update there during the day; `dum -d`
turns it into an Outlook draft that you double-click, read once, and send.

The point is that the writing happens in the place where the rest of the
research notes live, in markdown, and the email formatting is somebody else's
problem. Nothing is ever sent by this tool. It produces a draft and stops, so
the last read before the message leaves is yours.

## Install

Requires macOS, Python 3 (the system one is enough, no packages), Obsidian, and
Outlook for Mac.

```sh
git clone https://github.com/oruc47/dum.git
ln -s "$PWD/dum/dum" ~/bin/dum
```

Then open the script and edit the block at the top:

```python
VAULT = Path("/Users/oruc/Desktop/bfi2b")
VAULT_NAME = "bfi2b"                                  # the name Obsidian shows
UPDATES = VAULT / "7 - updates" / "daily_updates"
TO = "Ali Hortacsu <hortacsu@gmail.com>, Ali Hortacsu <hortacsu@uchicago.edu>"
CC = ""
```

`VAULT_NAME` has to be the vault name as Obsidian knows it, since that is what
the `obsidian://` link uses. `TO` and `CC` are only the defaults written into a
new note; changing the recipients for one day is an edit to that day's
frontmatter, not to the script.

## The two commands

```sh
dum        # start today's note, open it in Obsidian
dum -d     # build the Outlook draft from today's note   (--done also works)
```

`dum` creates

```
7 - updates/daily_updates/daily_update_2026-09-16/
└── daily_update_2026-09-16.md
```

and opens that note in Obsidian. Run it again later in the day and it opens the
same note: an existing note is never rewritten, so a half-written update cannot
be lost by typing `dum` out of habit.

`dum -d` reads the note and writes the draft beside it:

```
7 - updates/daily_updates/daily_update_2026-09-16/
├── daily_update_2026-09-16.md
└── [Update] Wednesday, September 16.emltpl
```

Double-click the `.emltpl`. macOS knows that file as
`com.microsoft.outlook.template`, so Outlook opens it as a **new** message with
the recipients and subject filled in and a Send button, rather than as
something that arrived in your inbox. Read it there and send it.

Run `dum -d` as many times as you like; it rebuilds the draft from whatever the
note currently says. The draft is a generated file, so it can be deleted
without losing anything.

## The note

```markdown
---
to: Ali Hortacsu <hortacsu@gmail.com>, Ali Hortacsu <hortacsu@uchicago.edu>
cc:
subject: "[Update] Wednesday, September 16"
date: 2026-09-16
---

Merhaba Hocam,

**What I did Today:**

- Ran the full-sample pass; 3,233 reports at `temperature=0`.
- Wrote up the [feature memo](https://example.org/memo) for Dr. Oto.

**What I will do tomorrow:**

- Start the holdout evaluation.

**Questions for Ali Hoca (y/n)?**

n

—
Ikbal
```

What you read in the note is what gets sent, greeting and signature included.
The subject comes from the frontmatter, so correcting it for one day means
editing that line; the date in it follows the template you were already using,
`[Update] <weekday>, <month> <day>`, with no year.

Bullets left as a bare `-` are dropped from the email, so the unfilled parts of
the skeleton do not turn into empty bullets in front of your advisor.

## Markdown that survives the trip

The email carries a plain text part and an HTML part, and Outlook shows the
HTML one in Verdana 11pt, which is the formatting of the template these updates
have always used. Supported: paragraphs, `-` or `*` bullet lists, `**bold**`,
`*italic*`, `` `code` ``, and `[text](url)` links. Ampersands and angle
brackets are escaped, so writing `d-dimer < 500` is safe.

Anything else — headings, tables, images, block quotes — is passed through as
plain text. A daily update does not need them, and the converter stays small
enough to read in one sitting.

## Notes

The folder name uses the ISO date so the updates sort chronologically; the
subject line uses the weekday and month, because that is what reads well in a
mail client.

`dum -d` before `dum` on a given day exits with a message rather than inventing
an empty update.

Vibe coded with Claude Code, then read and corrected by hand.
