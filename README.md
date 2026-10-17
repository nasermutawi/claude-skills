# Claude skills

One place for the skills I use across projects. Each folder under `.claude/skills/` is one skill. Attach this repo to a Claude session (or open it in Claude Code) and they all load.

## What's here

| Skill | What it's for | Source |
| --- | --- | --- |
| ui-ux-pro-max | UI/UX rules, palettes, fonts and checks, searchable | Added October 2026 |
| google-apps-script | Apps Script server code: sheets, web apps, triggers, locks, quotas | Built with Skill Seekers from Google's samples, then written up by Claude (see its `SOURCES.md`) |
| emil-design-eng | UI polish and the small details that make software feel great | [emilkowalski/skills](https://github.com/emilkowalski/skills) (MIT) |
| apple-design | Apple-style gestures, springs, sheets, typography, for the web | emilkowalski/skills |
| mobile-native | Make a web app feel native on a phone | emilkowalski/skills |
| animate | Build an animation the right way | emilkowalski/skills |
| review-animations | Strict review of motion code (only when asked) | emilkowalski/skills |
| improve-animations | Audit all the motion in an app and plan fixes | emilkowalski/skills |
| find-animation-opportunities | Spot where motion would help, and where it wouldn't | emilkowalski/skills |
| animation-performance | Fix janky or dropped-frame animations | emilkowalski/skills |
| animation-vocabulary | Name a motion effect you can only describe | emilkowalski/skills |
| break-ui | Stress-test a screen with worst-case data | emilkowalski/skills |
| prototype | Several versions of a UI piece behind a picker (only when asked) | emilkowalski/skills |
| pick-ui-library | Choose a frontend library (only when asked) | emilkowalski/skills |
| ask-sonner | The Sonner toast library for React | emilkowalski/skills |
| animate-expo | Animations in React Native / Expo (for a future native app) | emilkowalski/skills |
| write-swift | Modern Swift (for a future native iPhone app) | emilkowalski/skills |
| impeccable | Full design toolkit: critique, audit, polish, bolder, quieter, typeset, layout, delight, harden, onboard; also native iPhone and Android screens and design-system files. Its command-line tool downloads a program from the author's GitHub on first use (only needed for automated scans and live mode) | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) (Apache 2.0) |
| taste-skill | Art direction for websites and landing pages, from a study of 247 award-winning sites. Not for app screens | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) (MIT) |

### Business logic and tests

From [meirm/reverse-engineering-skill](https://github.com/meirm/reverse-engineering-skill) (MIT). For requirements and tests, not design.

| Skill | What it's for |
| --- | --- |
| bdd-orchestrator | Runs the whole flow below, step by step, for one feature |
| tailor-bdd-skills-for-project | Sets the other skills up for a repo: where the business-logic docs live, the terms it uses |
| reverse-engineering-business-logic | Works out what the code actually does: rules, workflows, states |
| validate-business-logic-against-code | Checks written rules against the code: implemented, partly, contradicted or missing |
| analyze-business-logic-gaps | Finds missing, vague or contradictory rules |
| refine-business-logic-for-implementation | Rewrites vague rules into exact, testable ones |
| derive-acceptance-criteria-from-business-logic | Turns rules into Given/When/Then acceptance criteria and tasks |
| generate-tests-from-business-logic | Turns rules into test cases |

## Adding skills

- **From another repo:** copy its skill folders into `.claude/skills/`, keep its licence file in each folder, and add a row above.
- **From docs, a repo or a PDF:** use [Skill Seekers](https://github.com/yusufkaraaslan/Skill_Seekers) on your computer (`pip install skill-seekers`, then `skill-seekers create <source>` and `skill-seekers enhance <folder> --agent claude`), check the result, and copy the folder here.
- Read a third-party skill before adding it: skills are instructions Claude follows.
