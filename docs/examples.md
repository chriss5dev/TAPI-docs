# **Examples**

## `GetClientRole`

    #include <sourcemod>
    #include <treason>
    
    // When this plugin starts...
    public void OnPluginStart()
    {
        // Register a console command "sm_myrole" (!myrole)
        RegConsoleCmd("sm_myrole", CmdMyRole);
    }
    
    Action CmdMyRole(int client, int args)
    {
        // Get the role of the client that called the command
        int role = GetClientRole(client);
        
        // Print the role to the client using formatting parameters
        PrintToChat(client, "Your role is: %d", role)
        
        // Tell SourceMod this command is done.
        return Plugin_Handled;
    }

## `GetClientRoleName`

    #include <sourcemod>
    #include <treason>
    
    // When this plugin starts...
    public void OnPluginStart()
    {
        // Register a console command "sm_myrolename" (!myrolename)
        RegConsoleCmd("sm_myrolename", CmdMyRoleName);
    }
    
    Action CmdMyRoleName(int client, int args)
    {
        // Create a char[] (C-String) to store the result of `GetClientRoleName`
        char nameBuffer[32];
        
        // Get the name of the role of the calling client
        GetClientRoleName(client, true, nameBuffer, sizeof(nameBuffer));
        
        // Print `nameBuffer` to the client using formatting parameters
        PrintToChat(client, "Your role is: %s", nameBuffer);
        
        // Tell SourceMod this command is done.
        return Plugin_Handled;
    }
    
## `TAPI_Version`

    #include <sourcemod>
    #include <treason>
    
    // 1.6.1 (01.06.01)
    #define REQUIRED_TAPI_VERSION 010601
    
    // Once all plugins are loaded, including TAPI...
    public void OnAllPluginsLoaded()
    {
        // If the TAPI version is less than the required version...
        if (TAPI_Version() < REQUIRED_TAPI_VERSION)
        {
            // Throw an error
            SetFailState("[ERROR] This plugin requires Treason-API version %d or later!", REQUIRED_TAPI_VERSION);
        }
    }

## `SetClientZombie`

    #include <sourcemod>
    #include <treason>
    
    // When this plugin starts...
    public void OnPluginStart()
    {
        // Register a console command "sm_zombie" (!zombie)
        RegConsoleCmd("sm_zombie", CmdZombie);
    }
    
    Action CmdZombie(int client, int args)
    {
        // If command is called with incorrect argument amount
        if(args != 1)
        {
            // Describe usage in chat
            PrintToChat(client, "Usage: sm_zombie <isZombie>");
            
            // Tell SourceMod this command is done.
            return Plugin_Handled;
        }
        
        // Get isZombie argument
        int isZombie = GetCmdArgInt(0);
        
        // Set the caller client's zombie state to `isZombie`
        // If failure (false), print error
        if(!SetClientZombie(client, isZombie, 0))
        {
            // Print `isZombie` to the client using formatting parameters
            PrintToChat(client, "Zombie state '%d' is invalid!", isZombie);
            
            // Tell SourceMod this command is done.
            return Plugin_Handled;
        }
        
        // Print `isZombie` to the client using formatting parameters
        PrintToChat(client, "Zombie state set to %d", isZombie);
        
        // Tell SourceMod this command is done.
        return Plugin_Handled;
    }
