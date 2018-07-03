

# REQUIREMENTS:

### A) Nitrado account, that has access to the Server Dashboard.
it will give you access to the **Server Dashboard** link https://webinterface.nitrado.net/13739121/wi/

### B) FTP client
Install the `FileZilla Client` - https://filezilla-project.org/ with default options, you may ignore the bloatware that it may offer (Opera browser).

### C) Mission content
Mission files, the modlist.html, and all the mods for your mission must be on your PC.

# DEPLOYING THE MISSION TO THE SERVER:

### Phase 1. Stop the server

buttons for starting/stopping the server are available on the main page of the Server Dashboard

### Phase 2. Upload the necessary mods to the server

This is the most monke part of the process, as all mods must be copied over to the server manually, if they're not there yet, or if they need updating.
In ArmA Launcher, by clicking on any mod options icon `(...) - Open folder in Windows Explorer` you will end up in the directory where this mod is stored locally, which is what you need to copy to the server. Mod folder typically starts with the `@` symbol.

- Open Filezilla and connect to the ArmA FTP server. To authenticate, use `hostname` `username` `password` `port` FTP credentials, that are available on the bottom part of the Server Dashboard main page.

Once connected, you should see the `arma3` folder, in the right window of the FTP client. This is where the server files reside.

- Check if the addon is already present in the system. All mods are located at the top level of the `arma3` folder. If it is there **AND** you know that it needs updating, delete it. You can drag and drop files from the left side of the FTP client, or from Windows Explorer.

#### ! moving/removing files through FTP client can take time, watch the progress log/bar at the top/bottom windows of FileZilla to see when it's done. !

- Upload the whole mod folder from the local PC to the server. It must end up on the same level of the `arma3` directory as other mods.

#### ! ALWAYS For TFAR rename the mod folder to `@TFAR_BETA`, as it has special characters, that cause issues. !

- If the mod has `keys` folder, we must add that key to the server too, but in a separate location. Copy that `*.bikey` file from the `keys` folder to the `arma3\keys` folder.

- Do this with every mod that has to be in the mission. Most of the mods (TFAR, RHS) don't really need updating, but if you saw in the launcher that they got updated recently, or if you will get an error/warning later on the server, then go ahead and do them too.

### Phase 3. Generate command-line parameter

Now with all mods present on the server, we must create a special launch parameter for the server, that tells which mods must be loaded.

- Go to the Website https://a3config.byjokese.com/stringGen.html to generate this string. All you need to do is drag and drop the modlist HTML file in there, and you will get a "result string".
- Open Server Settings page: https://webinterface.nitrado.net/13739121/wi/gameserver/gameserver/settings
- In the `Additional mods` field, paste that result string.

#### ! Every mod starts with `@` and ends with `;` . If you use Creator DLC, this is also where you declare them, they have special names that don't start with `@`: S.O.G. Prairie Fire - `vn`, Global Mobilization - `gm`, etc. No need to include compatibility mods!

#### ! Like in previous step, to change TFAR name to `@TFAR_BETA` !

- Example of the resulting mod string:

```
`gm;@CBA_A3;@ace;@Zeus Enhanced;@RHSUSAF;@RHSAFRF;@RHSGREF;@Immerse;@Zeus Enhanced - ACE3 Compatibility;@DUI - Squad Radar;@Enhanced Movement;@TFAR_beta;@Jbad;@LYTHIUM;@Simplex Support Services;@Project OPFOR;@CUP Terrains - Core;@Advanced IED System;@Project True Viking 2.0;@LAMBS_Turrets;@LAMBS_Suppression;@LAMBS_Danger.fsm;`
```

### phase 4. Upload mission files


- Your mission must go into the `arma3/mpmissions` folder. Copy it there through the FTP client.
- Open Config Files page: https://webinterface.nitrado.net/13739121/wi/gameserver/gameserver/configfiles
- Scroll down to the bottom, and edit the `template` parameter, that references the mission folder name, which tells which mission must be loaded.


#### If the mission file contains characters like empty spaces, they must be converted into the special encoded sequence (e.g. %20 for space). If you don't want to deal with that, only use basic numbers, letters, and `_` or `-` symbols for your mission folder name.


- Example for the config file section  (`Danmark Afghanistan` becomes `Danmark%20Afghanistan` because of the space in it):

```
class Missions
{
          class Mission1
         {
              template="Danmark%20Afghanistan.lythium";
              difficulty="custom";
          };
};
```

### phase 5. Launch the server and verify

You can now start the server with your goofy ahh op through the Nitrado Dashboard

## If you messed up and you can't connect to the server, look at the error (errors related to mods/keys/mission, w.e.) go through all phases again and fix it.
## If the server is not visible, check its logs


