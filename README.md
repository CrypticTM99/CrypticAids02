
This is a small tutorial on creating a script for the **ESX Framework for FiveM**. This guide is designed to walk you through the process of creating a basic script, from setting up the environment to scripting your first functional feature.
-
-
# **Creating a Script for ESX(es_extended) Framework**

## **Overview**

This tutorial will guide you through the process of creating a basic script for the **ESX Framework** in **FiveM**, using the most up-to-date version. We'll cover everything from setting up your development environment, scripting a simple feature, to testing and deploying it on your server.

Whether you're new to FiveM scripting or experienced with Lua, this guide will get you up and running quickly.

## **Prerequisites**

Before we begin, ensure you have the following:

* **A working FiveM server**: You need a server running FiveM and ESX. If you don't have one set up, [follow the FiveM server setup guide here](https://docs.fivem.net/docs/server-manual/setting-up-a-server/).
* **Basic knowledge of Lua**: ESX scripts are written in Lua, so it's good to have a basic understanding of how to work with the language.
* **A text editor/IDE**: You can use any text editor for coding, but [Visual Studio Code](https://code.visualstudio.com/) is highly recommended for its excellent Lua support and extensions.

---

## **Step 1: Setting Up Your Environment**

1. **Install ESX Framework**
   Ensure your FiveM server is running **ESX**. If ESX is not yet installed, you'll need to follow the [official ESX installation guide](https://github.com/ESX-Org/esx_framework). This will set up your server with the necessary dependencies.

2. **Create a Script Folder**
   Navigate to the `resources/` directory of your server, and create a new folder for your script. For example:

   ```
   resources/
     └── my_script/
   ```

   This will be the folder where you'll store all the files related to your script.

3. **Create the Basic Script Files**
   Inside your new script folder (`my_script/`), create these basic files:

   * `fxmanifest.lua`: This is the metadata file that tells FiveM how to load and configure your script.
   * `client.lua`: This file will contain the client-side script.
   * `server.lua`: This file will contain the server-side script.

---

## **Step 2: Writing the fxmanifest.lua File**

The `fxmanifest.lua` file is required for every FiveM script. It defines the metadata and ensures your script is properly loaded.

```lua
fx_version 'cerulean'
game 'gta5'

author 'Your Name'
description 'Cryptic's tutorial script_here'
version '1.0.0'

-- The client-side Lua script
client_script 'client.lua'

-- The server-side Lua script
server_script 'server.lua'
```

This file does the following:

* Specifies the FiveM `fx_version` (use 'cerulean' for the latest stable version).
* Defines the game you're scripting for (`gta5`).
* Lists the author, description, and version of your script.
* Specifies which files should be loaded for the client and server.

---

## **Step 3: Writing the Client-Side Script (client.lua)**

In the `client.lua` file, you can write code that runs on the client’s machine. For example, we'll create a simple command that sends a message when a player enters a specific area.

```lua
Citizen.CreateThread(function()
    while true do
        Citizen.Wait(0)

        -- Define the player's current position
        local playerPed = PlayerPedId()
        local pos = GetEntityCoords(playerPed)

        -- Define the area (x, y, z)
        local area = vector3(100.0, -2000.0, 30.0)

        -- Check if the player is in the defined area
        if Vdist(pos.x, pos.y, pos.z, area.x, area.y, area.z) < 10.0 then
            -- Display a notification to the player
            SetTextComponentFormat('STRING')
            AddTextComponentString("You are near the special area!")
            DisplayHelpTextFromStringLabel(0, 0, 1, -1)

            -- Optionally, trigger a server event
            if IsControlJustPressed(1, 51) then  -- Press 'E' to trigger an event
                TriggerServerEvent('my_script:triggerAction')
            end
        end
    end
end)
```

This script:

* Runs a loop to constantly check the player's position.
* If the player enters a specific area (defined by coordinates), it shows a notification.
* Pressing the **E** key triggers a server-side event, which we'll handle in `server.lua`.

---

## **Step 4: Writing the Server-Side Script (server.lua)**

The `server.lua` file is where you'll define events and interactions that happen on the server. Here's an example where the server listens for the event triggered by the client.

```lua
RegisterServerEvent('my_script:triggerAction')
AddEventHandler('my_script:triggerAction', function()
    local _source = source
    local playerName = GetPlayerName(_source)

    -- Print a message to the console (this is just an example)
    print(playerName .. ' has triggered the action!')

    -- Optionally, send a message to the player
    TriggerClientEvent('chat:addMessage', _source, {
        args = {'Server', 'You triggered the action!'}
    })
end)
```

This script:

* Listens for the `my_script:triggerAction` event sent by the client.
* When the event is received, it retrieves the player’s name and prints it to the console.
* Sends a message to the player confirming the action was triggered.

---

## **Step 5: Testing Your Script**

1. **Start Your Server**
   After saving your script files, start your server. Ensure that your script is placed in the `resources/` folder and is added to the `server.cfg` file. For example:

   ```cfg
   ensure my_script
   ```

2. **Run the Game**
   Connect to your FiveM server and check if the script works as expected:

   * When you approach the defined area, you should see a message.
   * Pressing **E** should trigger the server event, which will print to the console and send a message to the player.

---

## **Step 6: Deploying the Script**

Once your script is tested and working, you're ready to deploy it to your server:

1. **Upload the Script**
   Upload the script folder (`my_script/`) to your server's `resources/` directory.

2. **Add the Script to server.cfg**
   Open the `server.cfg` file and add the following line to ensure your script is loaded:

   ```cfg
   ensure my_script
   ```

3. **Restart the Server**
   Restart your server to apply the changes. Players can now use your new script on your server!

---

## **Conclusion**

Congratulations! You've just created and deployed your first script for the ESX Framework in FiveM. While this example was simple, you can extend it further by adding more complex features like jobs, inventories, or custom UI elements.

If you want to continue developing your script, here are some ideas:

* Create custom commands that players can use in-game.
* Integrate with the ESX economy (e.g., give players money, items, etc.).
* Create a custom menu or UI that interacts with server-side data.

Feel free to experiment and build upon this foundation!

---

## **Credits**

* **Author**: CrypticTM
* **Version**: 1.0.0
* Special thanks to the **ESX Framework** and **FiveM** communities for their continued support and development.

---

This tutorial is structured to be beginner-friendly while still providing a solid foundation for more advanced scripting. You can adjust the complexity as you get more comfortable with FiveM and ESX scripting. Let me know if you need any additional clarifications!
