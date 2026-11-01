# One pass. Every age rating.

In summer 2026 I started building [DCProof](<https://www.forret.com/portfolio/dcproof/index.md>), a toolkit for the DCP workflows of sales houses.

But the market kept asking a different question: is this trailer or this ad fine for a 6-year-old, a 12-year-old, or only for adults?

Today that answer comes from a rating agency that doesn't explain itself, or from gut feeling.

A cartoon sword fight and a realistic stabbing are both "violence". They land in very different buckets.

[![One pass, every age rating: watch, listen, map, explain, learn](<https://www.forret.com/blog/posts/img/one-pass-every-age-rating.png>)](<https://www.forret.com/blog/posts/img/one-pass-every-age-rating.png>)

## Watch, listen, map

So age rating became the focus, built around one method, "watch, listen, map":

1. A **video model** watches the whole clip and returns timestamped observations.
2. A **speech model** transcribes the dialogue for language checks.
3. A **language model** maps each observation onto Kijkwijzer, FSK, BBFC and MPAA criteria.

Claude Code built the whole workflow with me: the model calls, the timecodes that tie picture and dialogue together, the mapping onto each rating system.

## Keeping it right is the work

But the first rating is the easy part. Keeping it right is the work.

So every rating system has its own evals: reference trailers and ads with a known rating. Markets judge differently: swearing that earns a Kijkwijzer language pictogram won't raise a Swedish rating. So the mapping prompt adapts per market, and the evals run again. Claude Code runs that loop with me: adjust, rerun, compare.

Two years ago this would have been a research project. This summer it became three models and a working prototype.

## No black box

The part I am proudest of: none of it is a black box. Every rating comes with what was detected, where, and why it lands in that bucket for that system.

One pass, every rating, and a reason for each.

How does your team rate video today: an agency, gut feeling, or nothing? [Join the conversation on LinkedIn](<https://www.linkedin.com/feed/update/urn:li:activity:7518921701100015616/>)
