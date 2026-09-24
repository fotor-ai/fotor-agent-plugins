---
name: fotor-credits
description: Check available Fotor credits or open its signed-in top-up page. Use for Fotor balance and recharge requests; website connection belongs to fotor-connect and media tasks use their own skills.
---

# Fotor Credits

Follow the shared [credits and top-up workflow](../../references/credits.md). It covers authentication, balance versus task consumption, conditional navigation, and errors.

Report the actual balance. A zero balance or explicit top-up request selects the recharge entry; a positive balance query alone ends with the balance. Respect an explicit no-browser or balance-only/no-web instruction. The tool returns `credits` and `top_up_url`, with no `expires_in` field.

Keep payment under the user's control. Neither a returned link nor an opened page establishes that a purchase or recharge completed.
