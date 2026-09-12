<style>
	h2 {
		color: green;
		font-weight: bold;
	}
	h4 {
		font-weight: bold;
	}
	h5 {
		color: purple;
		font-weight: bold;
	}
</style>

# **Overview**

- The **Treason-API (TAPI)** creates enumerations for the magic numbers **Treason (KVT)** stores to memory.
- This generally **improves readability**, and therefore usage is **highly encouraged.**
- It is common for these enumerations to be applicable to TAPI, **but not always meaningful to KVT.**
    - *Example 1: `TRole_Ghost`*
    - *Example 2: `TEndCondition_CustomSolo`*  
    
> Note that enumerations are a collection of enumerators. Not the other way around.
> Example: `treasonRole` (enumeration) contains `TR_Traitor` (enumerator)

# **Enumerations**

<!-- Roles -->
## treasonRole
- Used to semantically express a role in **Treason.**
- Some entries do **not** directly map to the **KVT role ID.**

##### TRole_Unassigned
- Alias: `TR_None`
##### TRole_Innocent
- Alias: `TR_Innocent`
##### TRole_Traitor
- Alias: `TR_Traitor`
##### TRole_Detective
- Alias: `TR_Detective`
##### TRole_Doctor
- Alias: `TR_Doctor`
##### TRole_Annihilator
- Alias: `TR_Annihilator`
##### TRole_Ghost
- Alias: `TR_Ghost`
##### TRole_Independent
- Alias: `TR_Solo`

<!-- Classes -->
## treasonClass
- Used to express a weight class in **Treason.**
- **Maps directly to the magic values Treason understands.** (With the exception of `TClass_Invalid`)

##### TClass_Invalid
- Alias: `TC_None`
##### TClass_Light
- Alias: `TC_Light`
##### TClass_Medium
- Alias: `TC_Med`
##### TClass_Heavy
- Alias: `TC_Heavy`

<!-- Abilities -->
## treasonAbility
- Used to express an ability in **Treason.**
- **Maps directly to the magic values Treason understands.**

##### TAbility_None
- Alias: `TA_None`
##### TAbility_Medkit
- Alias: `TA_Medkit`
##### TAbility_Shield
- Alias: `TA_Shield`
##### TAbility_Adrenaline
- Alias: `TA_Adrenaline`
##### TAbility_ClueRadar
- Alias: `TA_ClueRadar`
##### TAbility_TeamRadar
- Alias: `TA_TeamRadar`
##### TAbility_TraitorRadar
- Alias: `TA_TRadar`
##### TAbility_ZombieRevive
- Alias: `TA_Zombie`
##### TAbility_DetectiveRadar
- Alias: `TA_DRadar`
##### TAbility_DetectiveResuscitate
- Alias: `TA_DetectiveRes`
##### TAbility_RangeHeal
- Alias: `TA_RangeHeal`
##### TAbility_RangeAdrenaline
- Alias: `TA_RangeAdrenaline`
##### TAbility_DoctorResuscitate
- Alias: `TA_DoctorRes`
##### TAbility_BodyRadar
- Alias: `TA_BodyRadar`
##### TAbility_GhostTransform
- Alias: `TA_Ghost`
##### TAbility_GhostRadar
- Alias: `TA_GhostRadar`

<!-- Gadgets -->
## treasonGadget
- Used to express a gadget in **Treason.**
- **Maps directly to the magic values Treason understands.**

##### TGadget_None
- Alias: `TG_None`
##### TGadget_Bomb
- Alias: `TG_Bomb`
##### TGadget_LethalTaser
- Alias: `TG_CarnageTaser`
##### TGadget_SilencedRevolver
- Alias: `TG_Revolver`
##### TGadget_SpikedBat
- Alias: `TG_SpikedBat`
##### TGadget_Disguise
- Alias: `TG_Disguise`
##### TGadget_BearTrap
- Alias: `TG_BearTrap`
##### TGadget_Landmine
- Alias: `TG_Landmine`
##### TGadget_PoisonDart
- Alias: `TG_PoisonDart`
##### TGadget_SilencedPistol
- Alias: `TG_Pistol`
##### TGadget_Scanner
- Alias: `TG_Scanner`
##### TGadget_StunTaser
- Alias: `TG_Taser`
##### TGadget_HealingStation
- Alias: `TG_HealingStation`

<!-- Player States -->
## treasonPlayerState
- Used to express a player state in **Treason.**
- **Maps directly to the magic values Treason understands.**
- Most enumerators are unidentified and require further reverse engineering or developer-help to understand.
- This is useful for **read-only** operations such as `GetClientState`.

##### TPlayerState_Default `[0]`
- Alias: `TS_Default`
##### TPlayerState_Unknown1 `[1]`
- Alias: `TS_Unknown1`
##### TPlayerState_Spectator `[2]`
- Alias: `TS_Spectator`
##### TPlayerState_FreezeCam `[3]`
- Alias: `TS_FreezeCam`
##### TPlayerState_Unknown2 `[4]`
- Alias: `TS_Unknown2`
##### TPlayerState_Injured `[5]`
- Alias: `TS_Injured`
##### TPlayerState_Ghost `[6]`
- Alias: `TS_Ghost`
##### TPlayerState_Unknown3 `[7]`
- Alias: `TS_Unknown3`
##### TPlayerState_Unknown4 `[8]`
- Alias: `TS_Unknown4`
##### TPlayerState_UIFrozen `[9]`
- Alias: `TS_UIFrozen`

<!-- End Conditions -->
## treasonEndCondition
- Used to express a round-end condition in **TAPI.**
- **Not intended for use outside of `ForceEndRound`.**

##### TEndCondition_TeamWin
- Alias: `TE_TeamWin`
##### TEndCondition_Deathmatch
- Alias: `TE_Deathmatch`
##### TEndCondition_Time
- Alias: `TE_Time`
##### TEndCondition_CustomSolo
- Alias: `TE_Solo`
