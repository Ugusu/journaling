Personal Journal Script
=======================

This Bash script manages encrypted, versioned personal journal entries and image attachments. It supports editing, viewing, syncing with a remote (via rclone), and full-text search.

Requirements
------------
- `gpg` (for encryption/decryption)
- `rclone` (for syncing with remote)
- `glow` or any Markdown terminal renderer (optional, for `view`)
- A configured remote in rclone (e.g., "gdrive:journal")
- `EDITOR` environment variable set (e.g., to `nano`, `vim`, etc.)

Directory Structure
-------------------
```
~/journal/YYYY/MM/YYYY-MM-DD.md.gpg
~/journal/YYYY/MM/images/YYYY-MM-DD-*.gpg
~/Documents/.journal-preview/ # Temporary decrypted previews
```

Usage
-----

```bash
journal [command] [--date=YYYY-MM-DD] [args...]
```


Commands
--------

- `edit [YYYY-MM-DD]`  
  Decrypt and open a journal entry for editing. Re-encrypts only if changes were made.  
  Defaults to today if no date is given.

- `attach [YYYY-MM-DD] img1 [img2 ...]`  
  Encrypt and attach images to a journal entry. Files are renamed with the date prefix.

- `view [--date=YYYY-MM-DD]`  
  Preview a journal entry and its images in terminal using `glow` or `$JOURNAL_RENDERER`.

- `search --query=STRING`  
  Decrypt and search all journal entries for the given string. Matches are printed with context.

- `sync-up`  
  Upload the encrypted journal and attached images for a specific date to the rclone remote.

- `sync-down`  
  Download the encrypted journal and images for a specific date from the rclone remote.

- `sync-up-all`  
  Upload **all** `.gpg` files recursively from `~/journal/` to the rclone remote.

- `sync-down-all`  
  Download **all** `.gpg` files recursively from the rclone remote to `~/journal/`.

Environment Variables
---------------------
- `JOURNAL_REMOTE`  
  Default: `gdrive:journal`  
  Defines the rclone remote path.

- `JOURNAL_RENDERER`  
  Default: `glow`  
  Command used to render Markdown in `view` mode.

Tips
----
- You can attach images before editing. After editing, paste the image markdown like:

- Always run `sync-up` after editing to upload the encrypted version.