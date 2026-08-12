# Check Vivideo video status

Report the status of the user's Vivideo videos: $ARGUMENTS

Steps:

1. If a video ID was given, call `get_video` with it.
2. If a name was given, call `list_videos` and match it, then `get_video` for
   the match.
3. If nothing was given, call `list_videos` and summarize the most recent
   videos with their statuses.

Report the real status from the tool result — queued, processing, completed, or
failed — and include the playable URL only when the result contains one. If a
video is still rendering, say so plainly instead of implying it is finished.

Never invent video IDs, statuses, or URLs.
