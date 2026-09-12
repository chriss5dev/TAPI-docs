# **The TCR System**
*"Treason Custom Roles"*  
</br>
The **Treason-API (TAPI)** runs a system called **Treason Custom Roles (TCR)** to manage custom roles. TCR is contained within `treason_customroles.sp`, which is included in the main `treason_api.sp`.  
> Note that TCR forcibly disables the Karma system due to conflicts with how the vanilla game handles incorrect and correct kills.
> This is to be fixed in the future.

# **The `CustomRole` enum struct**
The details of how enum structs work is irrelevant. **However,** the data stored inside of this particular enum struct *is* both relevant and important to understand.  

- The `CustomRole` enum struct contains all the desired details required for indentifying, assigning, and handling custom roles in TCR.  
- Some notable examples include:
    - `char[] id`  
    - `char[] displayName`  
    - `treasonRole underlyingRole`  
    - `int prevalence`  
    - `char[] playerModel`  
    - `etc...`  
- The variables contained inside of `CustomRole` is referred to as the **prototype.**
- As new variables are added in TAPI updates, the associated **prototype version** is incremented to allow for backwards compatibility with old TCR-based custom role plugins.

# **How TCR uses `CustomRole`**
## **OnPluginStart**
#### Relevant variables are created.
    
    public CustomRole g_CustomRoles[MAXCUSTOMROLES];
    public int g_ClientRoles[MAXPLAYERS+1];
    
#### TCR runs setup.
- TCR checks for the **SendProxy** dependency.  
- An error is thrown if this dependency is not installed.  
- **TCR natives** are created.  
- **Global Forwards** are created.  
- TCR prints to the server console: `"[TCR] Treason Custom Roles Loaded!"`  
- **The `g_CustomRoles` array is initialized/cleared.**  
    - For each index of the array, the first character of the `char id[32]` is set to **byte 0** (`'\0'`)  
    - **! The index of this array is referred to as the `customRoleIndex` !**  

## **Usage of `g_ClientRoles`**
- Since custom roles are stored in the `g_CustomRoles` array, we can refer to any of these custom roles by using an array index.  
- **! The index of the `g_CustomRoles` array is referred to as the `customRoleIndex` !**  
- To track a client's custom role, we can use their client index as the index of the `g_ClientRoles` array, and store the `customRoleIndex` at that location.  
</br>
#### For example:
    
    // Suppose custom role "Lone Wolf" is registered at `g_CustomRoles[4]`
    // Therefore, the `customRoleIndex` of the "Lone Wolf" custom role is 4.

    int myImaginaryClient = 7;
    int customRoleIndex = 4;

    g_ClientRoles[myImaginaryClient] = customRoleIndex;
    
    // or hard-coded (don't actually write like this)
    g_ClientRoles[7] = 4;
    
- Now, the client with client index `7` is internally tracked by TCR as having a `customRoleIndex` of `4`.  
- A `customRoleIndex` of `0` semantically represents "no custom role".  
</br>
**This is how TCR internally tracks the custom roles of all clients.**

## **OnMapStart**
#### TCR adds custom assets to the downloads table.

    public void TCR_OnMapStart()
    {
        AddFolderToDownloadsTable("models/props_cluesystem/custom");
        AddFolderToDownloadsTable("models/player/custom");
        AddFolderToDownloadsTable("materials/hud/playercard/custom");
        AddFolderToDownloadsTable("materials/models/player/custom");
        AddFolderToDownloadsTable("materials/models/props_cluesystem/custom");
        
        // unrelated behavior that comes after this
        // ...
    }
    
- If any of these folders do not exist, a warning will be shown in the server console.  
- However, if you don't have any desired content to put inside any one of these folders, you can ignore the warning message.

# **The Round Instance**
To further explain how TCR operates, its behavior is primarily documented below in the context of a "Round Instance" which hypothetically resembles a round of Treason.  
> Note that some details are entirely skipped since they aren't relevant to understanding how TCR behaves.

