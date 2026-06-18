CLAUDE.md (Website Factory OS)

WHAT THIS REPO IS:
This is the Website Factory OS. It turns a business into a launch-ready website through a
chain of agents. Each agent does one job, reads the file the previous agent made, and writes
its own. These are the house rules. Every agent in this repo follows them, on every run, no
matter which job it is doing.

FOLDER MAP:
- agents/    The reusable tools. One file per agent (research, strategy, copy, build spec).
- projects/  The work, one folder per client. Each agent saves its output here under the
             client's folder.
- runner.md  How a run is driven.

THE NON-NEGOTIABLE HOUSE RULES (every agent obeys these):

1. NEVER FABRICATE.
   Do not invent a fact, a price, a name, a number, a quote, a review, a rating, a credential,
   a source, or a statistic. If a fact is not verified or not supplied, do not make one up.
   Use a clearly marked [OWNER TO SUPPLY: ...] placeholder, tag it [UNVERIFIED], or leave it
   out and name the gap. A labeled gap is always better than a confident wrong answer.

2. VERIFY AND SOURCE.
   When you state a real-world fact (a competitor, a price, a figure, an endorsement), it must
   come from a source you actually checked, and you cite it. Prefer primary sources over
   aggregators. If you have no web access this run, say so and do not name specific facts.

3. LEAD FROM THE STANDPOINT (CULTURAL LENS).
   Do not treat the dominant culture as a neutral default. Name the real audience's cultural
   context in every section. When the business serves a Black community, or any specific
   community, center it fully: the real trust signals, language, and networks, not generic
   data. When mainstream data thins out or erases that community, say so and name the gap
   instead of filling it with a generic average. The lens shapes every section. It is never a
   bolt-on at the end.

4. KEEP FACT AND ANALYSIS SEPARATE.
   A fact is a verified real-world claim and needs a source. Analysis is your reasoning,
   interpretation, or recommendation. Label them so the reader can always tell which is which.

5. DO ONE JOB AND STOP.
   Each agent does only its own job. Read the input file in full first. If the required input
   file does not exist, STOP and say so, do not invent it. Save your output to
   projects/{project-name}/, then STOP. Never run the next phase unless you are explicitly
   told to. Never chain phases on your own.

6. WRITE PLAINLY.
   Be structured, not verbose. No fluff, no empty marketing language. Every line must be usable.

These rules sit above every agent file. If an agent file ever conflicts with these, these win.
