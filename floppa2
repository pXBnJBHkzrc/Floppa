-- locals
local players = game:GetService("Players")
local teleportService = game:GetService("TeleportService")
local replicatedStorage = game:GetService("ReplicatedStorage")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local remotes = replicatedStorage:WaitForChild("Remotes")
local remote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Data"):WaitForChild("UpdateHotbar")
local senv = getsenv(players.LocalPlayer.PlayerGui:WaitForChild("Inventory"):WaitForChild("InventoryHandle"))
local FireServer = senv._G.FireServer
local inventoryRemote = remotes.Information.InventoryManage
local localPlayer = players.LocalPlayer
local Moves = {}
local lp = game:GetService("Players").LocalPlayer
local variables = {
    ToDrop = {},
    DropToggle = false,
}
-- fuctions
-- fuctions


local function GetUniqueToolNames()
    local uniqueToolNames = {}
    local toolsFolder = localPlayer.Backpack:FindFirstChild("Tools")
    if toolsFolder then
        for _, tool in ipairs(toolsFolder:GetChildren()) do
            if not table.find(uniqueToolNames, tool.Name) then
                table.insert(uniqueToolNames, tool.Name)
            end
        end
    end
    return uniqueToolNames
end
-- moves

-- Anti Afk
if getconnections then
    for _, v in next, getconnections(game:GetService("Players").LocalPlayer.Idled) do
        v:Disable()
    end
end

if not getconnections then
    game:GetService("Players").LocalPlayer.Idled:connect(
        function()
            game:GetService("VirtualUser"):ClickButton2(Vector2.new())
        end
    )
end
--end


local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Fluent " .. Fluent.Version,
    SubTitle = "by floppa",
    TabWidth = 100,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.L
})
local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "settings" }),
    Teleports = Window:AddTab({ Title = "Teleports", Icon = "settings" }),
    Dupe = Window:AddTab({ Title = "Dupe", Icon = "settings" }),
}

Tabs.Dupe:AddButton({
    Title = "Pickup All Plants",
    Callback = function()
        local avoidCFrame = CFrame.new(1465.6145, 48.1683693, -3372.54272, -0.406715393, 0, -0.913554907, 0, 1, 0,
            0.913554907, 0, -0.406715393)
        local trinkets = {}
        local originalLocation = lp.Character.HumanoidRootPart.CFrame

        for _, Trinket in pairs(game:GetService("Workspace").SpawnedItems:GetDescendants()) do
            if Trinket:IsA("Part") and Trinket.Name == "ClickPart" and Trinket.CFrame ~= avoidCFrame then
                table.insert(trinkets, Trinket)
            end
        end

        for _, Trinket in ipairs(trinkets) do
            lp.Character.HumanoidRootPart.CFrame = Trinket.CFrame
            task.wait(0.35)
            for _, v in pairs(game:GetService("Workspace").SpawnedItems:GetDescendants()) do
                if v:IsA("ClickDetector") and lp:DistanceFromCharacter(v.Parent.Position) <= 10 then
                    fireclickdetector(v)
                end
            end
        end
        lp.Character.HumanoidRootPart.CFrame = originalLocation
    end
})
function serverhop()
    local PlaceID = game.PlaceId
    local AllIDs = {}
    local foundAnything = ""
    local actualHour = os.date("!*t").hour
    local Deleted = false

    function TPReturner()
        local Site;
        if foundAnything == "" then
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100'))
        else
            Site = game.HttpService:JSONDecode(game:HttpGet('https://games.roblox.com/v1/games/' .. PlaceID .. '/servers/Public?sortOrder=Asc&limit=100&cursor=' .. foundAnything))
        end
        local ID = ""
        if Site.nextPageCursor and Site.nextPageCursor ~= "null" and Site.nextPageCursor ~= nil then
            foundAnything = Site.nextPageCursor
        end
        local num = 0;
        for i,v in pairs(Site.data) do
            local Possible = true
            ID = tostring(v.id)
            if tonumber(v.maxPlayers) > tonumber(v.playing) then
                for _,Existing in pairs(AllIDs) do
                    if num ~= 0 then
                        if ID == tostring(Existing) then
                            Possible = false
                        end
                    else
                        if tonumber(actualHour) ~= tonumber(Existing) then
                            local delFile = pcall(function()
                                -- delfile("NotSameServers.json")
                                AllIDs = {}
                                table.insert(AllIDs, actualHour)
                            end)
                        end
                    end
                    num = num + 1
                end
                if Possible == true then
                    table.insert(AllIDs, ID)
                    wait()
                    pcall(function()
                        -- writefile("NotSameServers.json", game:GetService('HttpService'):JSONEncode(AllIDs))
                        wait()
                        game:GetService("TeleportService"):TeleportToPlaceInstance(PlaceID, ID, game.Players.LocalPlayer)
                    end)
                    wait(4)
                end
            end
        end
    end

    function Teleport()
        while wait() do
            pcall(function()
                TPReturner()
                if foundAnything ~= "" then
                    TPReturner()
                end
            end)
        end
    end

    Teleport()
