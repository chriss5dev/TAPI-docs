# **Overview**

- The **Treason-API (TAPI)** creates global forwards that allow other plugins to react to common events.
- These global forwards mostly consist of events from [the TCR system.](tcr.md)
- Non-TCR-related global forwards are typically made to fill in for non-existent game events in Treason.

# **Game Utilities**

#### `OnTreasonRoundEnd`

    // Called when the current round ends.
    public void OnTreasonRoundEnd(int winner, int reason) {}

# **TCR Utilities**

#### `OnRegisterCustomRoles`

    // Called during round start, when TCR asks custom-role plugins to register their custom roles.
    // Typically used in custom-role plugins, containing the `RegisterCustomRole` native function.
    public void OnRegisterCustomRoles() {}
    
#### `OnSoloStoppedRoundEnd`

    // Called when a client with a solo (independent) custom-role prevents the round from ending normally.
    
    // For example:
    // - Imagine an alive player with the solo-based custom-role "Lone Wolf".
    // - If all the traitors are dead, the innocents should win... right?
    // - Wrong! The `TR_Solo` / `TRole_Independent` player prevents the round from ending!
    //
    // In this hypothetical situation, `OnSoloStoppedRoundEnd` is called,
    // allowing for the "Lone Wolf" developer to:
    // - Check the provided client's customRoleIndex ()
    // - If it is the "Lone Wolf" index, display a message "The Lone Wolf is still alive!".
    
    public void OnSoloStoppedRoundEnd(int client) {}

#### `OnSoloWin`

    // Called when a client with a solo (independent) custom-role wins the round.
    public void OnSoloWin(int client, int customRoleIndex) {}

#### `OnClearCustomRoles`

    // Called when the TCR system clears the array of custom roles, typically at round end/start.
    // Useful for clearing temporary variables such as `g_iMyCustomRoleIndex` or `int loneWolfClient`
    public void OnClearCustomRoles() {}

#### `OnClientAssignedCustomRole`

    // Called when the TCR system assigned a custom role to a client.
    public void OnClientAssignedCustomRole(int client, int customRoleIndex) {}
