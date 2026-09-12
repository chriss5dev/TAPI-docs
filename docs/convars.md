# **Overview**

- The **Treason-API (TAPI)** creates console variables (ConVars) as a means to configure different behaviors present in TAPI.
- A large amount of these are directly related to [the TCR system.](tcr.md)

# **General**

#### `tapi_deathmatchmusic`
- Enables the `carend.wav` sound that plays at the end of carnage rounds if not set to 0.
- This enables plugin developers to play music at the end of rounds without the `carend.wav` sound clashing with the desired music.
- The workaround used to disable this sound has minor visual downsides, such as the winner name being `[REDACTED]`.
- To remedy this, the winner of annihilation rounds is instead printed into chat.

#### `tapi_keybind1`
- The key used when force-binding console command `tapi_action1` to all clients.
- I **STRONGLY ADVISE** against using common Treason inputs such as `W`, `F`, `G`, `Y`, etc.
- The default key is `6` for this exact reason.

# **Debug**

#### `tapi_cr_debug`
- Displays extra TCR debugging messages in the server console when set to 1.
- Primarily intended for custom role development.

# **Custom Roles**

#### `tapi_cr_min_traitor`
- The **minimum** amount of custom **traitor-roles** to consider assigning to traitors at round start, when possible.

#### `tapi_cr_min_innocent`
- The **minimum** amount of custom **innocent-roles** to consider assigning to innocents at round start, when possible.

#### `tapi_cr_min_solo`
- The **minimum** amount of custom **solo-roles** to consider assigning to innocents at round start, when possible.

#### `tapi_cr_max_traitor`
- The **maximum** amount of custom **traitor-roles** to consider assigning to traitors at round start, when possible.

#### `tapi_cr_max_innocent`
- The **maximum** amount of custom **innocent-roles** to consider assigning to innocents at round start, when possible.

#### `tapi_cr_max_solo`
- The **maximum** amount of custom **solo-roles** to consider assigning to innocents at round start, when possible.
- In any normal game, **you likely don't want this to be greater than 1.**
