<style>
	h4 {
		font-weight: bold;
	}
</style>

# **Overview**

- The **Treason-API (TAPI)** creates native functions for invoking or manipulating various Treason-related behaviors.
- These functions serve as a large abstraction layer for complex underlying behaviors such as:
    - Build-specific details
        - Offsets
        - Function Signatures
    - Fully-custom behavior-management systems
        - Custom role system
        - Pseudoname overrides
    - Semantic translation layers
        - `TR_Ghost` and `TR_Solo` of `treasonRole` enumeration
        - `GetClientRole` filtering out spectators and dead players.

# **TAPI Utilities**
*"Treason-API"*

#### `TAPI_Version`

    // Returns the current version of the TAPI plugin as a 6-digit integer.
    native int TAPI_Version();
    
# **TCR Utilities**
*"Treason Custom Roles"*

## Notice
- I **strongly** advise **all developers** understand [the TCR system](https://github.com/chriss5dev/Treason-API) before using these functions.

#### `ClearCustomRole`

    //Clears the id of a CustomRole using its custom role index.
    //This effectively "deletes" a custom role from the internal custom roles array by marking it as invalid.

    //@param customRoleIndex	The target custom role index.
    native void ClearCustomRole(int customRoleIndex);

#### `GetCustomRoleIndex`

    //Finds the CustomRole index of a CustomRole by searching for a string id.
    //Returns -1 if no CustomRole can be found.

    //@param id					The string id to search for.
    //@return					The resulting CustomRole index.
    native int GetCustomRoleIndex(char[] id);

#### `GetCustomRoleID`


    //Gets the CustomRole.id (string) of the given custom role index.
    //Returns false if the custom role index is invalid.
    //Returns true on success.

    //@param customRoleIndex	The target custom role index.
    //@param result				The a buffer to store the string ID of the target custom role index. MaxSize is 32.
    //@return					Returns false if the custom role index is invalid.
    native bool GetCustomRoleID(int customRoleIndex, char[] result);

#### `GetCustomRoleIsConfirmed`

    //Gets the CustomRole.isConfirmed (bool) of the given custom role index.
    //Additionally returns false on failure.

    //@param customRoleIndex		The target custom role index.
    //@return					The value of isConfirmed, assuming the check didn't fail.

    native bool GetCustomRoleIsConfirmed(int customRoleIndex);

#### `SetCustomRoleIsConfirmed`

    //Sets the CustomRole.isConfirmed (bool) of the given custom role index.

    //@param customRoleIndex	The target custom role index.
    //@return					Returns true if successful.

    native bool SetCustomRoleIsConfirmed(int customRoleIndex, bool state);
    
#### `RegisterCustomRole`
Discover documentation and usage details at [The TCR System.](tcr.md)

# **Game Utilities**

## - Find Clients -

#### `GetDetectiveIndex`

    //Returns the client index of the Detective client.
    //Returns 0 if no detective can be found.
    //"Client 0" errors will occur if ignoring the "no detective" case above.
    //Depends on GetClientRole().

    //@return					The index of the round's Detective if one can be found. 0 if none can be found.

    native int GetDetectiveIndex();
    
#### `GetDoctorIndex`

    //Returns the client index of the Detective client.
    //Returns 0 if no doctor can be found.
    //"Client 0" errors will occur if ignoring the "no doctor" case above.
    //Depends on GetClientRole().

    //@return					The index of the round's Doctor if one can be found. 0 if none can be found.

    native int GetDoctorIndex();

## - Round Data -

#### `GetIsCarnage`

    //Gets the "iscarnage" boolean of "round_start", without having to hook the event and store it manually.
    //Returns true if it is currently Annihilation.
    //Returns false if it is not.

    //@param includePreRound	If true, this function will also return true during the pre-round state, before the Annihilator role is displayed. ("Preparing...")
    //@return					Returns the current state of Annihilation, as described above.

    native any GetIsCarnage(bool includePreRound);
    
## - Round Management -

#### `ForceEndRound`

    //Attempts to force the round to end given custom conditions.
    //Resolves the custom conditions into more complex sets of event keys, then uses a deferred SDKCall to end the round.
    //*Note the existence of ConVar "tapi_deathmatchmusic".

    //@param endCondition		The desired endCondition, as a treasonEndCondition or an int.
    //@param winner				The desired winning team or client index, depending on the endCondition.
    //@return					Returns true on success, or false on failure.

    native any ForceEndRound(any endCondition, int winner);

# **Client Utilities**

## - Karma -

#### `GetClientScoreboardKarma`

    //Returns the scoreboard karma value of a client index as an integer.
    //Returns -1 if PlayerResourceEntity is invalid.
    
    //@param client				The target client index.
    //@return					The Karma value of the provided client, taken from the netprop supplying the scoreboard.
    native int GetClientScoreboardKarma(int client);

#### `GetClientKarma`

    //Returns the realtime karma value of a client index as an integer.
    //Returns -1 if client index is invalid.
    
    //@param client				The target client index.
    //@return					The Karma value of the provided client, taken from the server memory.
    native int GetClientKarma(int client);

#### `SetClientKarma`

    //Sets a client index's realtime karma value.
    //Returns false if client index is invalid.
    //Returns true on success.

    //@param client				The target client index.
    //@param karma				The desired karma to set, as an integer.
    //@return					Returns false boolean if client index is invalid.
    native any SetClientKarma(int client, int karma);

## - Zombie State -
    
#### `IsClientZombie`

    //Returns int 1 if client index is a zombie.
    //Returns int 0 if client index is normal.
    //Returns int -1 if PlayerResourceEntity is invalid.

    //@param client				The target client index.
    //@return					The Zombie state of the provided client, formatted as described above.
    native int IsClientZombie(int client);

#### `SetClientZombie`

    //Sets whether a client is a zombie.
    //Only valid state values are 0 and 1. (false and true)
    //Role value 0 will assign the role based on the current role.
    //Role value 1 will assign the client as a zombie innocent.
    //Role value 2 will assign the client as a zombie traitor.
    //Returns false on failure.
    //Returns true on success.

    //@param client				The target client index.
    //@param isZombie			The Zombie state of the provided client, formatted as described above.
    //@param role				The desired role to assign, formatted as described above.
    native any SetClientZombie(int client, int isZombie, int role);

## - Revealed State -
    
#### `IsClientRevealed`

    //Sets whether a client is revealed.
    //Only valid state values are 0 and 1. (false and true)

    //@param client				The target client index.
    //@return					The revealed state of the provided client, formatted as described above.				
    native any IsClientRevealed(int client);

#### `SetClientRevealed`

    //Sets whether a client is revealed.
    //Only valid state values are 0 and 1. (false and true)
    //Returns false on failure.
    //Returns true on success.

    //@param client				The target client index.
    //@param isRevealed			The desired revealed state to apply to the provided client, formatted as described above.
    native any SetClientRevealed(int client, int isRevealed);

## - PseudoNames -

#### `GetClientPseudoName`

    //Returns the PseudoName of a client index as an integer.

    //@param client				The target client index.
    //@return					The PseudoName of the provided client as an integer.

    native int GetClientPseudoName(int client);

#### `OverrideClientPseudoName`

    //Overrides the return value of GetClientPseudoName() that the server recieves.
    //This does not change the "truth" value, but instead replaces the return value of the function responsible for handling the "truthful" PseudoName.
    //Note that the PseudoName integer corresponds to multiple variants, selected by the player's class.

    //@param client				The target client index.
    //@param pseudoName			The desired PseudoName integer.

    native void OverrideClientPseudoName(int client, int pseudoName);

#### `RestoreClientPseudoName`
    
    //Allows the server to set the PseudoName of this client normally.
    //Disables the replacements made in OverrideClientPseudoName().
    //Restores the source of "truth" to the original default logic.

    //@param client				The target client index.

    native void RestoreClientPseudoName(int client);

## - Player State -

#### `GetClientState`

    //Returns the PlayerState of a client index.
    //Returns -1 if client index is invalid.

    //@param client				The target client index.
    //@return					The PlayerState of the provided client, formatted as described above.

    native any GetClientState(int client);

## - Client Roles -

#### `GetClientRoleID`

    //Returns client index's Treason role as "treasonRole" or "int".
    //Returns 0 (TRole_Unassigned / TR_None) if unassigned.
    //Values above 5 (TR_Annihilator) are not valid.
    //This value does not fully dictate the current state or visible role of the player at all times.
    //If you do not know what this does, use GetClientRole() instead.

    //@param client				The target client index.
    //@return					The Treason role of the provided client, formatted as described above.

    native any GetClientRoleID(int client);

#### `SetClientRoleID`

    //Sets a client index's role id on the server.
    //This does not update the player's role, this only sets the value.
    //Returns false when a simple failure is detected.
    //Returns true on success.

    //@param client				The target client index.
    //@param client				The desired role to set, as an integer.
    //@return					Returns false boolean on detected failure.

    native any SetClientRoleID(int client, int role);

#### `GetClientRole`

    //Returns client index's Treason role as "treasonRole" or "int".
    //Returns 0 (TRole_Unassigned / TR_None) if unassigned, spectator, or dead.
    //Dead players are "filtered out" and return 0 (TRole_Unassigned / TR_None).

    //@param client				The target client index.
    //@return					The Treason role of the provided client, formatted as described above.

    native any GetClientRole(int client);

#### `SetClientRole`

    //Sets a client index's role.
    //Returns false when a simple failure is detected.
    //Returns true on success.
    //May return true even if the SDKCall fails. (uncommon)

    //@param client				The target client index.
    //@param client				The desired role to set, as an integer.
    //@return					Returns false boolean on detected failure.

    native any SetClientRole(int client, int role);

#### `GetClientRoleUnfiltered`
    
    //Returns client index's Treason role as "treasonRole" or "int", without any PlayerState checks.
    //Returns 0 (TRole_Unassigned / TR_None) only if unassigned.
    //Death does not affect the return value. A dead traitor/solo still returns TR_Traitor/TR_Solo using this function.
    //Does not return TR_Ghost.

    //@param client				The target client index.
    //@return					The Treason role of the provided client, formatted as described above.

    native any GetClientRoleUnfiltered(int client);
    
#### `GetClientRoleName`

    //Gets client index's role name as an char[] and copies the result to a pre-existing array.
    //Includes all non-conventional role names such as "Ghost" and custom roles, if also desired.
    //Does not work without treason_customroles (TCR) installed.
    //Returns true if successful.
    //Returns false if client index is invalid or another error occurs.

    //@param client				The target client index.
    //@param includeCustomRoles	Whether the output char[] should include custom role names.
    //@param name				A char[] to copy the output of this function to.
    //@param maxlength			The maximum length of the output array.
    //@return					Returns true if successful.

    native any GetClientRoleName(int client, bool includeCustomRoles, char[] name, int maxlength);
    
#### `GetClientRoleNameVanilla`

    //Gets client index's role name as an char[] and copies the result to a pre-existing array.
    //Includes the non-conventional role name "Ghost".
    //Also works without treason_customroles (TCR) installed.
    //Returns true if successful.
    //Returns false if client index is invalid or another error occurs.

    //@param client				The target client index.
    //@param name				A char[] to copy the output of this function to.
    //@param maxlength			The maximum length of the output array.
    //@return					Returns true if successful.

    native any GetClientRoleNameVanilla(int client, char[] name, int maxlength);

#### `GetClientRoleIDName`

    //Gets client index's role id name as an char[] and copies the result to a pre-existing array.
    //Output is decided based on the literal server memory values. This means "Ghost", custom roles, and "Solo" are all not possible outputs.
    //Also works without treason_customroles (TCR) installed.
    //Returns true if successful.
    //Returns false if client index is invalid or another error occurs.

    //@param client				The target client index.
    //@param name				A char[] to copy the output of this function to.
    //@param maxlength			The maximum length of the output array.
    //@return					Returns true if successful.

    native any GetClientRoleIDName(int client, char[] name, int maxlength);
    
## - Client Custom Roles -

#### `IsClientSoloCustomRole`

    //Returns true if the provided client index has a solo-based custom role.
    //Returns false otherwise.
    //Also returns false if the provided client index is invalid.

    //@param client				The target client index.
    //@return					Returns true or false as formatted above.

    native any IsClientSoloCustomRole(int client);
    
#### `SetClientCustomRole`

    //Sets the CustomRole of a client in memory without fully updating the cosmetics.
    //This applies to all HudText, player models, etc...
    //Returns false if the custom role index or client index is invalid.
    //Returns true on success.

    //@param client				The target client index.
    //@param customRoleIndex	The target custom role index.
    //@return					Returns true on success, or false as formatted above.

    native bool SetClientCustomRole(int client, int customRoleIndex);

#### `ResetClientCustomRole`

    //Resets the CustomRole of a client in memory to be default (no custom role).

    //@param client				The target client index.

    native void ResetClientCustomRole(int client);

#### `GetClientCustomRoleIndex`

    //Gets the CustomRole index of the given client index.
    //Returns -1 if client index is invalid.
    //Returns 0 if the client is not assigned a custom role.

    //@param client				The target client index.
    //@return					The CustomRole index of the target client.

    native int GetClientCustomRoleIndex(int client);

## - Weight Classes -

#### `GetClientClass`

    //Returns client index's Treason class as "treasonClass" or "int"
    //Returns -1 if unassigned/inconclusive.

    //@param client				The target client index.
    //@return					The Treason class of the provided client, formatted as described above.

    native any GetClientClass(int client);

## - Abilities -

#### `GetClientAbility`

    //Returns client index's Treason ability from a specific slot (starting from 0)  as "treasonAbility" or "int"
    //Returns integer 0 if client index is invalid.

    //@param client				The target client index.
    //@param slot				The ability slot to return the value of, starting from 0, max 2. (3 available slots)

    native any GetClientAbility(int client, int slot);

#### `GetClientAbilities`

    //Gets client index's Treason abilities as an int[3] and copies the result to a pre-existing int[3] array.
    //USE AN INT ARRAY WITH SIZE 3 FOR ALL THE DATA!
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.
    //@param abilities			An int[] to copy the output of this function to. (Total 3 slots)
    //@param maxlength			The maximum length of the output array. (Your array should ideally always be size 3)

    native any GetClientAbilities(int client, int[] abilities, int maxlength);

#### `ResetClientAbilities`

    //Resets client index's Treason abilities.
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.

    native any ResetClientAbilities(int client);

#### `AddClientAbility`

    //Adds a Treason ability to the given client index.
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.
    //@param ability			The desired ability id as an int.

    native any AddClientAbility(int client, int ability);

## - Gadgets -

#### `GetClientGadget`

    //Returns client index's Treason gadget from a specific slot (starting from 0)  as "treasonGadget" or "int"
    //Returns integer 0 if client index is invalid.

    //@param client				The target client index.
    //@param slot				The gadget slot to return the value of, starting from 0, max 2. (2 available slots)

    native any GetClientGadget(int client, int slot);

#### `GetClientGadgets`

    //Gets client index's Treason gadgets as an int[2] and copies the result to a pre-existing int[2] array.
    //USE AN INT ARRAY WITH SIZE 2 FOR ALL THE DATA!
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.
    //@param gadgets			An int[] to copy the output of this function to. (Total 2 slots)
    //@param maxlength			The maximum length of the output array. (Your array should ideally always be size 2)

    native any GetClientGadgets(int client, int[] gadgets, int maxlength);

#### `ResetClientGadgets`

    //Resets client index's Treason gadgets.
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.

    native any ResetClientGadgets(int client);

#### `AddClientGadget`

    //Adds a Treason gadget to the given client index.
    //Returns true if successful.
    //Returns false if client index is invalid.

    //@param client				The target client index.
    //@param ability			The desired gadget id as an int.

    native any AddClientGadget(int client, int gadget);
