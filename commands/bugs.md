---
description: Scout my assigned Linear issues in parallel and rank them by how quick they are to fix
---
My open Linear issues for this repo's team:
!`linear issue query --assignee @me -s triage -s backlog -s unstarted -s started --no-pager 2>&1`

$ARGUMENTS

Do this:

1. From the list above, take the issues that are bugs. If labels aren't shown, take all of them; the scouts will sort it out. If I named specific IDs or a filter above, use only those. If the list is empty or errored, stop and tell me.

2. Start one `issue-scout` subagent **per issue, all in parallel**, passing each one its issue ID. Do not investigate them yourself, and do not edit anything.

3. When all the scouts are done, give me one table sorted by quick wins first (S + high confidence at the top, L or low confidence at the bottom):

   | ID | Title | Size | Confidence | Likely cause | Blockers |

4. Below the table, list:
   - **Quick wins:** the IDs I could probably knock out today
   - **Needs info:** issues blocked on the reporter, with a one-line question to ask each one
   - **Overlaps:** issues that look like the same root cause or would touch the same files. These shouldn't be fixed in parallel.

Stop there. I'll pick which ones to start.