end

Tabs.Main:AddButton({
    Title = "Server Hop",
    Default = false,
    Save = false,
Callback = function()
    serverhop()
end
})

Tabs.Main:AddButton({
    Title = "Teleport To Merchant",
    Callback = function()
        if game:GetService("Workspace").NPCs:FindFirstChild("Mysterious Merchant") then
            lp.Character.HumanoidRootPart.CFrame = game:GetService("Workspace").NPCs:FindFirstChild(
                "Mysterious Merchant").HumanoidRootPart.CFrame
        else
            Fluent:Notify({
                Title = "Mysterius Merchant Has Not Spawned",
                Content = "Can't Teleport To Mysterius Merchant, Has Not Appear",
                Duration = 5
            })
        end
    end
end
})
local Walk = 1.6
Tabs.Main:AddButton({
    Title = "Enable Sprint Speed",
    Default = false,
    Callback = function(Value)
        Walkspeeder = Value
        while Walkspeeder do
            task.wait()
            if Walkspeeder then
                lp.Character.Effects.RunBuff.Value = Walk
            end
        end
    end
end
})
local Slider = Tabs.Main:AddSlider("Slider", {
    Title = "Speed Modificator",
    Content = "Change Sprint Speed",
    Default = 2,
    Min = 1,
    Max = 10,
    Rounding = 1,
    Callback = function(Value)
        Walk = Value
        if Walkspeeder then
            lp.Character.Effects.RunBuff.Value = Value
        end
    end
end
})
Slider:OnChanged(function(Value)
    print("Slider changed:", Value)
end)

Slider:SetValue(1)

local merchNotiEnabled = false
local merchNotifier = false
local ToggleMerchant = Tabs.Main:AddToggle("ToggleMerchant", {Title = "Merchant Notifier", Default = false })

Toggle:OnChanged(function()
    Flag = "MerchantNoti",
    Callback = function(value)
        merchNotiEnabled = value
        if merchNotiEnabled then
            while merchNotiEnabled do
                task.wait()
                local merchant = game:GetService("Workspace").NPCs:FindFirstChild("Mysterious Merchant")
                if merchant then
                    Fluent:Notify({
                        Title = "Mysterious Merchant Detected",
                        Content = "The Mysterious Merchant Has Appear!",
                        Duration = 5
                    })
                    merchnotifier = true
                    task.wait(10)
                    merchnotifier = false
                end
            end
        end
    end
end
    print("Toggle changed:", Options.MyToggle.Value)
end)

Main.ToggleMerchant:SetValue(false)


-- Rejoin
Tabs.Dupe:AddButton({
    Title = "Rejoin",
    Callback = function()
        game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game:GetService("Players").LocalPlayer)
    end
})

local FireServer = senv._G.FireServer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local remote = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Data"):WaitForChild("UpdateHotbar")

local Toggle = Tabs.Dupe:AddToggle("MyToggle", {Title = "Enable Rollback", Default = false })

local toggleState = false

Toggle:OnChanged(function(state)
    if state ~= toggleState then
        toggleState = state
        if toggleState then
            coroutine.wrap(function()
                while toggleState do
                    task.wait()
                    FireServer(remote, {[255] = "\255"})
                    FireServer(remote, {[2] = "\255"})
                    print("Rollback Setup")
                end
            end)()
        end
    end
end)
Toggle:SetValue(false)



SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
