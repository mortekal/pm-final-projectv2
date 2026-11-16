# Affordability check before the plan options, Simplified PRD (Riverty BU Collection, A Fair Way Forward)

**Scenario:** Riverty BU Collection, A Fair Way Forward

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Financially stretched consumer with open claims

## 1. The Big Picture
- **Vision:** Every financially stretched consumer who starts an installment plan online sees which plans fit what they can pay before choosing, so the plan they pick is one they can keep
- **Press release:** Riverty Collection helps stretched consumers choose an installment plan they can keep. Today a consumer with open claims sees a list of plans with no sense of what they can afford, and picks the shortest term because it "looked like the right answer" (N-02). That consumer missed the second payment. In the sample data pack, missed payments within 60 days rose from 15.6% to 19.0% after the 6-month plan was shown first, while plans kept per journey starter stayed flat at 37.3%.
- **Success metric:** Plans kept per journey starter
- **Guardrail:** 37.3%

## 2. The Details
### User stories
- As a financially stretched consumer with an open claim, I want to see each plan's installment against my income before I choose, so that I don't pick a plan I can't pay .
- As an anxious consumer, I want to know why my income is asked and to correct it, so that I'm not afraid of typing the wrong number.
### Screens to build
- Screen 1: Confirm your income (entry point)
- Screen 2: Plans that fit (feature core)
- Screen 2: Plans that fit (feature core)
- • Header showing the income used, with an Edit link that reopens the field inline.
- • Multi-claim banner, shown only with two or more open claims.
- • Three plan cards (6, 12 and 24 months). Each shows term, installment, total amount, the installment as a percentage of income, and a fit label with an icon and text so colour is never the only signal.
- • Choose this plan button on every card; no card is pre-selected.
- • No-income state: neutral cards with no labels and the link "Add income to see what fits".
- • All-too-high state: a short message above the list and no highlighted card.
- Screen 3: Plan confirmed (success)
### Functional requirements
- 1. Single input. The check uses one whole-number monthly income field in euros, between 1 and 99,999. It adds no other question and no document upload.
- 2. Fixed rule. For each plan, ratio = installment ÷ monthly income, rounded to the nearest whole percent. Fits is 10% or less, Stretch is 11% to 20%, and Likely too high is above 20%. The values are constants at the top of the file.
- 3. Label on every plan. Each plan shows its installment, term, percentage of income and exactly one fit label, in text plus icon.
- 4. Fit-driven order. Plans are sorted Fits, then Stretch, then Likely too high. The top plan is the shortest term among Fits. No plan is pre-selected. If every plan is Likely too high, sort by lowest installment and highlight none.
- 5. Guide, never block. All three plans stay visible and selectable in every state.
### Smart behaviors (Situation → Outcome)
- Income entered, at least one plan Fits
- Fits plans first, shortest fitting plan on top, then Stretch, then Likely too high. Nothing pre-selected.
- Income entered, no plan Fits
- Order by Stretch, then Likely too high, lowest installment first. Message: "None of these plans fit comfortably. You can still choose one, or call us on [placeholder number]." No card highlighted.
- Income skipped, blank or zero
- Neutral cards, no labels, no fit ordering, link "Add income to see what fits".
- Income edited on the plan screen
- All percentages, labels and order recompute immediately from the current value.
- Two or more open claims
- Caveat banner above the list with the other-claim count.
- Income above 10,000 a month (placeholder)
- Ask "Is this per month?" before computing, so an annual figure cannot produce false Fits labels.
- Consumer picks a Likely too high plan (Should have)
- Inline line: "This installment is {x}% of the income you entered. You can still choose it." Selection proceeds on a second tap.
- Consumer taps Change plan on Screen 3
- Return to Screen 2 with income and claim count kept.
### Technical constraints
- • No external APIs, no credit-bureau, open-banking or income-lookup calls.
- • No login, accounts or real consumer data. All data is mock and hardcoded.
- • One HTML file with plain JavaScript and inline CSS. No frameworks, no build step, no backend.
- • No persistence. State lives in memory and resets on reload.
- • No ML or scoring, and no generative text. Every label and message is a fixed string chosen by the rule.
- • Works at 375 px and at desktop width, and every control is keyboard reachable.

## 3. The Logistics
### Features out
- • Hard blocks or auto-rejection of any plan.
- • Credit-bureau data, open-banking income pulls or ML scoring (they conflict with N-06 and N-12, and portfolio data is uneven per N-11).
- • Income document verification as a precondition (B3's scope).
- • Combined or cross-claim plans (B4).
- • The agent handoff view (B6).
- • Pay-day date picking (B5).
- • New questions beyond the existing income step, such as an expenses or hardship questionnaire.
- • New plan terms or custom "pay what you can" amounts.
- • Fixing the online-versus-letter amount mismatch (N-07).
### Edge cases & safety guard
- Edge Cases
- • Blank, zero or non-numeric income. Show an inline error: "Enter a monthly amount in euros, or skip." Skip leads to the neutral no-label state.
- • Annual figure entered as monthly. Above 10,000 a month (placeholder), ask "Is this per month?" before any label is computed. This stops a false Fits label for a shift worker who types a yearly figure (N-01).
- • Every plan Likely too high. Show the message from Smart Behaviors, order by lowest installment and highlight nothing. Never present the least bad plan as recommended.
- • Income edited several times. Labels recompute from the current value only, so a stale label can never remain on screen.
- • Three or more claims. The banner shows the exact count of other open claims. The check still covers this claim only.
- • Back navigation from the confirmation screen. Income and claim count are kept, and no plan is pre-selected on return.
- • Safety guard (no invented advice). The prototype contains no generative text. Every label and message is a fixed string chosen by the rule. Copy says a plan "fits the income you entered" and never states that the consumer can afford it. The balance and installment come from one constant, so Screen 2 and Screen 3 can never show different amounts (N-07).
### Decision log
- 1. Guide, don't block. All plans stay selectable. The agreement-rate guardrail of 44.2% equals the pre-change Month 3 level, so a block risks breaching it. Trade-off: some consumers will still pick a Likely too high plan, and the pilot has to show that plans kept and the missed-payment rate still improve.
- 2. One input and a fixed rule, no external data or scoring. The check uses the income already entered and a ratio test that an agent and the consumer can both be told in a sentence (N-12). It avoids new data collection that would feed N-06's fear, and it fits one sprint for 1 PM, 1 senior BA, 4 engineers and 1 QA. Trade-off: labels are coarse, and the thresholds stay placeholders until compliance and the BA set them.
### Evals
- Rule accuracy
- Time-on-task and comprehension
- Safety triggers

## MoSCoW scope
- **Must:** Reuse the income already entered at the affordability step as the only input; A rule-based installment-to-income test for each plan option
- **Should:** Variable-income handling
- **Could:** A what-if adjuster
- **Won't (now):** Hard blocks or auto-rejection of any plan

