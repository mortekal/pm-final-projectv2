# A/B Experiment Brief, Riverty BU Collection, A Fair Way Forward

## Parameters
| Parameter | Decision |
|---|---|
| Feature under test | Affordability check before the plan options. |
| Persona | A financially stretched consumer with one or more open claims who is anxious about making a mistake, and who wants a payment plan they can actually sustain. |
| Expected outcome | Consumers choose plans that match what they can pay, so fewer agreements break within 60 days. |
| Primary success metric | Plans kept per journey starter: agreements without a missed payment within 60 days, divided by journey starters. |
| Baseline rate | 37.3% (Month 6). It stayed between 36.5% and 37.6% across Months 1 to 6, so it is stable. I calculated it as agreements × (1 − missed-payment rate) ÷ starters from the data pack. |
| Guardrail metric | Agreement rate |
| Guardrail boundary | Not drop lower than 46.0% (Month 6). |
| Second guardrail | · |
| Minimum Detectable Effect | +1,5% |
| Sample size per arm | About 16,450 users per arm. |
| Traffic split | 50/50, randomized per consumer |
| Test duration | 4 months |
| Significance threshold | p < 0.05, two-sided, with no deviation |

## Control vs. Variant
- **Control (A):** The plan list shows the 6-month plan first, with no guidance on what each installment means for the consumer’s income. N-02 picked the shortest term because it “looked like the right answer” and missed the second payment. In Month 6, 19.0% of agreements missed a payment within 60 days (up from 15.6% before the 6-month plan went first), leaving plans kept per starter at 37.3%.
- **Variant (B):** An affordability check on the plan screen. Using the income already entered, each plan shows its installment as a percentage of income and a fit label: Fits (10% or less), Stretch (11% to 20%) or Likely too high (above 20%). Plans are sorted by fit with the shortest Fits plan on top. Nothing is pre-selected or blocked.
- **Held constant (isolation check):** The income question and field, and the income document step.
Plan terms, installments and the total (6, 12 and 24 months, same amounts).
Eligibility rules, agreement flow and payment collection.
Letters, notifications and the chat channel.
Agent tooling.
Randomization per consumer at journey start, with both arms running at the same time.

## Hypothesis
> I believe that Affordability check before the plan options. for A financially stretched consumer with one or more open claims who is anxious about making a mistake, and who wants a payment plan they can actually sustain. will result in Consumers choose plans that match what they can pay, so fewer agreements break within 60 days., as measured by a +1,5% change in Plans kept per journey starter: agreements without a missed payment within 60 days, divided by journey starters. within 4 months. We will protect Agreement rate throughout the test.

## Shipping criteria
> We will **ship** if Plans kept per journey starter: agreements without a missed payment within 60 days, divided by journey starters. improves by ≥ +1,5% at p < 0.05, two-sided, with no deviation and Agreement rate does not reach Not drop lower than 46.0% (Month 6). after 4 months.
> We will **iterate** if direction is positive but lift is below the MDE.
> We will **kill** if the primary metric shows no improvement or moves negatively.
> The read date is fixed at the end of 4 months, no results reviewed before this date.
