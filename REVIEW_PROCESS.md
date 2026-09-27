# Review Process

Version 3.0. Updated 20 September 2026.

How to review a project against DESIGN_PRINCIPLES.md and REFERENCE_ARCHITECTURE.md. Read both first. Deviation record formats are in PROJECT_TEMPLATE.md.

## Method

1. **Set the class.** Check the declared class against the class questions. If none is declared, or it fails the questions, use the class the questions give and say why. Unsure means Shared.
2. **Score the gates first.** N1 to N7 and H1 to H3, each pass, fail, not applicable, or unverified. Silence in the description is unverified: say what you would need to see. A stated absence, such as no restore test, is a fail. One fact can fail more than one gate. Report it under each.
3. **Assess the relevant principles.** Skip the irrelevant ones and say so.
4. **Judge each deviation.** Is it justified and recorded? Do not mark down a justified deviation.
5. **Look for excess.** Over-specification, needless infrastructure, duplicated capability, engineering gymnastics.
6. **Look for gaps.** Any important concern these documents miss.
7. **Check the documents against reality.** If REFERENCE_ARCHITECTURE.md disagrees with what runs, or the state document is missing or stale, report it and carry on.
8. **Pick the top three changes, ordered by harm.** First exposure or leaked secrets (N1, N2). Then risk to data (N3, N4, N5, H1 to H3). Then harm to people (N6, N7). Then the rest. Unverified gates are questions to answer, not findings, unless a failure would be severe (N1 or N2).
9. **Challenge once.** If a principle would make the product worse, say so (principle 10). Do not repeat a challenge already recorded.

Lead with what is wrong or at risk. Say what is right afterwards, briefly.

## Output

For Lab, use only sections 1, 2, 3, and 8.

1. **Bottom line.** Two or three sentences: sound, at risk, or unsafe to release.
2. **Class.** The class, and whether it is set correctly.
3. **Gates.** Pass, fail, not applicable, or unverified for N1 to N7 and H1 to H3, with one line of evidence each.
4. **Deviations.** Deviation, principle affected, justified, recorded, evidence.
5. **What conforms.** Short.
6. **Excess.** Anything over-built.
7. **Missing concerns.** Anything the documents do not cover.
8. **Top three changes.** In order, each with the reason.
9. **Challenges.** New ones only.

## Easy to miss

- Does each Mac service meet the Server Manager contract, and is it in the port register?
- Is the backup register complete for this project's data?
- Is the state document current?
- Was the smoke test run on real or realistic data, and did someone look at the output?
- Does anything run only from a terminal window nobody watches?
- For a Shared project, has the data protection review been done or started?
