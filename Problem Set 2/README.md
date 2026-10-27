# Econ 3916 Problem Set 2: The Survey That Wasn't Random

## Objective
I tested whether a product manager's claim, that satisfaction rose from 3.6 to 4.3 after an onboarding redesign, holds up once you account for who chose to answer the survey.

## What I did
- Simulated 50,000 users before and after the redesign, with a known true improvement of 0.10 points (3.291 to 3.391).
- Simulated a survey where happier users are more likely to answer, which reproduced the PM's numbers: 3.592 before and 4.309 after.
- Built bootstrap intervals by hand and with scipy, and checked their coverage in 400 repeated surveys.
- Ran a Welch's t-test and a power analysis, and simulated whether a bigger survey would help.
- Used AI to write an inverse-probability weighting correction, and checked it against the known truth.

## Key findings
- The survey showed a 0.72-point improvement, but the true improvement was 0.10. Most of the jump came from a change in who answered.
- The 95% intervals from respondents never contained the true mean (0% coverage), while the same intervals on a random sample covered it about 95% of the time.
- Surveying more people made the estimate more precise but not more correct: the bias stayed at about 0.61 from 1,000 to 50,000 users invited.
- Weighting for engagement and support tickets only moved the estimate from 4.31 to 4.14, against a true 3.39, because response depends on satisfaction itself.
- The fix is to show the same survey to randomly chosen users in both versions of onboarding, so any response bias cancels out in the comparison.
