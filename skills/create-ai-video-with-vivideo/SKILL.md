---
name: create-ai-video-with-vivideo
description: Create or manage a real AI video with Vivideo. Use when the user asks to generate an AI video, turn text or an image into video, create a product demo or social clip, check a Vivideo render, or list their Vivideo videos.
---

# Creating AI videos with Vivideo

Vivideo renders real videos through the `vivideo` MCP server. The tools are
authoritative — this skill only helps sequence them correctly.

## Before anything else

Call `check_configuration` when a request needs the user's Vivideo account. It
reports whether the connection is authenticated and usable. If it indicates the
user is not connected, tell them to connect the Vivideo MCP server in Cursor
settings rather than guessing at a fix.

For plan, credit balance, or quota questions, call `check_account`.

## Free accounts: the video plan

When the user asks for a video, call `check_account` first. If it shows
`premium=false`, do not call `create_auto_video` or `create_manual_video`:

1. Call `create_video_plan` with the user's request in their own words. Vivideo's
   planner writes the plan; never write a plan, script or scenes yourself.
2. Show the plan from the result (title, style, script, scenes) and ask whether to
   make the video or change something. For a change, call `revise_video_plan`.
3. When the user says go, call `generate_video_from_plan` and give the user the
   Vivideo link from its result exactly as written.
4. Later, `get_video_plan` reports whether the video is being made or finished.

Follow the "Next step" line in each tool result.

## Choosing how to create the video (paid accounts)

Prefer `create_auto_video` for the simplest complete result. It selects
reasonable defaults, so it is the right default when the user describes what
they want in plain language and has not asked for a specific model.

Use `create_manual_video` when the user wants direct control. In that case:

1. Call `list_models` and choose from the returned options.
2. Optionally call `estimate_credits` to show cost before committing.
3. Call `create_manual_video` with valid values taken from `list_models`.

For image-to-video, use `create_manual_video` with `image_url`.

Call `list_avatars`, `list_voices`, or `list_brand_kits` only when the request
actually involves an avatar, a specific voice, or brand styling.

## Checking status and retrieving videos

Use `get_video` for a specific video and `list_videos` to browse recent ones.
`wait_for_video` polls until a render finishes; use it only when the user wants
to wait, and tell them it may take a while.

Prefer `get_video` and `list_videos` in Cursor. The `show_video` tool exists for
a rich card surface that Cursor does not render, so it offers no advantage here.

## Rules

- Never invent model IDs, video IDs, prices, credit balances, or render status.
  Every such value must come from a tool result.
- Rendering can consume the user's Vivideo credits. Creation is not free and not
  reversible, so confirm before calling a create tool when the user has not
  clearly asked for it.
- Respect Cursor's tool approval prompts. If a creation call fails, do not blindly
  retry it — a retry can produce a second charged render. Report what happened
  and reuse the same idempotency key if retrying is genuinely intended.
- Say clearly when a video is queued or still processing rather than implying it
  is finished.
- This skill is about video creation. Do not edit the user's source code unless
  they explicitly asked for a code change.
