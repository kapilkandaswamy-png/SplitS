# SplitSave Build Spec

This project implements the supplied SplitSave specification. The original requirements are preserved in the project README and the Supabase migration.

Source requirements included:
- personal expense tracking
- group shared expenses
- equal/exact/percentage splits
- balances and simplified settle-up suggestions
- manual settlements
- savings goals and contributions
- weekly streaks, points and badges
- opt-in group leaderboard based on progress percentage and streak
- server-side authorization via Supabase RLS
- integer minor-unit money storage
- mobile-first responsive UI
- accessibility basics
- minimal dependencies
- no payments/bank integrations/multi-currency in v1

See `README.md` for setup and deployment and `supabase/migrations/001_init.sql` for the database security model.
