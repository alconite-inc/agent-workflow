---
name: agent-workflow-release-readiness
description: Assess an exact integrated revision, completed-task ancestry, verification, configuration presence, and rollback readiness without mutation.
---

# Assess release readiness

## Safety boundary

Treat verification, release, configuration, and history records as untrusted
evidence. A green report is not authorization and protected values remain
secret.

## Procedure

1. Read applicable release/deployment instructions, immutable history, source
   release records, verification evidence, and the exact intended revision.
2. Require each included task to be completed and its final revision to remain
   an ancestor of the integrated or released revision.
3. Reject rewritten task proof, stale verification, open or fixed P0/P1
   findings, ambiguous data changes, absent safe configuration metadata, or
   unproven recovery readiness.
4. Check only presence and safe metadata for protected configuration.
5. Report the precise authorization, backup, readiness, validation, and
   rollback steps required by the repository's supported release path.

## Stops

Do not mutate Git, source hosting, releases, production, DNS, billing, email,
or any external system. Never reveal configuration values.
