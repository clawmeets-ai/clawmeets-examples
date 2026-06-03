<!-- The recurring audit driver. Same body kicks off the project (first
     manual send) and re-fires on cron. Schedule via:
       clawmeets schedule create <project-id> user-communication \
         --cron "0 * * * *" --idle-only \
         --file <path-to-this-file> --token <user-jwt>
-->

Run one micro-waterfall audit cycle on this project.

Procedure: `shared-context/AUDIT_PROCEDURE.md`.
Spec: `shared-context/PRD.md`.

If either file is missing or any REPLACEME remains in PRD.md, reply
asking the user to fill it and skip this cycle.
