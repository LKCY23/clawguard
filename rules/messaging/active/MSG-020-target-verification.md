Title: Target Verification

ID: MSG-020
Status: active
Pack: messaging
Owner: user
Last Updated: 2026-03-04

When:
- Any time a message would be sent/edited/deleted/reacted to in an external channel.

Rule:
- MUST verify the target channel and recipient before sending.
- If the target is ambiguous (multiple chats/channels), MUST ask one targeted clarification.
- MUST avoid sending to group chats when the user intent is a private reply (and vice versa).

Related:
- [MSG-010] Send Authorization
