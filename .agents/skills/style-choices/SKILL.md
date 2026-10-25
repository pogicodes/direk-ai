---
name: style-choices
description: Turn ready-made style choices (multi-speaker captions, quiet-voice text, corner badge, translations, …) on or off in the active profile's style.md. Use when the user asks to add or remove one ("add multi-speaker captions", "remove the corner badge"), change one's values ("change Sam's color", "add my mom as a speaker"), asks what style options there are, or wants part of their style made reusable ("make my badge a reusable choice").
---

# style-choices

The **catalog** is a set of ready-made style choices any profile can switch on, one
file per choice in `choices/` next to this file. Adding a choice writes it into
`profile/style.md` as normal bullets, in the user's words where they gave them. No
markers are left behind, so a choice is always recognized **by meaning**: bullets the
user wrote by hand count the same as bullets this skill wrote. Speak plainly, in the
user's language.

## Where things live

| Path | Purpose |
| --- | --- |
| `.agents/skills/style-choices/choices/<name>.md` | The catalog. Generic and committed: no personal names, colors or labels. |
| `profile/style.md` | The active profile's style. The only file this skill changes, besides what a choice's **Also** lists. |
| `profiles/<name>/style.md` | Saved profiles. Read-only here, for showing another profile's status. |
| `profile.example/style.md` | Section order, used when a choice needs a section that is missing. |

## A choice file

Frontmatter: `name`, `description` (one line, shown in the list), `section` (the
`style.md` heading it goes under). Then:

- **Ask**: the questions to ask when adding it, each with a default where one makes
  sense, so "defaults" answers them all. Questions marked *required* have no default.
- **Write**: the bullet template. `{placeholders}` are filled from the answers,
  `{this | that}` picks one wording by the answer, and `[…]` parts are written only
  when their answer was given.
- **Conflicts**: what in `style.md` clashes with it.
- **Also** (optional): other profile files to update when adding or changing it.

## Status

A choice is **on** when `style.md` has bullets that say what its **Write** template
says, in any wording, with any values. If only part of it is there, it is **partly
on**: name the missing part. Otherwise it is **off**.

## List ("what style options are there")

For every file in `choices/`: name, **on**/**partly on**/**off** for the active
profile, and its description. If the user names another profile, judge its
`profiles/<name>/style.md` instead, without switching.

## Add

1. **Check status.** Read the choice file and `profile/style.md`. Already on: reply
   "Already on:" with the matching bullets, name any clash step 2 would find, and
   offer to change the values, then stop.
2. **Check conflicts.** Compare `style.md` with the file's **Conflicts** and with
   anything else that contradicts the **Write** bullets. On a clash, name both sides
   in one line and ask which to keep. Change nothing until the user answers. If
   they keep the old bullet, stop.
3. **Ask** the **Ask** questions in one message, defaults marked. Skip any the
   request already answered ("add a corner badge saying DAILY VLOG").
4. **Write** the filled bullets under the choice's section, creating the section in
   `profile.example/style.md` order if it is missing. If the user chose the choice
   over a clashing bullet, edit or remove that bullet as the conflict line says.
5. Do what **Also** lists.
6. **Confirm** in one line ("Added to style: speaker colors for you, Sam and
   Mia."), mentioning any **Also** change, then run `node tools/profile.mjs save`.

## Change ("change Sam's color", "add my mom as a speaker")

Find the choice's bullets by meaning and edit only the values asked for, in place.
Ask only the **Ask** questions the change needs (a new speaker needs a color). Check
**Conflicts** and **Also** again for the new values. Confirm in one line, then save.

## Remove ("remove the corner badge", "turn off quiet-voice")

Find the choice's bullets by meaning and remove exactly those. When a bullet mixes
the choice with something else, remove only the choice's part and keep the rest.
Leave **Also** changes in place (names in vocabulary still help transcription).
Confirm in one line ("Removed from style: corner badge."), then save.

## Make a reusable choice ("make my badge a reusable choice")

1. Find the bullets the user means in `profile/style.md`.
2. Draft `choices/<kebab-name>.md` in the format above. Every personal value (names,
   relationships, colors, labels, languages, channel names) becomes an **Ask**
   question with a generic default, or *required* when no default fits. The
   **Write** template keeps the user's phrasing with `{placeholders}` in their place.
3. Show the whole draft and write it only on a yes. Leave committing to the user.

## Scope

- Only the active profile changes. For another profile, switch first through the
  profile-manager skill (`.agents/skills/profile-manager/SKILL.md`), then run this.
- Never copy, move or delete profile folders yourself.
- Style requests that match no catalog choice ("bigger captions") are direk-ai notes,
  not this skill's.
