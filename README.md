# Raidflag keeper scheduler

This repository contains only the schedule that calls Raidflag's authenticated keeper and checks service health. Application code and wallet keys are stored elsewhere. No checkout, third-party actions, artifacts or caches are used.

The workflow uses a standard Ubuntu runner in a public repository, which is free under [GitHub Actions billing](https://docs.github.com/en/billing/concepts/product-billing/github-actions).

Configuration:

- Variable `RAID_SITE_URL`: HTTPS deployment origin, with no path.
- Variable `RAID_KEEPER_ENABLED`: `true` to enable scheduled executions.
- Secret `RAID_CRON_SECRET`: keeper endpoint credential, not a wallet key.
- Optional secret `VERCEL_AUTOMATION_BYPASS_SECRET`: required when targeting a protected preview. Remove when switching to the public production origin.

Manual dispatch also works while the schedule is disabled. Check the Actions run result after changing configuration. A successful run checks both the keeper result and service health; operational response bodies are not printed in public logs.

The schedule requests execution every five minutes, offset from the top of the hour. GitHub schedules are best effort and can be delayed or dropped. Public repository schedules are disabled after 60 days without repository activity. The operator must monitor failures, check the last successful run and keep scheduling enabled; this is not a guaranteed availability service. See [GitHub schedule limitations](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule).

To stop automated executions, set `RAID_KEEPER_ENABLED=false` or disable the workflow in Actions. Vercel's separate daily cron is unaffected.
