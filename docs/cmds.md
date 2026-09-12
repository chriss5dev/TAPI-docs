# **Overview**

- The **Treason-API (TAPI)** creates console commands.
- Most of these are intended for experimental use by developers.

# **General**

#### `tapi`
- Displays the current version of TAPI in the console.

#### `tapi_int`
- Displays the current version of TAPI in the console as a 6-digit integer. (From native function `TAPI_Version`)

#### `tapi_action1`
- A command that is automatically force-bound to all clients using `tapi_keybind1`.
- Intended to be registered by custom role plugins that wish to automatically bind an action to a player's keyboard.

> Note that the default `tapi_keybind1` is `6`.

# **Treason Tools**

#### `tapi_getability`
- Prints the ability ID of a given slot from the calling player.

#### `tapi_getgadget`
- Prints the gadget ID of a given slot from the calling player.

#### `tapi_getroleid`
- Prints the role ID of the calling player.

#### `tapi_getrole`
- Prints the result of `GetClientRole` on the calling player.

#### `tapi_getzombie`
- Prints the current zombie state of the calling player.

#### `tapi_setzombie`
- Sets the current zombie state of the calling player.

#### `tapi_getkarma`
- Prints the current karma value of the calling player.

#### `tapi_setkarma`
- Sets the current karma value of the calling player.

# **TCR Tools**

#### `tcr_list`
- Lists all of the custom roles currently registered by TCR.
- Aliases:
    - `tapi_listcr`
    - `sm_listcr` *(!listcr)*
#### `tcr_get`
- Lists all of the custom roles currently registered by TCR.
- Aliases:
    - `sm_getcr` *(!getcr)*

#### `tcr_set`
- Lists all of the custom roles currently registered by TCR.
- Aliases:
    - `sm_setcr` *(!setcr)*
