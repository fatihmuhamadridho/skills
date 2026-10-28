# config.toml

```md
[features]
memories = true

[agents]
max_concurrent_threads_per_session = 6

[memories]
generate_memories = true
use_memories = true

# Pertimbangkan percakapan hingga 90 hari

max_rollout_age_days = 90

# Memory yang tidak digunakan tetap eligible hingga 365 hari

max_unused_days = 365

# Perbesar jumlah raw memories untuk konsolidasi

max_raw_memories_for_consolidation = 1024

# Proses lebih banyak percakapan per startup

max_rollouts_per_startup = 10

# Minimum idle sebelum diproses

min_rollout_idle_hours = 6
```
