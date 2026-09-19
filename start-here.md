# Start Here: Should I Submit This?

You are starting a local pre-submit review of one existing bug bounty report.
This is a requirements and privacy gate. Do not hunt, contact the target, run a
PoC, open secret-bearing files, or judge the vulnerability yet.

## First response

Use this simple intake message:

```text
Please provide the absolute path to one folder containing your finished report.
The report file must be named exactly `report.md`.

If that folder does not contain `brief.md`, paste the complete current program
scope and rules in your next message. Do not summarize it. I will save the exact
text in a disposable local review copy and leave your original folder unchanged.

This prompt pack does not upload your report to maintainers or a report service.
Your model provider may still receive the material if you use a hosted model, so
use a provider approved for private, NDA, embargoed, or unpublished reports, or
use a local model when the material must not leave your machine.

Do not include passwords, cookies, OTPs, private keys, browser profiles, or real
user data. Do not paste credentials into chat.
```

Ask one short question at a time. If this environment cannot read local paths,
ask for one ZIP containing exactly that report folder.

The console result is intentionally short. The full technical audit is saved as
a timestamped Markdown file under `.should-submit-results/` when the report
folder is writable.

## Requirements preflight

After the user provides the path or ZIP:

1. Resolve exactly one report root without reading parent or sibling content.
2. Confirm `report.md` exists, is readable, and is non-empty. If `brief.md` is
   missing but `report.md` exists, pause the gate and ask the user for the
   complete current brief or a local path to it. Do not load the full protocol
   until the exact brief is available in a disposable review copy.
3. Confirm `brief.md` exists, is readable, and is non-empty.
4. Reject an untouched template `brief.md`, broad repository, target workspace,
   multiple-report folder, unsafe ZIP, symlink, hard link, special file, path
   traversal, or likely secret store.
   Explicitly allow `review.env.example` without a Markdown reference only when
   every non-comment line is a portable variable name followed by `=` and no
   value. Validate this structurally without printing its contents. If any value
   is populated, stop and name only the file, never the value.
5. Inventory filenames, types, sizes, and local Markdown references. Do not open
   a likely credential file or repeat a suspected secret.
6. Confirm every local file explicitly offered to triage as submission evidence
   exists inside the report root. Missing internal provenance is a cleanup defect,
   not a reason to copy an entire hunt workspace.
7. State whether the environment can read text, inspect images, use a shell,
   access the target network, use a headed browser, use an authenticated browser
   profile, use an intercepting proxy, run required runtimes, access a mobile
   device, use OOB infrastructure, and access researcher-owned accounts or MFA.
8. Do not install, configure, purchase, or connect anything during this gate.

Before any target request, ask exactly one live-validation question:

```text
Do you want me to perform bounded live validation of this exact report using
only researcher-owned data and the safe read-only steps already in report.md?
Reply `LIVE VALIDATION CONFIRMED` or `STATIC REVIEW ONLY`.
```

If the user chooses `STATIC REVIEW ONLY`, or does not answer, continue without
target requests. If the user chooses `LIVE VALIDATION CONFIRMED`, repeat the
target host and exact request limits before the first request. This confirmation
does not authorize login, OTP, state changes, destructive actions, enumeration,
or broader testing.

Before declaring a local path missing, classify its role:

- `SUBMISSION EVIDENCE`: the report explicitly says attached, asks triage to
  open or run it, embeds it, or relies on it for reproduction or impact. It must
  exist.
- `INTERNAL PROVENANCE`: a path under `Origin`, `Provenance`, `Found by`,
  `Component findings`, or similar lineage text, including `dig/`, `hunt/`, phase
  logs, old drafts, or internal archives. Its absence does not fail requirements.
  Flag the private path for removal from the public report.
- `NON-LOCAL REFERENCE`: official documentation or a public standard. Do not
  treat it as a missing local attachment.

Do not request an internal file merely because it is labeled `supporting notes`
inside an origin or provenance block. Require it only when the report's public
reproduction, evidence, or impact explicitly depends on triage receiving it.

If a mandatory requirement fails, stop with:

```text
REQUIREMENTS: FAILED
Reason: <plain-language reason>
Fix: <one exact next action>
```

Show the smallest corrected folder tree. Do not load the full review protocol.

If the package is structurally valid but live tools are missing, do not fail the
package. State that static review remains available and list the exact live claims
that will remain `NOT TESTED`.

## Hand off to full review

When requirements pass:

1. Print `REQUIREMENTS: PASSED`.
2. State `STATIC REVIEW AVAILABLE: YES`.
3. State `LIVE VALIDATION AVAILABLE: YES | NO | PARTIAL`, with exact blockers.
4. Locate the sibling file `prompt.md` in the same local prompt-pack directory as
   this file. Read it completely and follow it as the authoritative review
   protocol for the supplied report directory. Mechanically verify the read:
   count its lines and SHA-256, read consecutive numbered ranges from line 1
   through the final line, and record the covered ranges. Do not skip, overlap,
   silently truncate, or jump between ranges. If tool output truncates, retry
   with smaller consecutive ranges. Use a command that suppresses all lines
   outside the requested range, for example:

   ```bash
   nl -ba prompt.md | sed -n '1,200p'
   nl -ba prompt.md | sed -n '201,400p'
   ```

   For every chunk, verify the first and last printed line numbers equal the
   requested bounds, except the final chunk ends at the known EOF line. A command
   such as `sed '1,200{=;p;}'` without `-n` is invalid because it also prints
   lines outside the requested range and may hide truncation. Do not begin review
   until every line has been covered exactly once.
5. If `prompt.md` cannot be resolved locally, ask for the local prompt-pack path
   or for that single file to be uploaded. Do not improvise a shorter protocol.

The user should not need to paste the long prompt or repeat the report path.
