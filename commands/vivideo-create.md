# Create a Vivideo AI video

Create an AI video with Vivideo from the user's description: $ARGUMENTS

Steps:

1. Call `check_configuration` to confirm the Vivideo connection is usable. If it
   is not, tell the user to connect the Vivideo MCP server in Cursor settings and
   stop.
2. Call `check_account`. If it shows `premium=false`, call `create_video_plan`
   with the description instead of a create tool, show the plan, and follow the
   "Next step" line in each result (`revise_video_plan` for a change,
   `generate_video_from_plan` to make it). Skip steps 3–6.
3. If the user named a specific model, resolution, aspect ratio, or duration,
   call `list_models` first and pick only from the returned values. Otherwise
   prefer `create_auto_video`.
4. If the request includes an image URL, use `create_manual_video` with
   `image_url` for image-to-video.
5. Before creating, state briefly what will be generated and that rendering may
   consume Vivideo credits. Wait for the user to approve the tool call.
6. After creation, report the returned video ID and its current status. Do not
   claim the video is ready unless a tool result says so.

Never invent model IDs, durations, credit costs, or a completion status.
