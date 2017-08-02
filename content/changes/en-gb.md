---
title: Status API Limits
author_name: rsanheim
---

To ensure a high level of service for all API consumers, we will soon limit the number of [statuses]
to 1000 per commit SHA, repository, and context.

Beginning Monday, November 3rd, we will trim existing data sets that exceed this limit, deleting the oldest
records first. Attempts to create statuses beyond that limit will result in [validation error] a .

If you have any feedback or questions, please don't hesistate to contact us.
