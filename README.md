# Oura Ring V2: Data That Actually Coaches You

I’m tired of seeing Readiness scores that just sit there. An 85 is great, but what does it actually change about my Tuesday? This skill is my answer to that. It bridges the gap between Oura’s V2 usercollection API and your actual life.

This isn't just a data dump. It’s a tactical layer for your day. It pulls your metrics, calculates the momentum, and gives you a "Morning Brief" that tells you whether to sprint or recover.

## What it gives you
*   *The Morning Insight*: A sharp summary of your sleep architecture (Deep, REM, Efficiency) and a direct recommendation for your focus today.
*   *True Trend Analysis*: I don't just list scores. I track the deltas. Is your RHR spiking? Is your HRV dipping? I'll flag the anomalies so you can adjust your stress or diet before the crash.
*   *V2 Power*: Built on Oura’s latest usercollection endpoints. It’s faster and handles the pagination for you.

## Getting Started (The 2-Minute Setup)
1.  *Clone it*.
2.  *Token up*: Grab a Personal Access Token from [Oura's Cloud portal](https://cloud.ouraring.com/personal-access-tokens).
3.  *Hide the secret*: Create a `.env` file in the root and drop in `OURA_PERSONAL_ACCESS_TOKEN=your_token`.
4.  *Launch*: Run `bash scripts/morning_brief.sh`.

## The Philosophy
I built this because I wanted an assistant that knew when I was red-lining. If you use Clawdbot, this skill allows your agent to proactively suggest task rebalancing based on how you actually slept.