## **PreRoundStart**
- `g_ClientRoles` is cleared.  
- `g_CustomRoles` is cleared.  
- Global Forward `OnRegisterCustomRoles` is called.

### **`OnRegisterCustomRoles`**
**This is the global forward where custom role plugins are expected to use the `RegisterCustomRole` function.**
#### When `RegisterCustomRole` is called:
- TCR checks the prototype version.  
- TCR assigns the provided arguments from the caller to local variables based on the prototype version.  
- TCR calls an internal version of `RegisterCustomRole` using the local variables.  
- `RegisterCustomRole` checks for an empty index in `g_CustomRoles`.  
- If there is an empty index, its `CustomRole` variables are assigned to the values of the local variables.  
- This index's element is now populated with the desired custom role data.  
- At this point in time, the previously empty element index is now our `customRoleIndex`, which is the value returned by `RegisterCustomRole`.

## **RoundStart (Post)**
**Here, TCR calls `AssignCustomRoles`.**
### **`AssignCustomRoles`**
**There is a ridiculous amount of behavior that goes into randomly assigning custom roles, but to keep it super simple:**  

- All valid players are selected and added to an array based on their role ID.
- All registered custom role indexes are added to a lottery pool.
- Based on the provided value of `prevalence`, random custom-role candidates are selected in a dice roll, according to their `underlyingRole`
- If multiple custom roles tie in the lottery, a weighted dice roll is used to determine the final winner based on their provided custom-role `weight`.
- The final winning custom roles for each `underlyingRole` category are assigned to random valid players.
- This is done with the native function `SetClientCustomRole`.
- Inside of `SetClientCustomRole`, the Global Forward `OnClientAssignedCustomRole` is called.
#### `SetClientCustomRole`

    //get client index and customRoleIndex arguments (from native call)
    int client = GetNativeCell(1);
    int customRoleIndex = GetNativeCell(2);
    
    // if customRoleIndex != 0 and provided client index is valid
    if(IsCustomRoleValid(customRoleIndex) && client > 0 && IsClientInGame(client))
    {
        //internally assign the custom role to the client index
        g_ClientRoles[client] = customRoleIndex;
        
        // Global Forward `OnClientAssignedCustomRole` is also called here in the actual code.
        
        return true; //success
    }
    return false; //failure
    
> Note that this native is not typically intended for use mid-round.
> An attempt to do so will lead to broken behavior.

### `OnClientAssignedCustomRole`
**This is the global forward where custom role plugins are expected to track if a client has their registered custom role.**
#### For example:

    int g_MyCustomRoleClient = -1; // "invalid entity" or "no client right now"
    int g_crMyCustomRoleIndex; // assigned to `RegisterCustomRole` return value
    
    public void OnClientAssignedCustomRole(int client, int customRoleIndex)
    {
        if(customRoleIndex == g_crMyCustomRoleIndex) // the customRoleIndex returned when using `RegisterCustomRole`
        {
            //track which client index is the custom role of `g_crMyCustomRoleIndex`
            g_MyCustomRoleClient = client;
        }
    }

## **RoundEnd (Pre)**
- All clients' stored custom roles (`g_ClientRoles`) are cleared.
- All custom roles (`g_CustomRoles`) are cleared.
- Global Forward `OnClearCustomRoles` is called.
### `OnClearCustomRoles`
**This is the global forward where custom role plugins are expected to reset the variables they use to track a custom role.**
#### For example:
    
    int g_MyCustomRoleClient = 5; // example client `5`
    int g_crMyCustomRoleIndex = 3; // example customRoleIndex `3`
    
    public void OnClearCustomRoles()
    {
        //this is all no longer accurate, so reset it
        g_crMyCustomRoleIndex = -1;
        g_MyCustomRoleClient = -1;
    }

# **Moving on...**
**Congratulations!**  
</br>
If you understand the system described above, you are ready to [create your first custom role!](tcr_create.md)  
