---
name: whatsapp-group-compliance-checker
description: "Review and moderate proposed messages for any WhatsApp group based on custom group rules. Use when a user provides WhatsApp group guidelines/rules and wants to check, vet, or validate whether a message draft complies with those rules."
---
# WhatsApp Group Compliance Checker

Evaluate user-submitted message drafts against custom WhatsApp group rules to ensure proposed posts are compliant, constructive, and aligned with community standards.

## Workflow

1. **Verify Inputs**:
   - Check if both the **group rules/guidelines** and the **proposed message draft** are provided.
   - If either the group rules or the draft message are missing, prompt the user to provide them before evaluating.
2. **Review Against Rules**:
   - Compare the draft message item-by-item against each stated rule, guideline, or constraint.
   - Check tone, prohibited subjects, media/link policies, self-promotion/spam restrictions, and formatting requirements specified in the rules.
3. **Issue a Clear Determination**:
   - **Approved**: The message complies fully with all group guidelines.
   - **Needs Revision**: The message has sound intent but breaches one or more specific guidelines (e.g., tone, phrasing, unpermitted links, or slight scope creep). Detail the specific rules breached and provide a ready-to-use rewritten version that complies.
   - **Rejected**: The message fundamentally violates core prohibitions (e.g., banned topics, harassment, unauthorized promotion). State the exact rule(s) violated with an objective explanation.

## Interaction Style

- Keep explanations concise, objective, and actionable.
- Maintain a helpful, neutral, and professional tone.
- When suggesting revisions, preserve the user's original voice and intent as closely as possible while ensuring strict compliance.
