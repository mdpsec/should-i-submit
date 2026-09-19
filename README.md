# Should I Submit This?

This reviewer checks one finished bug bounty report before you submit it. It can
confirm the report, find missing proof, identify unsafe steps, or explain why the
report should not be submitted. It does not hunt for a better bug or submit
anything for you.

## What you need

Create one folder for one report. It needs only two files:

```text
my-report/
  report.md
  brief.md
```

- `report.md` is the complete report you are about to submit. It is not recon
  notes, a possible lead, or a request for the agent to finish the research.
- `brief.md` is the complete current program scope and rules copied from the
  bounty platform. Keep the platform's original format. Do not summarize it.

Add a screenshot, PoC, HAR, video, or evidence file only when `report.md`
actually refers to it. Do not add your whole research folder.

See [`examples/example-report/`](examples/example-report/) for a sanitized folder
and [`examples/example-result.md`](examples/example-result.md) for the resulting
short answer.

Never put passwords, cookies, tokens, OTPs, recovery codes, private keys,
browser profiles, or unrelated user data in the report folder.

The prompt pack has no report-upload service and does not send reports to its
maintainers. A hosted model provider may still receive the text and attachments
you give it, so use an approved provider or a local model for private material.

## Choose the agent

Recommended models:

- GPT-5.6 Sol on high reasoning
- Claude Opus 5.0 on high reasoning

## Run it

1. Give a file-capable agent this folder containing `start-here.md` and
   `prompt.md`. Start with `start-here.md`; do not paste or run `prompt.md`
   directly.
2. Send the agent this one instruction:

   ```text
   Read /path/to/should-submit/start-here.md completely and follow it.
   ```

3. When asked, give the absolute path to `my-report/`.

The agent checks the folder first. If something is missing, it tells you the
exact file or change needed. If the folder is valid, it loads `prompt.md`
automatically. You never need to paste the long prompt.

If your folder has `report.md` but no `brief.md`, the agent asks you to paste the
complete current program brief. It saves the exact text in a disposable review
copy and leaves your original folder unchanged. You do not need to create the
file manually.

Each completed review is also saved locally as a timestamped Markdown file under
`my-report/.should-submit-results/`. These files are review outputs, not evidence
for submission. Older results are ignored on later reviews, so one report folder
can hold a history of runs.

The console stays short: decision, reason, next action, unsafe step, and the
saved audit path. Open the Markdown result when you want the full technical
details.

Long reports are acceptable when they document one root cause or one necessary
exploit chain. Reports that combine unrelated findings must be split into one
folder per finding. Unexpectedly large packages stop before bulk reading and
name the files that need a smaller export or explanation.

Run it through Codex CLI or Claude Code with one of the recommended models. The
agent adapts safe read-only checks to Windows, macOS, or Linux. You do not need
to run the report's Bash commands yourself.

The answer begins with one of these plain results:

- `YES`: the exact report passed safe live validation.
- `NO`: do not submit this report.
- `NOT YET`: one specific proof or repair is still needed.
- `CANNOT DECIDE SAFELY`: scope, access, or safety prevents an honest answer.

The console shows the plain result and the path to the saved audit. Open that
local Markdown file for the detailed technical review. You can use the first
block alone when you only need the decision and next action.

`NOT YET` pauses the review. Reply `CONTINUE` only to approve the one exact safe
next step shown by the agent, or reply `STOP`. `YES` and `NO` finish the review;
the agent does not continue testing automatically.

## Optional files

Most people do not need these:

- `review.env.example`: for an authenticated report, list required variable
  names with empty values. Never put real values in it.
- `review-policy.md`: personal rules such as "do not submit below High." Copy
  and rename `review-policy.example.md` only when you want this.

Real login material stays outside the report folder. Use an already signed-in
researcher-owned browser, manual login, protected environment variables, or a
protected local secret file when the reviewer asks for an approved handoff.
Never paste credentials into chat.

## Tool limitations

Missing tools do not automatically make a report invalid. Without a browser,
proxy, account, device, or command-line tool, the reviewer performs every safe
static check it can and names the exact live claim it could not test.

If setup or installation is needed, the reviewer asks first and never silently
installs a dependency, uses `sudo`, or runs an untrusted installer. Any target
state change requires separate, exact permission. Destructive, high-risk,
denial-of-service, real-user, and unknown-owner actions are refused.

## Repository contents

The standalone public repository should contain:

```text
.gitignore
LICENSE
README.md
start-here.md
prompt.md
brief.md
review.env.example
review-policy.example.md
SECURITY.md
examples/
```

For an actual review, the agent needs only `start-here.md`, `prompt.md`, and the
hunter's separate folder containing `report.md` and `brief.md`.
