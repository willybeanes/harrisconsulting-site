# Building the xR Philosophy Bot: Expected Runs, RE24, and What the Score Doesn't Tell You

*Published 2026 · Baseball Data · 10 min read*

---

Every MLB game ends with a final score. That score tells you who won. It tells you almost nothing about how the game was actually played.

Two teams can both score four runs and have played completely different games — one grinding out walks and manufacturing runs through good process, the other getting bailed out by a solo home run after stranding six baserunners. The score looks the same. The underlying quality of play is not the same.

That gap between what happened and what the game *deserved* to produce is what the xR Philosophy project is built around.

---

## What Is xR?

xR stands for Expected Runs — specifically, runs calculated through RE24 (Run Expectancy based on 24 base-out states).

The concept behind RE24 is elegantly simple. At any point in a baseball game, there are 24 possible states: eight base configurations (bases empty, runner on first, runner on second, and so on) combined with three out states (zero, one, or two outs). For each of those 24 states, we can calculate — based on decades of historical data — how many runs a team is *expected* to score before the inning ends.

RE24 tracks how individual plate appearances and plays change that run expectancy. A walk with the bases loaded in a two-out situation adds significant expected run value. A strikeout to lead off an inning costs less. Every event in a game shifts the run expectancy meter, and the cumulative sum of those shifts — anchored to actual run scoring — gives you xR: a picture of how many runs a team *earned* through productive play, as opposed to how many happened to cross the plate.

The difference between xR and actual runs scored is telling. Teams that consistently outscore their xR are often benefiting from sequencing luck — hits clustering together more than their rate would predict. Teams that underperform their xR are frequently better than their run totals suggest.

---

## The Technical Stack

The xR Philosophy project runs on three components: a data pipeline, a calculation engine, and a publishing layer.

**Data: The MLB Stats API (GUMBO)**

MLB's Stats API — colloquially called GUMBO — is the same data source that powers most serious baseball analytics work. It provides play-by-play data at a granular level: every pitch, every event, every base state before and after each plate appearance.

Working with GUMBO isn't always straightforward. The API is well-documented in some areas and opaque in others. One of the more significant debugging challenges in building xR was an off-by-one error in how out states were being read from the feed — a parsing issue that produced subtly wrong run expectancy figures until we isolated exactly where the state transition was being captured (before the play recorded, vs. after).

The lesson: always verify your out-state counts against known game logs before trusting your RE24 numbers. A single-out error compounds across every state transition in a game.

**Calculation: True RE24**

The RE24 table we use is derived from multi-year historical MLB data — the average runs scored from each of the 24 base-out states across a large sample of games. Applying this table to live play-by-play data means calculating, for each plate appearance: the run expectancy of the state entering the play, the run expectancy of the state exiting the play, and any runs that actually scored. The delta, summed across a game, gives you xR.

Getting this right required careful attention to edge cases: double plays, fielder's choices, baserunner outs on the basepaths, and plays where multiple runners score. Each of these requires correctly identifying the pre- and post-play state and crediting or debiting the appropriate RE24 values.

**Publishing: GitHub Actions + Bluesky**

Once the calculation layer was working, the publishing layer was the fun part. The bot runs on GitHub Actions — a free CI/CD platform that lets you schedule code to run on a cron schedule without maintaining any server infrastructure.

After each day's games complete, the pipeline pulls the previous day's play-by-play from GUMBO, calculates xR for every game, formats a post for each result, and publishes to Bluesky via their AT Protocol API. The post format shows the final score alongside each team's xR — giving followers an immediate read on whether the result reflected the underlying play.

The GitHub Actions setup means the entire pipeline runs for free, on a schedule, with no ongoing maintenance overhead. For a side project with no monetization, this is the right architecture.

---

## What the Data Actually Shows

After running the bot through a full season, a few patterns emerge consistently.

**High-offense games mask xR divergence.** When both teams score a lot of runs, it's easy to assume the game was a slugfest driven by good hitting. Often, it's a game where one or both teams had significant sequencing luck — runners happened to reach base in the right order. Their xR is frequently lower than actual runs scored.

**Pitching-dominated games are more xR-accurate.** Low-scoring games tend to produce xR figures closer to actual run totals. Fewer baserunners means fewer opportunities for sequencing to matter.

**The most interesting games are the divergent ones.** A team that scores two runs but posts an xR of 4.8 played a significantly better game than the score suggests. Their pitcher stranded runners; their hitters hit the ball hard in tough spots. They deserved better. Surfacing that story — that 5-2 loss that wasn't really a 5-2 loss — is exactly what xR is designed to do.

---

## Why Build This at All?

Honestly, because the score isn't enough.

I've been a baseball fan long enough to have sat through games where my team lost and felt like they played well, and games where they won and felt uneasy about it. xR gives a vocabulary and a framework for that intuition. It turns "we played better than the score showed" from a feeling into something you can point to.

The project also gave me a real-world context to push through some genuinely tricky technical work: GUMBO API parsing, RE24 table construction, out-state edge cases, and GitHub Actions scheduling. Building something you actually care about makes that debugging less tedious.

There's also something philosophically satisfying about a bot named "xR Philosophy" posting expected run data to Bluesky at 2am. It feels appropriately earnest for a project about caring a little too much about baseball.

---

## What's Next

A few things I'm looking at adding:

**Season-long xR tracking.** Daily game posts are useful, but cumulative xR vs. actual runs over a full season tells a richer story about which teams are over- and underperforming their true quality.

**W%∆ correlation.** Pairing xR data with win probability shifts — how much did each team's actions actually move their probability of winning — would add another layer of context beyond raw run production.

**Catch probability integration.** I've done separate work scraping catch probability data from Baseball Savant, including reverse-engineering the hidden 0-star play columns that don't surface through the standard interface. Connecting that defensive data to the offensive xR picture would get closer to a full game quality metric.

If you follow baseball analytically and want to see the bot in action, it's running on Bluesky. If you're building something similar and want to talk through the RE24 implementation, I'm always happy to get into the weeds.

---

*Will Harris is an independent consultant and lifelong baseball analytics enthusiast. He can be reached at [will@harrisconsulting.us](mailto:will@harrisconsulting.us).*
