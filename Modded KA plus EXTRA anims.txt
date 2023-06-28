
if game.CoreGui:FindFirstChild("Sigma_1") then

else



local savedc0 = game:GetService("ReplicatedStorage"):WaitForChild("Assets"):WaitForChild("Viewmodel"):WaitForChild("RightHand"):WaitForChild("RightWrist").C0
local setc0

----------------------------------------------------------------------------------------------------VARS
local lplr = game.Players.LocalPlayer
local tab = {}
local PLAYERS = game:GetService("Players")
local isnetworkowner = isnetworkowner or function() return true end
local SteppedTable = {}
local RunService = game:GetService("RunService")
local Heartbeat = RunService.Heartbeat
humanoid = lplr.Character.Humanoid
character = lplr.Character
lplrvelo = lplr.Character:WaitForChild("HumanoidRootPart").Velocity
lplrvelomag = lplrvelo.Magnitude
vec3 = Vector3.new
queuedup = false
cantblatantmode = false

local cam = workspace.CurrentCamera
workspace:GetPropertyChangedSignal("CurrentCamera"):connect(function()
	cam = (workspace.CurrentCamera or workspace:FindFirstChild("Camera") or Instance.new("Camera"))
end)
local repstorage = game:GetService("ReplicatedStorage")
loadsettingsdone = false
-----------------------------------------------------------------------------------------FUNCS
local function isAlive(plr)
    local plr = plr or lplr
    if plr and plr.Character and ((plr.Character:FindFirstChild("Humanoid")) and (plr.Character:FindFirstChild("Humanoid") and plr.Character:FindFirstChild("Humanoid").Health > 0) and (plr.Character:FindFirstChild("HumanoidRootPart")) and (plr.Character:FindFirstChild("Head"))) then
        return true
    end
end

local function getremote(t)
    for i,v in next, t do 
        if v == "Client" then 
            return t[i+1]
        end
    end
end

local function hashvector(vec)
	return {
		value = vec
	}
end

local function hashvec(vec)
	return {
		value = vec
	}
end

local spawn = function(func) 
    return coroutine.wrap(func)()
end

local function BindToStepped(name, func)
    if SteppedTable[name] == nil then
        SteppedTable[name] = game:GetService("RunService").Stepped:connect(func)
    end
end

local function anticheatdisabler() 
    character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, true)
    character.Humanoid:ChangeState(Enum.HumanoidStateType.Dead)
    --repeat task.wait() until character.Humanoid.MoveDirection ~= Vector3.zero
    task.wait(0.2)
    character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
    character.Humanoid:ChangeState(Enum.HumanoidStateType.Running)
    workspace.Gravity = 192.6
end

spawn(function()
    pcall(function()
        while true do
            game:GetService("ReplicatedStorage").rbxts_include.node_modules.net.out._NetManaged.GroundHit:FireServer()
            wait(0.52)
        end
    end)
end)

--[[
local On = Instance.new("Sound")
On.SoundId = "rbxassetid://9043887091" -- this is a lofi beat some reason
On.Parent = workspace
On.Volume = 5


local Off = Instance.new("Sound")
Off.SoundId = "rbxassetid://9991593671"
Off.Parent = workspace
Off.Volume = 10
--]]

local Hit = Instance.new("Sound")
Hit.SoundId = "rbxassetid://6361963422"
Hit.Parent = workspace
Hit.Volume = 3

----------------------------------------------

local Lofi1 = Instance.new("Sound")
Lofi1.SoundId = "rbxassetid://9042722976"
Lofi1.Parent = workspace
Lofi1.Volume = 1
Lofi1.Looped = true

local Lofi2 = Instance.new("Sound")
Lofi2.SoundId = "rbxassetid://9048039534"
Lofi2.Parent = workspace
Lofi2.Volume = 1
Lofi2.Looped = true

local Lofi3 = Instance.new("Sound")
Lofi3.SoundId = "rbxassetid://9047053092"
Lofi3.Parent = workspace
Lofi3.Volume = 1
Lofi3.Looped = true

local Lofi4 = Instance.new("Sound")
Lofi4.SoundId = "rbxassetid://9040313420"
Lofi4.Parent = workspace
Lofi4.Volume = 1
Lofi4.Looped = true

local Lofi5 = Instance.new("Sound")
Lofi5.SoundId = "rbxassetid://9047051355"
Lofi5.Parent = workspace
Lofi5.Volume = 1
Lofi5.Looped = true

-----------------------------------------------------------------------------------------FUNCS
local KnitClient = debug.getupvalue(require(lplr.PlayerScripts.TS.controllers.game["block-break-controller"]).BlockBreakController.onEnable, 1)
local Client = require(game:GetService("ReplicatedStorage").TS.remotes).default.Client

local bedwars = {
    ["sprintTable"] = KnitClient.Controllers.SprintController,
    ["SwordController"] = KnitClient.Controllers.SwordController,
    ["AttackRemote"] = getremote(debug.getconstants(getmetatable(KnitClient.Controllers.SwordController)["attackEntity"])),
    ["SwingSwordRegion"] = getmetatable(KnitClient.Controllers.SwordController).swingSwordInRegion,
    ["KnockbackTable"] = debug.getupvalue(require(game:GetService("ReplicatedStorage").TS.damage["knockback-util"]).KnockbackUtil.calculateKnockbackVelocity, 1),
    ["KnockbackTable2"] = require(game:GetService("ReplicatedStorage").TS.damage["knockback-util"]).KnockbackUtil,
    ["VelocityUtil"]  = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["game-core"].out["shared"].util["velocity-util"]).VelocityUtil,
    ["KnockbackTable"] = debug.getupvalue(require(game:GetService("ReplicatedStorage").TS.damage["knockback-util"]).KnockbackUtil.calculateKnockbackVelocity, 1),
    ["KnockbackTable2"] = require(game:GetService("ReplicatedStorage").TS.damage["knockback-util"]).KnockbackUtil,
    ["Remotes"] = {},
    ["QueueMeta"] = require(game:GetService("ReplicatedStorage").TS.game["queue-meta"]).QueueMeta,
    --["LobbyClientEvents"] = (require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"].lobby.out.client.events).LobbyClientEvents),
    ["ClientHandler"] = Client,
    ["ItemTable"] = debug.getupvalue(require(repstorage.TS.item["item-meta"]).getItemMeta, 1),
    ["SoundManager"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["game-core"].out).SoundManager,
    ["SoundList"] = require(game:GetService("ReplicatedStorage").TS.sound["game-sound"]).GameSound,
    ["CombatConstant"] = require(repstorage.TS.combat["combat-constant"]).CombatConstant,
    ["AnimationUtil"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["game-core"].out["shared"].util["animation-util"]).AnimationUtil,
    ["ViewmodelController"] = KnitClient.Controllers.ViewmodelController,
            ["getInventory"] = function(plr)
        local plr = plr or lplr
        local suc, result = pcall(function() return InventoryUtil.getInventory(plr) end)
        return (suc and result or {
            ["items"] = {},
            ["armor"] = {},
            ["hand"] = nil
        })
    end,
    ["CheckWhitelisted"] = function(plr, ownercheck)
    end,
    ["getEntityTable"] = require(game:GetService("ReplicatedStorage").TS.entity["entity-util"]).EntityUtil,
    ["ClientStoreHandler"] = require(lplr.PlayerScripts.TS.ui.store).ClientStore,
    
}

local oldveloh, oldvelov, oldvelofunc = bedwars["KnockbackTable"]["kbDirectionStrength"], bedwars["KnockbackTable"]["kbUpwardStrength"], bedwars["VelocityUtil"].applyVelocity

----------------------------------------------------------------------------------------------------VARS




coroutine.wrap(function()
    if not (game.CoreGui:FindFirstChild('Sigma_1') == nil) then
        game.CoreGui.Sigma_1:Destroy()
    end
end)()


function entityFunction()
    local entity = {
        entityList = {},
        entityConnections = {},
        isAlive = false,
        character = {
            Head = {},
            Humanoid = {},
            HumanoidRootPart = {}
        }
    }
    local lplr = game:GetService("Players").LocalPlayer

    do
        entity.isPlayerTargetable = function(plr)
            if plr.Team ~= lplr.Team or lplr.Team == nil then
                return true
            end
        end

        entity.getEntityFromPlayer = function(char)
            for i,v in pairs(entity.entityList) do
                if v.Player == char then
                    return i, v
                end
            end
            return nil
        end

        entity.removeEntity = function(obj)
            local tableIndex = entity.getEntityFromPlayer(obj)
            if tableIndex then
                table.remove(entity.entityList, tableIndex)
            end
        end

        entity.refreshEntity = function(plr, localcheck)
            entity.removeEntity(plr)
            entity.characterAdded(plr, plr.Character, localcheck)
        end

        entity.characterAdded = function(plr, char, localcheck)
            if char then
                spawn(function()
                    local humrootpart = char:WaitForChild("HumanoidRootPart", 10)
                    local head = char:WaitForChild("Head", 10)
                    local hum = char:WaitForChild("Humanoid", 10)
                    if humrootpart and hum and head then
                        if localcheck then
                            entity.isAlive = true
                            entity.character.Head = head
                            entity.character.Humanoid = hum
                            entity.character.HumanoidRootPart = humrootpart
                        else
                            table.insert(entity.entityList, {
                                Player = plr,
                                Character = char,
                                RootPart = humrootpart,
                                Head = head,
                                Humanoid = hum,
                                Targetable = entity.isPlayerTargetable(plr),
                                Team = plr.Team
                            })
                        end
                        entity.entityConnections[#entity.entityConnections + 1] = char.ChildRemoved:connect(function(part)
                            if part.Name == "HumanoidRootPart" or part.Name == "Head" or part.Name == "Humanoid" then
                                if localcheck then
                                    entity.isAlive = false
                                else
                                    entity.removeEntity(plr)
                                end
                            end
                        end)
                    end
                end)
            end
        end

        entity.entityAdded = function(plr, localcheck, custom)
            entity.entityConnections[#entity.entityConnections + 1] = plr.CharacterAdded:connect(function(char)
                entity.refreshEntity(plr, localcheck)
            end)
            entity.entityConnections[#entity.entityConnections + 1] = plr.CharacterRemoving:connect(function(char)
                if localcheck then
                    entity.isAlive = false
                else
                    entity.removeEntity(plr)
                end
            end)
            entity.entityConnections[#entity.entityConnections + 1] = plr:GetPropertyChangedSignal("Team"):connect(function()
                if localcheck then
                    entity.fullEntityRefresh()
                else
                    task.wait(0.5)
                    entity.refreshEntity(plr, localcheck)
                end
            end)
            if plr.Character then
                entity.refreshEntity(plr, localcheck)
            end
        end

        entity.fullEntityRefresh = function()
            entity.entityList = {}
            for i,v in pairs(entity.entityConnections) do if v.Disconnect then v:Disconnect() end end
            entity.entityConnections = {}
            for i2,v2 in pairs(game:GetService("Players"):GetPlayers()) do entity.entityAdded(v2, v2 == lplr) end
            entity.entityConnections[#entity.entityConnections + 1] = game:GetService("Players").PlayerAdded:connect(function(v2) entity.entityAdded(v2, v2 == lplr) end)
            entity.entityConnections[#entity.entityConnections + 1] = game:GetService("Players").PlayerRemoving:connect(function(v2) entity.removeEntity(v2) end)
        end
    end

    return entity
end
local entity = entityFunction()


spawn(function()
    while true do
        if (killaurastatson == false)  then
            MainGui.Visible = false

        elseif killaurastatson == true and (not (attacking == nil)) then
            MainGui.Visible = true
        end
        wait()
    end
end)


-- Made By LeakedCombat
-- Instances:

lplr = game.Players.LocalPlayer

 SoundService = game:GetService("SoundService")

 camera = game.workspace.CurrentCamera
 RunService = game:GetService("RunService")
 Heartbeat = RunService.Heartbeat

 VirtualInputManager = game:GetService("VirtualInputManager")
 UIS = game:GetService("UserInputService")

 WORKSPACE = game.Workspace

----------------------------------------------------------------

 TargetInfo = Instance.new("ScreenGui")
 MainGui = Instance.new("Frame")
 UICorner = Instance.new("UICorner")
 Name = Instance.new("TextLabel")
 PlayerFace = Instance.new("ImageLabel")
 HealthBar = Instance.new("Frame")
 UICorner_2 = Instance.new("UICorner")
 HealthLeft = Instance.new("TextLabel")
 EmptyHealthBar = Instance.new("Frame")
 UICorner_3 = Instance.new("UICorner")
 WarningAndInfoText = Instance.new("ScreenGui")
 HUD = Instance.new("Frame")
 Cheat1 = Instance.new("TextLabel")
 Cheat2 = Instance.new("TextLabel")
 Cheat3 = Instance.new("TextLabel")
 Cheat4 = Instance.new("TextLabel")
 Cheat5 = Instance.new("TextLabel")
 Cheat6 = Instance.new("TextLabel")
 Cheat7 = Instance.new("TextLabel")
 Cheat9 = Instance.new("TextLabel")
 Cheat10 = Instance.new("TextLabel")
 Cheat8 = Instance.new("TextLabel")
 Cheat11 = Instance.new("TextLabel")
 Cheat12 = Instance.new("TextLabel")
 Cheat13 = Instance.new("TextLabel")
 Cheat14 = Instance.new("TextLabel")
 Cheat15 = Instance.new("TextLabel")
 Cheat18 = Instance.new("TextLabel")
 Cheat20 = Instance.new("TextLabel")
 Cheat16 = Instance.new("TextLabel")
 Cheat17 = Instance.new("TextLabel")
 Cheat19 = Instance.new("TextLabel")
 Cheat21 = Instance.new("TextLabel")
 Cheat26 = Instance.new("TextLabel")
 Cheat30 = Instance.new("TextLabel")
 Cheat28 = Instance.new("TextLabel")
 Cheat29 = Instance.new("TextLabel")
 Cheat31 = Instance.new("TextLabel")
 Cheat22 = Instance.new("TextLabel")
 Cheat27 = Instance.new("TextLabel")
 Cheat32 = Instance.new("TextLabel")
 Cheat23 = Instance.new("TextLabel")
 Cheat25 = Instance.new("TextLabel")
 Cheat33 = Instance.new("TextLabel")
 Cheat24 = Instance.new("TextLabel")
 Cheat34 = Instance.new("TextLabel")
 Cheat40 = Instance.new("TextLabel")
 Cheat36 = Instance.new("TextLabel")
 Cheat37 = Instance.new("TextLabel")
 Cheat35 = Instance.new("TextLabel")
 Cheat38 = Instance.new("TextLabel")
 Cheat39 = Instance.new("TextLabel")
 WarningAndInfo = Instance.new("TextLabel")

local NoFallCheatOn = false
local AutoSprintCheatOn = false
local SpinBotCheatOn = false
local FastDropCheatOn = false
local AutoToxicCheatOn = false
local AutoSpamCheatOn = false
local CapeCheatOn = false
local KillAuraCheatOn = false
local ReachCheatOn = false
local AntiKnockBackCheatOn = false
local AutoClickerCheatOn = false
local NoClickDelayCheatOn = false
local MultiAuraCheatOn = false
local SpeedCheatOn = false
local BhopCheatOn = false
local FlyCheatOn = false
local LongJumpCheatOn = false
local HighJumpCheatOn = false
local Fly2CheatOn = false
local PhaseCheatOn = false
local BigSwordCheatOn = false
local ChestStealerCheatOn = false
local ScaffoldCheatOn = false
local HUDCheatOn = false
local ChillLofi1CheatOn = false
local ChillLofi2CheatOn = false
local ChillLofi3CheatOn = false
local ChillLofi4CheatOn = false
local ChillLofi5CheatOn = false
local AntiVoidCheatOn = false
local TimerCheatOn = false
local FPSBoostCheatOn = false
local FastFallCheatOn = false
local ClickTPCheatOn = false
local AntiCheatDisablerCheatOn = false
local AutoQueueCheatOn = false
local AntiAFKCheatOn = false
local CopyDiscordLinkCheatOn = false
local DinoFlyCheatOn = false
local BlatantModeCheatOn = false
local AutoSkywarsWinCheatOn = false
local ESPCheatOn = false
local TracersCheatOn = false
local FOVCheatOn = false
local ChamsCheatOn = false
local CameraNoClipCheatOn = false
local EmeraldESPCheatOn = false

FLYINPUTVALUE = false


local SteppedTable = {}


local attacking = nil
local killaurastatson = false

spawn(function()
    while true do
        if attacking == false then
            MainGui.Visible = false

            elseif attacking == true then
                MainGui.Visible = true
        end
        wait()
    end
end)

TargetInfo.Name = "TargetInfo"
TargetInfo.Parent = game.CoreGui
TargetInfo.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
TargetInfo.ResetOnSpawn = false

MainGui.Name = "MainGui"
MainGui.Parent = TargetInfo
MainGui.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainGui.BackgroundTransparency = 0.450
MainGui.BorderColor3 = Color3.fromRGB(0, 0, 0)
MainGui.BorderSizePixel = 0
MainGui.Position = UDim2.new(0.5, 0, 0.6, 0)
MainGui.Size = UDim2.new(0, 334, 0, 141)
MainGui.Active = true
MainGui.Visible = false

UICorner.CornerRadius = UDim.new(0, 25)
UICorner.Parent = MainGui

Name.Name = "Name"
Name.Parent = MainGui
Name.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
Name.BackgroundTransparency = 1.000
Name.BorderColor3 = Color3.fromRGB(255, 255, 255)
Name.BorderSizePixel = 0
Name.Position = UDim2.new(0.376097918, 0, 0.048488345, 0)
Name.Size = UDim2.new(0, 208, 0, 50)
Name.Font = Enum.Font.RobotoCondensed
Name.Text = "Name:"
Name.TextColor3 = Color3.fromRGB(255, 255, 255)
Name.TextSize = 32.000
Name.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
Name.TextXAlignment = Enum.TextXAlignment.Left

PlayerFace.Name = "PlayerFace"
PlayerFace.Parent = MainGui
PlayerFace.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
PlayerFace.BorderSizePixel = 0
PlayerFace.Position = UDim2.new(0.0476076044, 0, 0.14052695, 0)
PlayerFace.Size = UDim2.new(0, 100, 0, 100)
PlayerFace.Image = "rbxasset://textures/ui/GuiImagePlaceholder.png"


HealthBar.Name = "HealthBar"
HealthBar.Parent = MainGui
HealthBar.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
HealthBar.BorderSizePixel = 0
HealthBar.Position = UDim2.new(0.375647813, 0, 0.599836648, 0)
HealthBar.Size = UDim2.new(0.581269741, 0, 0.245628059, 0)
HealthBar.ZIndex = 10

UICorner_2.CornerRadius = UDim.new(0, 10)
UICorner_2.Parent = HealthBar

HealthLeft.Name = "HealthLeft"
HealthLeft.Parent = MainGui
HealthLeft.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
HealthLeft.BackgroundTransparency = 1.000
HealthLeft.BorderSizePixel = 0
HealthLeft.Position = UDim2.new(0.298039824, 0, 0.274283826, 0)
HealthLeft.Size = UDim2.new(0, 200, 0, 50)
HealthLeft.Font = Enum.Font.RobotoCondensed
HealthLeft.Text = "100 health left"
HealthLeft.TextColor3 = Color3.fromRGB(255, 255, 255)
HealthLeft.TextSize = 30.000

EmptyHealthBar.Name = "EmptyHealthBar"
EmptyHealthBar.Parent = MainGui
EmptyHealthBar.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
EmptyHealthBar.BorderSizePixel = 0
EmptyHealthBar.Position = UDim2.new(0.375647813, 0, 0.599836648, 0)
EmptyHealthBar.Size = UDim2.new(0.581269741, 0, 0.245628059, 0)

UICorner_3.CornerRadius = UDim.new(0, 10)
UICorner_3.Parent = EmptyHealthBar

spawn(function()
    function Round(n, decimals)
        decimals = decimals or 0
        return math.floor(n * 10^decimals) / 10^decimals
        
    end


    while true do
        while true do
            pcall(function()
                if not (attacking==nil) then
                    MainGui.Visible = true



                    UsedId = attacking.UserId



                    local thumbType = Enum.ThumbnailType.HeadShot
                    local thumbSize = Enum.ThumbnailSize.Size420x420
                    local content, isReady = game.Players:GetUserThumbnailAsync(UsedId, thumbType, thumbSize)

                    PlayerFace.Image = content



                    Name.Text = attacking.Name


                    local healthbar = HealthBar
                    local healthbarsize = HealthBar.Size.X.Offset
                    local playerChar = attacking.Character
                    local playerHumanoid = attacking.Character:WaitForChild("Humanoid")

                    local healthbarxsize = 0.00581 * playerHumanoid.Health
                    HealthLeft.Text = tostring(Round(playerHumanoid.Health)) .. " health left"
                    HealthBar.Size = UDim2.new(healthbarxsize, 0, 0.246, 0)
                    else
                    MainGui.Visible = false
                end 
            end)
            wait()
        end
    Heartbeat:wait()
    end
end)




WarningAndInfoText.Name = "WarningAndInfoText"
WarningAndInfoText.Parent = game.CoreGui
WarningAndInfoText.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

HUD.Name = "HUD"
HUD.Parent = WarningAndInfoText
HUD.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
HUD.BackgroundTransparency = 1.000
HUD.BorderColor3 = Color3.fromRGB(0, 0, 0)
HUD.BorderSizePixel = 0
HUD.Position = UDim2.new(-0.894999981, 1, -0.00300000003, 1)
HUD.Size = UDim2.new(0, 143, 0, 370)
HUD.Visible = true



----------------------------------------------------------------

local Sigma_1 = Instance.new("ScreenGui")
local Player = Instance.new("Frame")
local PlayerText = Instance.new("TextLabel")
local Cover1 = Instance.new("Frame")
local NoFall = Instance.new("TextButton")
local AutoSprint = Instance.new("TextButton")
local SpinBot = Instance.new("TextButton")
local FastDrop = Instance.new("TextButton")
local AutoToxic = Instance.new("TextButton")
local AutoSpam = Instance.new("TextButton")
local Cape = Instance.new("TextButton")
local Combat = Instance.new("Frame")
local CombatText = Instance.new("TextLabel")
local Cover2 = Instance.new("Frame")
local KillAura = Instance.new("TextButton")





--Properties:

Sigma_1.Name = "Sigma_1"
Sigma_1.Parent = game.CoreGui

--

Cover1.Name = "Cover1"
Cover1.Parent = Player
Cover1.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover1.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover1.BorderSizePixel = 0
Cover1.Position = UDim2.new(0.60799998, 0, 0, 0)
Cover1.Size = UDim2.new(0, 80, 0, 63)







Combat.Name = "Combat"
Combat.Parent = Sigma_1
Combat.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Combat.BorderColor3 = Color3.fromRGB(255, 255, 255)
Combat.BorderSizePixel = 0
Combat.Position = UDim2.new(0.254558742, 0, 0.0327613354, 0)
Combat.Size = UDim2.new(0, 204, 0, 291)
Combat.Visible = true
Combat.Active = true
Combat.Draggable = true

CombatText.Name = "CombatText"
CombatText.Parent = Combat
CombatText.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
CombatText.BorderColor3 = Color3.fromRGB(0, 0, 0)
CombatText.BorderSizePixel = 0
CombatText.Position = UDim2.new(0, 0, -0.00105012977, 0)
CombatText.Size = UDim2.new(0, 124, 0, 63)
CombatText.Font = Enum.Font.Roboto
CombatText.Text = "Combat"
CombatText.TextColor3 = Color3.fromRGB(255, 0, 0)
CombatText.TextSize = 32.000

Cover2.Name = "Cover2"
Cover2.Parent = Combat
Cover2.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Cover2.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover2.BorderSizePixel = 0
Cover2.Position = UDim2.new(0.60799998, 0, 0, 0)
Cover2.Size = UDim2.new(0, 80, 0, 63)

clientstorestate = bedwars["ClientStoreHandler"]:getState()
currentinventory = clientstorestate.Inventory.observedInventory

local killauratargetframe = {["Players"] = {["Enabled"] = false}}
local killaurarealremote = bedwars["ClientHandler"]:Get(bedwars["AttackRemote"])["instance"]
local killauracframe = {["Enabled"] = false}
local killauratarget = {["Enabled"] = false}
local killaurahitdelay = tick()
local Killauranear = false
-- local oldplay = function() end
-- local oldsound = function() end
local origC0 = nil

local cam = workspace.CurrentCamera


local function playanimation(id) 
    if isAlive() then 
        local animation = Instance.new("Animation")
        animation.AnimationId = id
        local animatior = lplr.Character.Humanoid.Animator
        animatior:LoadAnimation(animation):Play()
    end
end

local RunLoops = {HeartTable = {}}

function RunLoops:BindToHeartbeat(name, num, func)
    if RunLoops.HeartTable[name] == nil then
        RunLoops.HeartTable[name] = game:GetService("RunService").Heartbeat:connect(func)
    end
end

function RunLoops:UnbindFromHeartbeat(name)
    if RunLoops.HeartTable[name] then
        RunLoops.HeartTable[name]:Disconnect()
        RunLoops.HeartTable[name] = nil
    end
end

local function getSword()
	local bestsword, bestswordslot, bestswordnum = nil, nil, 0
    local clientstorestate = bedwars["ClientStoreHandler"]:getState()
    local currentinventory = clientstorestate.Inventory.observedInventory
    items = currentinventory.inventory.items
    
	for i5, v5 in pairs(items) do
		if bedwars["ItemTable"][v5.itemType]["sword"] then
			local swordrank = bedwars["ItemTable"][v5.itemType]["sword"]["damage"] or 0
			if swordrank > bestswordnum then
				bestswordnum = swordrank
				bestswordslot = i5
				bestsword = v5
			end
		end
	end

	return bestsword, bestswordslot
end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local function GetAllNearestHumanoidToPosition(distance)
    local Character = LocalPlayer.Character
    local HumanoidRootPart = Character and Character:FindFirstChild("HumanoidRootPart")
    if not (Character or HumanoidRootPart) then return {nil} end

    local TargetDistance = distance 
    local Target

    for i,v in pairs(Players:GetPlayers()) do
        if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
            if v.Team ~= LocalPlayer.Team then
                if v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health > 0 then
                    local TargetHRP = v.Character.HumanoidRootPart
                    if TargetHRP and HumanoidRootPart then
                        local mag = (HumanoidRootPart.Position - TargetHRP.Position).magnitude
                        if mag < TargetDistance then
                            TargetDistance = mag
                            Target = v
                        end
                    end
                end
            end
        end
    end

    return {Target}
end

local function getStrength(plr)
    local inv = inventories[plr.Player]
    local strength = 0
    local strongestsword = 0
    if inv then
        for i,v in pairs(inv.items) do 
            local itemmeta = bedwars["ItemTable"][v.itemType]
            if itemmeta and itemmeta.sword and itemmeta.sword.damage > strongestsword then 
                strongestsword = itemmeta.sword.damage / 100
            end	
        end
        strength = strength + strongestsword
        for i,v in pairs(inv.armor) do 
            local itemmeta = bedwars["ItemTable"][v.itemType]
            if itemmeta and itemmeta.armor then 
                strength = strength + (itemmeta.armor.damageReductionMultiplier or 0)
            end
        end
        strength = strength
    end
    return strength
end

local function getEquipped()
	local typetext = ""
	local obj = currentinventory.inventory.hand
	if obj then
		local metatab = bedwars["ItemTable"][obj.itemType]
		typetext = metatab.sword and "sword" or metatab.block and "block" or obj.itemType:find("bow") and "bow"
	end
    return {["Object"] = obj and obj.tool, ["Type"] = typetext}
end

local function newAttackEntity(plr, firstplayercodedone, attackedplayers)
    if game.Players.LocalPlayer.Character.Humanoid.Health <= 0 then
        return nil
    end
    local root = plr.Character.HumanoidRootPart
    if not root then 
        return nil
    end
    local sword = getSword()
    if not (sword and sword["tool"]) then
        return nil
    end
    if (workspace:GetServerTimeNow() - bedwars["SwordController"].lastAttack) < bedwars["ItemTable"][sword["tool"].Name].sword.attackSpeed then 
        return nil
    end
    local selfroot = (game.Players.LocalPlayer.Character.HumanoidRootPart)
    local selfrootpos = selfroot.Position
    local selfcheck = selfrootpos - (selfroot.Velocity * 0.164)
    if (selfcheck - (root.Position + (root.Velocity * 0.05))).Magnitude > 18 then 
        return nil
    end

    local selfpos = selfrootpos + (18 > 14 and (selfrootpos - root.Position).magnitude > 14 and (CFrame.lookAt(selfrootpos, root.Position).lookVector * 4) or Vector3.zero)
    bedwars["SwordController"].lastAttack = workspace:GetServerTimeNow() - 0.11
    killaurarealremote:FireServer({
        ["weapon"] = sword["tool"],
        ["chargedAttack"] = {chargeRatio = 1},
        ["entityInstance"] = plr.Character,
        ["validate"] = {
            ["raycast"] = {
                ["cameraPosition"] = hashvec(cam.CFrame.p), 
                ["cursorDirection"] = hashvec(Ray.new(cam.CFrame.p, root.Position).Unit.Direction)
            },
            ["targetPosition"] = hashvec(root.Position),
            ["selfPosition"] = hashvec(selfpos)
        }
    })
end

function StrengthSorting(a, b) 
    return getStrength(a) > getStrength(b)
end

KillAura.Name = "KillAura"
KillAura.Parent = Combat
KillAura.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
KillAura.BorderColor3 = Color3.fromRGB(0, 0, 0)
KillAura.BorderSizePixel = 0
KillAura.Position = UDim2.new(0, 0, 0.213058412, 0)
KillAura.Size = UDim2.new(0, 204, 0, 31)
KillAura.Font = Enum.Font.Roboto
KillAura.Text = "Moon Aura"
KillAura.TextColor3 = Color3.fromRGB(0, 0, 0)
KillAura.TextSize = 25.000
KillAura.MouseButton1Down:connect(function()
    if not KillAuraCheatOn == true then

        KillAuraCheatOn = true

        KillAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        KillAura.TextColor3 = Color3.fromRGB(255, 255, 255)

        KillAuraCheatOnSave = true

        --On:Play() 

        local playerentity
        task.spawn(function()
            repeat task.wait() plrentity = bedwars["getEntityTable"].getLocalPlayerEntity() until playerentity ~= nil
        end)
        local targetedplayer
        RunLoops:BindToHeartbeat("Killaura", 1, function()
            if game.Players.LocalPlayer.Character.Humanoid.Health > 1 then
                local Root = game.Players.LocalPlayer.Character.HumanoidRootPart
                if Root then
                    local Neck = game.Players.LocalPlayer.Character.Head:FindFirstChild("Neck")
                    local LowerTorso = Root.Parent and Root.Parent:FindFirstChild("LowerTorso")
                    local RootC0 = LowerTorso and LowerTorso:FindFirstChild("Root")
                    if Neck and RootC0 then
                        if orig == nil then
                            orig = Neck.C0.p
                        end
                        if orig2 == nil then
                            orig2 = RootC0.C0.p
                        end
                        if orig2 then
                            if targetedplayer ~= nil then 
                                local targetPos = targetedplayer.RootPart.Position + Vector3.new(0, 2, 0)
                                local direction = (Vector3.new(targetPos.X, targetPos.Y, targetPos.Z) - game.Players.LocalPlayer.Character.Head.Position).Unit
                                local direction2 = (Vector3.new(targetPos.X, Root.Position.Y, targetPos.Z) - Root.Position).Unit
                                local lookCFrame = (CFrame.new(Vector3.zero, (Root.CFrame):VectorToObjectSpace(direction)))
                                local lookCFrame2 = (CFrame.new(Vector3.zero, (Root.CFrame):VectorToObjectSpace(direction2)))
                                Neck.C0 = CFrame.new(orig) * CFrame.Angles(lookCFrame.LookVector.Unit.y, 0, 0)
                                RootC0.C0 = lookCFrame2 + orig2
                            else
                                Neck.C0 = CFrame.new(orig)
                                RootC0.C0 = CFrame.new(orig2)
                            end
                        end
                    end
                end
            end
        end)

        task.spawn(function()
            repeat
                --pcall(function()
                task.wait(0.03)
                if KillAuraCheatOn then


                    local plrs = GetAllNearestHumanoidToPosition(18 - 0.0001)

                    if plrs[1] == nil then
                        attacking = nil
                    end

                    local attackedplayers = {}
                    local firstplayercodedone = {done = false}
                    for i,plr in pairs(plrs) do
                        attacking = plr
                        task.spawn(newAttackEntity, plr, firstplayercodedone, attackedplayers)
                    end


                end
                --end)
            until KillAuraCheatOn == false
        end)



        
    elseif KillAuraCheatOn == true then
        KillAuraCheatOn = false

        killaurastatson = false

        KillAura.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
        KillAura.TextColor3 = Color3.fromRGB(0, 0, 0)   

        KillAuraCheatOnSave = false

        --Off:Play()

        RunLoops:UnbindFromHeartbeat("Killaura") 
        pcall(function()
            if true then
                local Root =game.Players.LocalPlayer.Character.HumanoidRootPart
                if Root then
                    local Neck = Root.Parent.Head.Neck
                    if orig and orig2 then 
                        Neck.C0 = CFrame.new(orig)
                        Root.Parent.LowerTorso.Root.C0 = CFrame.new(orig2)
                    end
                end
            end
            if origC0 == nil then
                origC0 = cam.Viewmodel.RightHand.RightWrist.C0
            end
        end)

    end
end)


local AuraAnimationList = {

    Normal = {
        Animation = {
            {CFrame = CFrame.new(0.69, -0.7, 0.6) * CFrame.Angles(math.rad(295), math.rad(55), math.rad(290)), Time = 0.1},
            {CFrame = CFrame.new(0.69, -0.71, 0.6) * CFrame.Angles(math.rad(200), math.rad(60), math.rad(1)), Time = 0.1}
        },  
        TweenTo = {CFrame = CFrame.new(0.69, -0.7, 0.6) * CFrame.Angles(math.rad(295), math.rad(55), math.rad(290)), Time = 0.1}
    },

}

local AuraAnimations = {}
for i,v in next, AuraAnimationList do 
    AuraAnimations[#AuraAnimations+1] = i
end

local AuraAnimation = {Value = "Animation"}

coroutine.wrap(function()
    while true do
        repeat task.wait()
            setc0 = setc0 or savedc0

            if attacking then
                
                playanimation("rbxassetid://4947108314")

                    cancelViewmodel = true

                    
                    animationFrame1 = CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(350), math.rad(45), math.rad(85))
                    animationFrame2 = CFrame.new(0, 0, 0) * CFrame.Angles(math.rad(350), math.rad(80), math.rad(60))
                    waitTime = .12

                    if not tweenedTo then 
                                      print('e2')
                        tweenedTo = true
       
                        local Tween = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(waitTime), {C0 = setc0 * animationFrame1})
                        Tween:Play()
                        task.wait(waitTime)
                    end
     
                    for i,v in next, {{CFrame = animationFrame1, Time = 0.1},{CFrame = animationFrame2, Time = 0.1}} do 
                        if not KillAuraCheatOn or not attacking then break end
                        --print('e3')
                        local Tween = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(v.Time), {C0 = setc0 * v.CFrame})
                        Tween:Play()
                        task.wait(v.Time)
                    end

               -- end)()
                

            else
            --  GuiLibrary["TargetHUDAPI"].clear()
                if tweenedTo then
                    print('e4')
                    cancelViewmodel = true
                    tweenedTo = false
                    local v = {CFrame = CFrame.new(0.69, -0.7, 0.6) * CFrame.Angles(math.rad(295), math.rad(55), math.rad(290)), Time = 0.1}
                    local Tween = game:GetService("TweenService"):Create(cam.Viewmodel.RightHand.RightWrist, TweenInfo.new(0.1), {C0 = setc0})
                    Tween:Play()
                    task.wait(v.Time - 0.01)
                    cancelViewmodel = false
                end
            end

        until not KillAuraCheatOn
        wait()
    end
end)()


KillAuraKeyBind = Enum.KeyCode.Escape.Value

KillAura.MouseButton2Down:connect(function()
    print('clicked')
    KillAura.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                KillAuraKeyBind = Enum.KeyCode.Escape.Value
            else 
                KillAuraKeyBind = input.KeyCode.Value
            end
            selecting = false
            KillAura.Text = "KillAura"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == KillAuraKeyBind then
        for i,v in pairs(getconnections(KillAura.MouseButton1Down)) do
            print('fire')
            v:Fire() 
        end  
    end
end)

Reach.Name = "Reach"
Reach.Parent = Combat
Reach.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Reach.BorderColor3 = Color3.fromRGB(0, 0, 0)
Reach.BorderSizePixel = 0
Reach.Position = UDim2.new(0, 0, 0.319587618, 0)
Reach.Size = UDim2.new(0, 204, 0, 31)
Reach.Font = Enum.Font.Roboto
Reach.Text = "Reach"
Reach.TextColor3 = Color3.fromRGB(0, 0, 0)
Reach.TextSize = 25.000
Reach.MouseButton1Down:connect(function()
    if not ReachCheatOn == true then
        ReachCheatOn = true

        Reach.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Reach.TextColor3 = Color3.fromRGB(255, 255, 255)    

        ReachCheatOnSave = true

        --On:Play()

        spawn(function()
            plr = game.Players.LocalPlayer
            lplr = game.Players.LocalPlayer

            local PLAYERS = game:GetService("Players")
            local Flamework = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@flamework"].core.out).Flamework
            repeat task.wait() until Flamework.isInitialized
            local KnitClient = debug.getupvalue(require(lplr.PlayerScripts.TS.controllers.game["block-break-controller"]).BlockBreakController.onEnable, 1)
            local Client = require(game:GetService("ReplicatedStorage").TS.remotes).default.Client
            local InventoryUtil = require(game:GetService("ReplicatedStorage").TS.inventory["inventory-util"]).InventoryUtil
            local OldClientGet = getmetatable(Client).Get
            local OldClientWaitFor = getmetatable(Client).WaitFor
            getmetatable(Client).Get = function(Self, remotename)
                if remotename == bedwars["AttackRemote"] then
                    local res = OldClientGet(Self, remotename)
                    return {
                        ["instance"] = res["instance"],
                        ["CallServer"] = function(Self, tab)
                            if ReachCheatOn then
                                local mag = (tab.validate.selfPosition.value - tab.validate.targetPosition.value).magnitude
                                local newres = hashvector(tab.validate.selfPosition.value + (mag > 14.4 and (CFrame.lookAt(tab.validate.selfPosition.value, tab.validate.targetPosition.value).lookVector * 4) or Vector3.new(0, 0, 0)))
                                tab.validate.selfPosition = newres
                            end
                            local suc, plr = pcall(function() return PLAYERS:GetPlayerFromCharacter(tab.entityInstance) end)
                            if suc and plr then
                                if plr and (bedwars["CheckWhitelisted"](plr) and bedwars["CheckWhitelisted"](lplr) == nil) then
                                    return nil
                                end
                            end
                            return res:CallServer(tab)
                        end
                    }
                end
                return OldClientGet(Self, remotename)
            end
        end)

        local reachConst1 = 14
        local reachConst2 = 18

        local old, old2 = debug.getconstant(bedwars["SwingSwordRegion"], reachConst1),debug.getconstant(bedwars["SwingSwordRegion"], reachConst2)
        local ReachValue = {["Value"] = 2}



        debug.setconstant(bedwars["SwingSwordRegion"], reachConst1, old*(ReachValue["Value"]+1))
        debug.setconstant(bedwars["SwingSwordRegion"], reachConst2, old2*(ReachValue["Value"]+1))




        elseif ReachCheatOn == true then
        ReachCheatOn = false

        Reach.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Reach.TextColor3 = Color3.fromRGB(0, 0, 0)

        ReachCheatOnSave = false

        ReachValue = {["Value"] = 0}

        --Off:Play() 
        
    end
end)

ReachKeyBind = Enum.KeyCode.Escape.Value

Reach.MouseButton2Down:connect(function()
    print('clicked')
    Reach.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ReachKeyBind = Enum.KeyCode.Escape.Value
            else 
                ReachKeyBind = input.KeyCode.Value
            end
            selecting = false
            Reach.Text = "Reach"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ReachKeyBind then
        for i,v in pairs(getconnections(Reach.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

AntiKnockBack.Name = "AntiKnockBack"
AntiKnockBack.Parent = Combat
AntiKnockBack.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AntiKnockBack.BorderColor3 = Color3.fromRGB(0, 0, 0)
AntiKnockBack.BorderSizePixel = 0
AntiKnockBack.Position = UDim2.new(0, 0, 0.426116824, 0)
AntiKnockBack.Size = UDim2.new(0, 204, 0, 31)
AntiKnockBack.Font = Enum.Font.Roboto
AntiKnockBack.Text = "AntiKnockBack"
AntiKnockBack.TextColor3 = Color3.fromRGB(0, 0, 0)
AntiKnockBack.TextSize = 25.000
AntiKnockBack.MouseButton1Down:connect(function()
    if not AntiKnockBackCheatOn == true then
        AntiKnockBackCheatOn = true

        AntiKnockBack.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AntiKnockBack.TextColor3 = Color3.fromRGB(255, 255, 255)  

        AntiKnockBackCheatOnSave = true

        --On:Play()

        bedwars["KnockbackTable"]["kbDirectionStrength"] = 0
        bedwars["KnockbackTable"]["kbUpwardStrength"] = 0

        bedwars["VelocityUtil"].applyVelocity = function(...) end

        bedwars["VelocityUtil"].applyVelocity = 0
        bedwars["KnockbackTable"]["kbDirectionStrength"] = 0
        bedwars["KnockbackTable"]["kbUpwardStrength"] = 0

        elseif AntiKnockBackCheatOn == true then
        AntiKnockBackCheatOn = false

        AntiKnockBack.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AntiKnockBack.TextColor3 = Color3.fromRGB(0, 0, 0) 

        AntiKnockBackCheatOnSave = false

        --Off:Play() 
        
    end
end)

AntiKnockBackKeyBind = Enum.KeyCode.Escape.Value

AntiKnockBack.MouseButton2Down:connect(function()
    print('clicked')
    AntiKnockBack.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AntiKnockBackKeyBind = Enum.KeyCode.Escape.Value
            else 
                AntiKnockBackKeyBind = input.KeyCode.Value
            end
            selecting = false
            AntiKnockBack.Text = "AntiKnockBack"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == AntiKnockBackKeyBind then
        for i,v in pairs(getconnections(AntiKnockBack.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

AutoClicker.Name = "AutoClicker"
AutoClicker.Parent = Combat
AutoClicker.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AutoClicker.BorderColor3 = Color3.fromRGB(0, 0, 0)
AutoClicker.BorderSizePixel = 0
AutoClicker.Position = UDim2.new(0, 0, 0.53264606, 0)
AutoClicker.Size = UDim2.new(0, 204, 0, 31)
AutoClicker.Font = Enum.Font.Roboto
AutoClicker.Text = "AutoClicker"
AutoClicker.TextColor3 = Color3.fromRGB(0, 0, 0)
AutoClicker.TextSize = 25.000
AutoClicker.MouseButton1Down:connect(function()
    if AutoClickerCheatOn == false then
        AutoClickerCheatOn = true


        AutoClicker.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AutoClicker.TextColor3 = Color3.fromRGB(255, 255, 255) 

        AutoClickerCheatOnSave = true

        --On:Play()

        local UIS = game:GetService("UserInputService")
        local held = false

        UIS.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                held = true
                while held == true do
                    bedwars["SwordController"]:swingSwordAtMouse()
                    game:GetService("RunService").RenderStepped:Wait()
                end
            end
        end)

        UIS.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                held = false
            end
        end)


    elseif AutoClickerCheatOn == true then
        AutoClickerCheatOn = false



        AutoClicker.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AutoClicker.TextColor3 = Color3.fromRGB(0, 0, 0)  

        AutoClickerCheatOnSave = false

        --Off:Play()
        
    end
end)


AutoClickerKeyBind = Enum.KeyCode.Escape.Value

AutoClicker.MouseButton2Down:connect(function()
    print('clicked')
    AutoClicker.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AutoClickerKeyBind = Enum.KeyCode.Escape.Value
            else 
                AutoClickerKeyBind = input.KeyCode.Value
            end
            selecting = false
            AutoClicker.Text = "AutoClicker"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == AutoClickerKeyBind then
        for i,v in pairs(getconnections(AutoClicker.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)


NoClickDelay.Name = "NoClickDelay"
NoClickDelay.Parent = Combat
NoClickDelay.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
NoClickDelay.BorderColor3 = Color3.fromRGB(0, 0, 0)
NoClickDelay.BorderSizePixel = 0
NoClickDelay.Position = UDim2.new(0, 0, 0.639175296, 0)
NoClickDelay.Size = UDim2.new(0, 204, 0, 31)
NoClickDelay.Font = Enum.Font.Roboto
NoClickDelay.Text = "NoClickDelay"
NoClickDelay.TextColor3 = Color3.fromRGB(0, 0, 0)
NoClickDelay.TextSize = 25.000
NoClickDelay.MouseButton1Down:connect(function()
    if not NoClickDelayCheatOn == true then
        NoClickDelayCheatOn = true

        NoClickDelay.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        NoClickDelay.TextColor3 = Color3.fromRGB(255, 255, 255) 

        NoClickDelayCheatOnSave = true

        --On:Play()

        getmetatable(bedwars["SwordController"]).isClickingTooFast = function(...) 
            return false
        end
        

        elseif NoClickDelayCheatOn == true then
        NoClickDelayCheatOn = false

        NoClickDelay.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        NoClickDelay.TextColor3 = Color3.fromRGB(0, 0, 0)  

        NoClickDelayCheatOnSave = false

        --Off:Play()
        
    end
end)

NoClickDelayKeyBind = Enum.KeyCode.Escape.Value

NoClickDelay.MouseButton2Down:connect(function()
    print('clicked')
    NoClickDelay.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                NoClickDelayKeyBind = Enum.KeyCode.Escape.Value
            else 
                NoClickDelayKeyBind = input.KeyCode.Value
            end
            selecting = false
            NoClickDelay.Text = "NoClickDelay"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == NoClickDelayKeyBind then
        for i,v in pairs(getconnections(NoClickDelay.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

MultiAura.Name = "MultiAura"
MultiAura.Parent = Combat
MultiAura.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
MultiAura.BorderColor3 = Color3.fromRGB(0, 0, 0)
MultiAura.BorderSizePixel = 0
MultiAura.Position = UDim2.new(0, 0, 0.745704532, 0)
MultiAura.Size = UDim2.new(0, 204, 0, 31)
MultiAura.Font = Enum.Font.Roboto
MultiAura.Text = "MultiAura"
MultiAura.TextColor3 = Color3.fromRGB(0, 0, 0)
MultiAura.TextSize = 25.000
MultiAura.MouseButton1Down:connect(function()
    if not MultiAuraCheatOn == true then
        MultiAuraCheatOn = true

        MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

        --On:Play()

        bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

        repeat
            task.wait(0.03)
            local plrs = GetAllNearestHumanoidToPosition(18.8)
            for i,plr in pairs(plrs) do
                local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                local newpos = plr.Character.HumanoidRootPart.Position
                bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
            end
        until MultiAuraCheatOn == false

    elseif MultiAuraCheatOn == true then
        MultiAuraCheatOn = false

        MultiAura.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        MultiAura.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()

    end
end)

MultiAuraKeyBind = Enum.KeyCode.Escape.Value

MultiAura.MouseButton2Down:connect(function()
    print('clicked')
    MultiAura.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                MultiAuraKeyBind = Enum.KeyCode.Escape.Value
            else 
                MultiAuraKeyBind = input.KeyCode.Value
            end
            selecting = false
            MultiAura.Text = "MultiAura"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == MultiAuraKeyBind then
        MultiAuraCheatOn = true

        MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

        --On:Play()

        bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

        repeat
            task.wait(0.03)
            local plrs = GetAllNearestHumanoidToPosition(18.8)
            for i,plr in pairs(plrs) do
                local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                local newpos = plr.Character.HumanoidRootPart.Position
                bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
            end
        until MultiAuraCheatOn == false 
    end
end)

Movement.Name = "Movement"
Movement.Parent = Sigma_1
Movement.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Movement.BorderColor3 = Color3.fromRGB(255, 255, 255)
Movement.BorderSizePixel = 0
Movement.Position = UDim2.new(0.435448587, 0, 0.0327613354, 0)
Movement.Size = UDim2.new(0, 204, 0, 291)
Movement.Visible = true
Movement.Active = true
Movement.Draggable = true

MovementText.Name = "MovementText"
MovementText.Parent = Movement
MovementText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
MovementText.BorderColor3 = Color3.fromRGB(0, 0, 0)
MovementText.BorderSizePixel = 0
MovementText.Position = UDim2.new(0, 0, -0.00105012977, 0)
MovementText.Size = UDim2.new(0, 167, 0, 63)
MovementText.Font = Enum.Font.Roboto
MovementText.Text = "Movement"
MovementText.TextColor3 = Color3.fromRGB(120, 120, 120)
MovementText.TextSize = 32.000

Cover3.Name = "Cover3"
Cover3.Parent = Movement
Cover3.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover3.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover3.BorderSizePixel = 0
Cover3.Position = UDim2.new(0.819000006, 0, 0, 0)
Cover3.Size = UDim2.new(0, 37, 0, 61)

Phase.Name = "Phase"
Phase.Parent = Movement
Phase.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Phase.BorderColor3 = Color3.fromRGB(0, 0, 0)
Phase.BorderSizePixel = 0
Phase.Position = UDim2.new(0, 0, 0.745704532, 0)
Phase.Size = UDim2.new(0, 204, 0, 31)
Phase.Font = Enum.Font.Roboto
Phase.Text = "Phase"
Phase.TextColor3 = Color3.fromRGB(0, 0, 0)
Phase.TextSize = 25.000

local function isAlive(plr, headCheck)
    local plr = plr or lplr
    if plr and plr.Character and ((plr.Character:FindFirstChild("Humanoid") and plr.Character:FindFirstChild("Humanoid").Health > 0) and (plr.Character:FindFirstChild("HumanoidRootPart")) and (headCheck and plr.Character:FindFirstChild("Head") or not headCheck)) then
        return true
    end
end

SteppedTablePhase = {}

local function BindToSteppedPhase(name, func)
    if SteppedTablePhase[name] == nil then
        SteppedTablePhase[name] = game:GetService("RunService").Stepped:connect(func)
    end
end

local function UnbindFromSteppedPhase(name)
	if SteppedTablePhase[name] then
		SteppedTablePhase[name]:Disconnect()
		SteppedTablePhase[name] = nil
	end
end

local lplr = game.Players.LocalPlayer
local cachedparts = {}


Phase.MouseButton1Down:connect(function()
    if not PhaseCheatOn == true then
        PhaseCheatOn = true

        Phase.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Phase.TextColor3 = Color3.fromRGB(255, 255, 255) 

        PhaseCheatOnSave = true

        --On:Play()

        BindToSteppedPhase("Phase", function()
            if isAlive() then
                for i,v in next, lplr.Character:GetDescendants() do 
                    if v:IsA("BasePart") and v.CanCollide then 
                        cachedparts[v] = v
                        v.CanCollide = false
                    end
                end
            end
        end)

    elseif PhaseCheatOn == true then
        PhaseCheatOn = false


        
        UnbindFromSteppedPhase("Phase")
        for i,v in next, cachedparts do 
            v.CanCollide = true
        end
        cachedparts = {}


        Phase.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Phase.TextColor3 = Color3.fromRGB(0, 0, 0)  

        PhaseCheatOnSave = false

        --Off:Play()

        --getgenv().speedval = {["Value"] = 20}
        
    end
end)

PhaseKeyBind = Enum.KeyCode.Escape.Value

Phase.MouseButton2Down:connect(function()
    print('clicked')
    Phase.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                PhaseKeyBind = Enum.KeyCode.Escape.Value
            else 
                PhaseKeyBind = input.KeyCode.Value
            end
            selecting = false
            Phase.Text = "Phase"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == PhaseKeyBind then
        for i,v in pairs(getconnections(Phase.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

getgenv().speedvalforspeed = {["Value"] = 50} --THIS IS 50

Speed.Name = "Speed"
Speed.Parent = Movement
Speed.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Speed.BorderColor3 = Color3.fromRGB(0, 0, 0)
Speed.BorderSizePixel = 0
Speed.Position = UDim2.new(0, 0, 0.213058412, 0)
Speed.Size = UDim2.new(0, 204, 0, 31)
Speed.Font = Enum.Font.Roboto
Speed.Text = "Speed"
Speed.TextColor3 = Color3.fromRGB(0, 0, 0)
Speed.TextSize = 25.000
Speed.MouseButton1Down:connect(function()
    if not SpeedCheatOn == true then
        SpeedCheatOn = true

        Speed.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Speed.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()

        spawn(function()
            while SpeedCheatOn == true do
                if SpeedCheatOn == true and BlatantModeCheatOn == false then
                    getgenv().speedvalforspeed = {["Value"] = 35} --30
                    wait(0.7)
                elseif SpeedCheatOn == true and BlatantModeCheatOn == true then
                    getgenv().speedvalforspeed = {["Value"] = 250} --200
                    wait()
                end
                if SpeedCheatOn == true and BlatantModeCheatOn == false then
                    getgenv().speedvalforspeed = {["Value"] = 90} --70
                    wait(0.2) --0.4
                elseif SpeedCheatOn == true and BlatantModeCheatOn == true then
                    getgenv().speedvalforspeed = {["Value"] = 250} --200
                    wait()
                end
            end
        end)

    elseif SpeedCheatOn == true then
        SpeedCheatOn = false

        Speed.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Speed.TextColor3 = Color3.fromRGB(0, 0, 0) 

        --Off:Play() 
        
    end
end)

coroutine.wrap(function() 
    repeat wait() until game:IsLoaded()
    local UIS = game:GetService("UserInputService")
    local TS = game:GetService("TweenService")
    local WORKSPACE = game:GetService("Workspace")
    local PLAYERS = game:GetService("Players")
    local lplr = PLAYERS.LocalPlayer
    local mouse = lplr:GetMouse()
    local cam = WORKSPACE.CurrentCamera
    local requestfunc = syn and syn.request or http and http.request or http_request or fluxus and fluxus.request or getgenv().request or request
    local queueteleport = syn and syn.queue_on_teleport or queue_on_teleport or fluxus and fluxus.queue_on_teleport


    stopSpeed = false

    local speedsettings = {
        factor = 5.37,  
        velocitydivfactor = 2.9,
        wsvalue = 22.5
    }

    BindToStepped("Speed", function(time, dt)
        if FLYINPUTVALUE == false then
            if SpeedCheatOn == true then
                if isAlive() and not stopSpeed or AntiCheatDisablerCheatOn == true then

                    spawn(function()
                    wait(0.1)
                        lplr.Character.Humanoid.WalkSpeed = speedsettings.wsvalue
                        local velo = lplr.Character.Humanoid.MoveDirection * (getgenv().speedvalforspeed["Value"]*((isnetworkowner and isnetworkowner(lplr.Character.HumanoidRootPart)) and speedsettings.factor or 0)) * dt
                        velo = Vector3.new(velo.x / 45, 0, velo.z / 45) -- originally 11 and 11 but before this bumber it was 45 45
                        lplr.Character:TranslateBy(velo)
                    end)
                    Heartbeat:wait()
                    
                    pcall(function()
                        local velo2 = (lplr.Character.Humanoid.MoveDirection * getgenv().speedvalforspeed["Value"]) / speedsettings.velocitydivfactor
                        lplr.Character.HumanoidRootPart.Velocity = Vector3.new(velo2.X, lplr.Character.HumanoidRootPart.Velocity.Y, velo2.Z)
                    end)

                end
            end
                
        elseif FLYINPUTVALUE == true then end
    end)
end)()

SpeedKeyBind = Enum.KeyCode.Escape.Value

Speed.MouseButton2Down:connect(function()
    print('clicked')
    Speed.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                SpeedKeyBind = Enum.KeyCode.Escape.Value
            else 
                SpeedKeyBind = input.KeyCode.Value
            end
            selecting = false
            Speed.Text = "Speed"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == SpeedKeyBind then
        for i,v in pairs(getconnections(Speed.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

Bhop.Name = "Bhop"
Bhop.Parent = Movement
Bhop.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Bhop.BorderColor3 = Color3.fromRGB(0, 0, 0)
Bhop.BorderSizePixel = 0
Bhop.Position = UDim2.new(0, 0, 0.319587618, 0)
Bhop.Size = UDim2.new(0, 204, 0, 31)
Bhop.Font = Enum.Font.Roboto
Bhop.Text = "Bhop"
Bhop.TextColor3 = Color3.fromRGB(0, 0, 0)
Bhop.TextSize = 25.000
Bhop.MouseButton1Down:connect(function()
    if not BhopCheatOn == true then
        BhopCheatOn = true

        Bhop.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Bhop.TextColor3 = Color3.fromRGB(255, 255, 255) 

        --On:Play()

        coroutine.wrap(function()
            while BhopCheatOn == true do
                if humanoid.MoveDirection.X > 0 or humanoid.MoveDirection.X < 0 or humanoid.MoveDirection.Z > 0 or humanoid.MoveDirection.Z < 0 or humanoid.MoveDirection.Y > 0 or humanoid.MoveDirection.Y < 0 then -- as you can see it's scuff since I didn't know what I was doing much back then
                    game.Players.LocalPlayer.Character.Humanoid.Jump = true
                    if FlyCheatOn == false and LongJumpCheatOn == false and HighJumpCheatOn == false and FastFallCheatOn == false then
                        workspace.Gravity = 70
                    end
                end
                wait()		
            end
        end)()



    elseif BhopCheatOn == true then
        BhopCheatOn = false

        workspace.Gravity = 196.2

        Bhop.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Bhop.TextColor3 = Color3.fromRGB(0, 0, 0)  

        BhopCheatOnSave = false

        --Off:Play()

        getgenv().speedval = {["Value"] = 20}
        
    end
end)

BhopKeyBind = Enum.KeyCode.Escape.Value

Bhop.MouseButton2Down:connect(function()
    print('clicked')
    Bhop.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                BhopKeyBind = Enum.KeyCode.Escape.Value
            else 
                BhopKeyBind = input.KeyCode.Value
            end
            selecting = false
            Bhop.Text = "Bhop"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == BhopKeyBind then
        for i,v in pairs(getconnections(Bhop.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

HighJump.Name = "HighJump"
HighJump.Parent = Movement
HighJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
HighJump.BorderColor3 = Color3.fromRGB(0, 0, 0)
HighJump.BorderSizePixel = 0
HighJump.Position = UDim2.new(0, 0, 0.639175296, 0)
HighJump.Size = UDim2.new(0, 204, 0, 31)
HighJump.Font = Enum.Font.Roboto
HighJump.Text = "HighJump"
HighJump.TextColor3 = Color3.fromRGB(0, 0, 0)
HighJump.TextSize = 25.000
HighJump.MouseButton1Down:connect(function()
    HighJump.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
    HighJump.TextColor3 = Color3.fromRGB(255, 255, 255) 

    HighJumpCheatOn = true

    --On:Play()

    coroutine.wrap(function()
        if HighJumpCheatOn == true and BlatantModeCheatOn == false then
            for i = 10, 30 do --5, 25
                task.wait(0.04)
                lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
            end

            elseif HighJumpCheatOn == true and BlatantModeCheatOn == true then
                for i = 25, 50 do
                    task.wait(0.04)
                    lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
                end
        end
    end)()

    wait(2)

    HighJumpCheatOn = false

    HighJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
    HighJump.TextColor3 = Color3.fromRGB(0, 0, 0)  
end)


HighJumpKeyBind = Enum.KeyCode.Escape.Value

HighJump.MouseButton2Down:connect(function()
    print('clicked')
    HighJump.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                HighJumpKeyBind = Enum.KeyCode.Escape.Value
            else 
                HighJumpKeyBind = input.KeyCode.Value
            end
            selecting = false
            HighJump.Text = "HighJump"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == HighJumpKeyBind then
        HighJump.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        HighJump.TextColor3 = Color3.fromRGB(255, 255, 255) 

        HighJumpCheatOn = true

        --On:Play()

        coroutine.wrap(function()
            if HighJumpCheatOn == true and BlatantModeCheatOn == false then
                for i = 5, 25 do
                    task.wait(0.04)
                    lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
                end

                elseif HighJumpCheatOn == true and BlatantModeCheatOn == true then
                    for i = 25, 50 do
                        task.wait(0.04)
                        lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
                    end
            end
        end)()

        wait(2)

        HighJumpCheatOn = false
        
        HighJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        HighJump.TextColor3 = Color3.fromRGB(0, 0, 0)  
    end
end)

FLYINPUTVALUE = false
goingup = true
dinotimeron = false
dinodisablerdone = true

Fly.Name = "Fly"
Fly.Parent = Movement
Fly.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Fly.BorderColor3 = Color3.fromRGB(0, 0, 0)
Fly.BorderSizePixel = 0
Fly.Position = UDim2.new(0, 0, 0.426116824, 0)
Fly.Size = UDim2.new(0, 204, 0, 31)
Fly.Font = Enum.Font.Roboto
Fly.Text = "Fly"
Fly.TextColor3 = Color3.fromRGB(0, 0, 0)
Fly.TextSize = 25.000
Fly.MouseButton1Down:connect(function()
    if not FlyCheatOn == true then
        FlyCheatOn = true

        Fly.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Fly.TextColor3 = Color3.fromRGB(255, 255, 255)

        --On:Play()

        coroutine.wrap(function()
            if dinotimeron == false and DinoFlyCheatOn == true then
                pcall(function()
                    game:GetService("ReplicatedStorage"):FindFirstChild("events-@easy-games/game-core:shared/game-core-networking@getEvents.Events").useAbility:FireServer("dino_charge")
                    dinoconnection = bedwars["ClientHandler"]:Get(bedwars["DinoRemote"]):Connect(function()
                    end)
                end)

                spawn(function()
                    wait(10)
                    dinodisablerdone = true
                end)

                dinotimeron = true
                dinodisablerdone = false

                local screenguifortimer = Instance.new("ScreenGui")
                screenguifortimer.Name = "extragui"
                screenguifortimer.Parent = game.Players.LocalPlayer.PlayerGui
                local disablertimer = Instance.new("TextBox")
                disablertimer.Size = UDim2.new(0.3, 0, 0, 10)
                disablertimer.TextSize = 30
                disablertimer.TextColor3 = Color3.fromRGB(255,255,255)
                disablertimer.TextStrokeTransparency = 0
                disablertimer.TextStrokeColor3 = Color3.fromRGB(0,0,0)
                disablertimer.AnchorPoint = Vector2.new(0.5, 0.5)
                disablertimer.BorderSizePixel = 0
                disablertimer.BackgroundTransparency = 1
                disablertimer.BackgroundColor3 = Color3.new()
                disablertimer.Position = UDim2.new(0.5, 0, 0.75, 0)
                disablertimer.Name = "disablertimer"
                disablertimer.Parent = screenguifortimer

                getgenv().speedvalforfly = {["Value"] = 115}

                spawn(function()
                    for i = 60, 0, -1 do
                        disablertimer.Text = "Boost again in ".. i ..""
                        wait(1)
                    end
                end)

                wait(60)

                disablertimer:Destroy()
                screenguifortimer:Destroy()
                dinotimeron = false
                dinodisablerdone = true

                elseif BlatantModeCheatOn == true then
            end
        end)()

        spawn(function()
            while FlyCheatOn == true do
                if FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == false then
                    getgenv().speedvalforfly = {["Value"] = 50} --55
                    wait(0.6) --0.5
                elseif FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == true then
                    getgenv().speedvalforfly = {["Value"] = 350} --300
                    wait()
                end
                if FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == false then
                    getgenv().speedvalforfly = {["Value"] = 70} --85
                    wait(0.3) --0.4
                elseif FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == true then
                    getgenv().speedvalforfly = {["Value"] = 350} --300
                    wait()
                end
            Heartbeat:wait()
            end
        end)

        flyfreezecazspeedstop = false

        if FlyCheatOn == true and SpeedCheatOn == true and BlatantModeCheatOn == false then
            spawn(function()
                game.StarterGui:SetCore("SendNotification", {
                    Title = "Fly Warning";
                    Text = "Using speed right before fly requires optimization. therefore the fly could go further.";
                    Icon = "";
                    Duration = 4;
                })

                local character = game.Players.LocalPlayer.Character
                character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 0, 1.5))

                xfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.X  
                yfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.Y  
                zfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.Z  

                while true do
                    lplr.Character.HumanoidRootPart.CFrame = CFrame.new(xfreezeifspeedwa, yfreezeifspeedwa, zfreezeifspeedwa)

                    if flyfreezecazspeedstop == true then
                        break
                    end
                    Heartbeat:wait()
                end
            end)
            wait(0.4) --0.2
            flyfreezecazspeedstop = true

            elseif FlyCheatOn == true and SpeedCheatOn == true and BlatantModeCheatOn == true then
                
        end 

        FLYINPUTVALUE = true
                --On:Play()
                Hit:Play()
                --getgenv().speedval = {["Value"] = 55} -- this was 55


                workspace.Gravity = -30 --I think about -30 is good


                spawn(function()
                    while FLYINPUTVALUE == true do
                        WORKSPACE = game.Workspace
                        lplr = game.Players.LocalPlayer

                        local params = RaycastParams.new()
                        params.FilterDescendantsInstances = {game:GetService("CollectionService"):GetTagged("block")}
                        params.FilterType = Enum.RaycastFilterType.Whitelist
                        local ray = WORKSPACE:Raycast(lplr.Character.HumanoidRootPart.Position, Vector3.new(0, -10, 0), params)
                        Heartbeat:wait()
                    end
                end)

                Y = 0

                if AntiCheatDisablerCheatOn and BlatantModeCheatOn then
                    if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then 
                        Y = 300
                        else
                        Y = 0
                    end
                    if UIS:IsKeyDown(Enum.KeyCode.Space) then 
                        Y = 300
                        else
                        Y = 0
                    end
                else 
                    if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then 
                        Y = 60
                        else
                        Y = 0
                    end
                    if UIS:IsKeyDown(Enum.KeyCode.Space) then 
                        Y = 60
                        else
                        Y = 0
                    end
                end         

                spawn(function()
                    while FLYINPUTVALUE == true do
                        character.HumanoidRootPart.Velocity = vec3(character.HumanoidRootPart.Velocity.X, Y, character.HumanoidRootPart.Velocity.Z)

                        Heartbeat:Wait()
                    end
                end)

    elseif FlyCheatOn == true then
        FlyCheatOn = false

    FLYINPUTVALUE = false


        Fly.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        Fly.TextColor3 = Color3.fromRGB(0, 0, 0)

                workspace.Gravity = 196.2

                --Off:Play() 

        getgenv().speedval = {["Value"] = 15}
        
    end
end)

local key = game:GetService("UserInputService")

repeat wait() until game:IsLoaded()
getgenv().Future = shared.Future
getgenv().UIS = game:GetService("UserInputService")
getgenv().TS = game:GetService("TweenService")
getgenv().WORKSPACE = game:GetService("Workspace")
getgenv().PLAYERS = game:GetService("Players")
getgenv().lplr = PLAYERS.LocalPlayer
getgenv().mouse = lplr:GetMouse()
getgenv().cam = WORKSPACE.CurrentCamera
getgenv().requestfunc = syn and syn.request or http and http.request or http_request or fluxus and fluxus.request or getgenv().request or request
getgenv().queueteleport = syn and syn.queue_on_teleport or queue_on_teleport or fluxus and fluxus.queue_on_teleport

local spawn = function(func) 
    return coroutine.wrap(func)()
end

local SteppedTable = {}

local function BindToStepped(name, func)
    if SteppedTable[name] == nil then
        SteppedTable[name] = game:GetService("RunService").Stepped:connect(func)
    end
end

local function isAlive(plr)
    local plr = plr or lplr
    if plr and plr.Character and ((plr.Character:FindFirstChild("Humanoid")) and (plr.Character:FindFirstChild("Humanoid") and plr.Character:FindFirstChild("Humanoid").Health > 0) and (plr.Character:FindFirstChild("HumanoidRootPart")) and (plr.Character:FindFirstChild("Head"))) then
        return true
    end
end

local isnetworkowner = isnetworkowner or function() return true end


stopSpeed = false

local speedsettings = {
    factor = 5.37,  
    velocitydivfactor = 2.9,
    wsvalue = 22.5
}



BindToStepped("Speed", function(time, dt)
    if FLYINPUTVALUE == true then
        if isAlive() and not stopSpeed or AntiCheatDisablerCheatOn == true then

            spawn(function()
                wait(0.1)
                lplr.Character.Humanoid.WalkSpeed = speedsettings.wsvalue
                local velo = lplr.Character.Humanoid.MoveDirection * (getgenv().speedvalforfly["Value"]*((isnetworkowner and isnetworkowner(lplr.Character.HumanoidRootPart)) and speedsettings.factor or 0)) * dt
                velo = Vector3.new(velo.x / 45, 0, velo.z / 45) -- originally 11 and 11 but before 50 50
                lplr.Character:TranslateBy(velo)
            end)
            Heartbeat:wait()
            
            pcall(function()
                local velo2 = (lplr.Character.Humanoid.MoveDirection * getgenv().speedvalforfly["Value"]) / speedsettings.velocitydivfactor
                lplr.Character.HumanoidRootPart.Velocity = Vector3.new(velo2.X, lplr.Character.HumanoidRootPart.Velocity.Y, velo2.Z)
            end)
            
        end
    end
end)



spawn(function()
    while true do
        if FlyCheatOn == false then
            FlyCheatOnSave = false
            
            elseif FlyCheatOn == true then
              FlyCheatOnSave = true  
        end
        wait()
    end
end)


FlyKeyBind = Enum.KeyCode.Escape.Value

Fly.MouseButton2Down:connect(function()
    print('clicked')
    Fly.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                FlyKeyBind = Enum.KeyCode.Escape.Value
            else 
                FlyKeyBind = input.KeyCode.Value
            end
            selecting = false
            Fly.Text = "Fly"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == FlyKeyBind then
        if not FlyCheatOn == true then
            FlyCheatOn = true

            Fly.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
            Fly.TextColor3 = Color3.fromRGB(255, 255, 255)

            --On:Play()

            coroutine.wrap(function()
                if dinotimeron == false and DinoFlyCheatOn == true then
                    pcall(function()
                        game:GetService("ReplicatedStorage"):FindFirstChild("events-@easy-games/game-core:shared/game-core-networking@getEvents.Events").useAbility:FireServer("dino_charge")
                        dinoconnection = bedwars["ClientHandler"]:Get(bedwars["DinoRemote"]):Connect(function()
                        end)
                    end)

                    spawn(function()
                        wait(10)
                        dinodisablerdone = true
                    end)

                    dinotimeron = true
                    dinodisablerdone = false

                    local screenguifortimer = Instance.new("ScreenGui")
                    screenguifortimer.Name = "extragui"
                    screenguifortimer.Parent = game.Players.LocalPlayer.PlayerGui
                    local disablertimer = Instance.new("TextBox")
                    disablertimer.Size = UDim2.new(0.3, 0, 0, 10)
                    disablertimer.TextSize = 30
                    disablertimer.TextColor3 = Color3.fromRGB(255,255,255)
                    disablertimer.TextStrokeTransparency = 0
                    disablertimer.TextStrokeColor3 = Color3.fromRGB(0,0,0)
                    disablertimer.AnchorPoint = Vector2.new(0.5, 0.5)
                    disablertimer.BorderSizePixel = 0
                    disablertimer.BackgroundTransparency = 1
                    disablertimer.BackgroundColor3 = Color3.new()
                    disablertimer.Position = UDim2.new(0.5, 0, 0.75, 0)
                    disablertimer.Name = "disablertimer"
                    disablertimer.Parent = screenguifortimer

                    getgenv().speedvalforfly = {["Value"] = 115}

                    spawn(function()
                        for i = 60, 0, -1 do
                            disablertimer.Text = "Boost again in ".. i ..""
                            wait(1)
                        end
                    end)

                    wait(60)

                    disablertimer:Destroy()
                    screenguifortimer:Destroy()
                    dinotimeron = false
                    dinodisablerdone = true

                    elseif BlatantModeCheatOn == true then
                end
            end)()

            spawn(function()
                while FlyCheatOn == true do
                    if FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == false then
                        getgenv().speedvalforfly = {["Value"] = 50} --55
                        wait(0.6) --0.5
                    elseif FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == true then
                        getgenv().speedvalforfly = {["Value"] = 350} --300
                        wait()
                    end
                    if FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == false then
                        getgenv().speedvalforfly = {["Value"] = 70} --85
                        wait(0.3) --0.4
                    elseif FlyCheatOn == true and dinodisablerdone == true and BlatantModeCheatOn == true then
                        getgenv().speedvalforfly = {["Value"] = 350} --300
                        wait()
                    end
                Heartbeat:wait()
                end
            end)

            flyfreezecazspeedstop = false

            if FlyCheatOn == true and SpeedCheatOn == true and BlatantModeCheatOn == false then
                spawn(function()
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "Fly Warning";
                        Text = "Using speed right before fly requires optimization. therefore the fly could go further.";
                        Icon = "";
                        Duration = 4;
                    })

                    local character = game.Players.LocalPlayer.Character
                    character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 0, 1.5))

                    xfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.X  
                    yfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.Y  
                    zfreezeifspeedwa = lplr.Character.HumanoidRootPart.CFrame.Z  

                    while true do
                        lplr.Character.HumanoidRootPart.CFrame = CFrame.new(xfreezeifspeedwa, yfreezeifspeedwa, zfreezeifspeedwa)

                        if flyfreezecazspeedstop == true then
                            break
                        end
                        Heartbeat:wait()
                    end
                end)
                wait(0.4) --0.2
                flyfreezecazspeedstop = true

                elseif FlyCheatOn == true and SpeedCheatOn == true and BlatantModeCheatOn == true then
                    
            end  

            FLYINPUTVALUE = true
                    --On:Play()
                    Hit:Play()
                    --getgenv().speedval = {["Value"] = 55} -- this was 65


                    workspace.Gravity = -30 --I think about -30 is good


                    spawn(function()
                        while FLYINPUTVALUE == true do
                            WORKSPACE = game.Workspace
                            lplr = game.Players.LocalPlayer

                            local params = RaycastParams.new()
                            params.FilterDescendantsInstances = {game:GetService("CollectionService"):GetTagged("block")}
                            params.FilterType = Enum.RaycastFilterType.Whitelist
                            local ray = WORKSPACE:Raycast(lplr.Character.HumanoidRootPart.Position, Vector3.new(0, -10, 0), params)
                            Heartbeat:wait()
                        end
                    end)

                Y = game.Workspace[game.Players.LocalPlayer.Name].HumanoidRootPart.CFrame.Y

                    spawn(function()
                        while FLYINPUTVALUE == true do
                            X = game.Workspace[game.Players.LocalPlayer.Name].HumanoidRootPart.CFrame.X
                            Z = game.Workspace[game.Players.LocalPlayer.Name].HumanoidRootPart.CFrame.Z

                            if AntiCheatDisablerCheatOn and BlatantModeCheatOn then
                                if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then 
                                    Y = Y-0.6
                                end
                                if UIS:IsKeyDown(Enum.KeyCode.Space) then 
                                    Y = Y+0.6
                                end

                            else 
                                if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then 
                                    Y = Y-0.3
                                end
                                if UIS:IsKeyDown(Enum.KeyCode.Space) then 
                                    Y = Y+0.3
                                end
                            end

                            game.Workspace[game.Players.LocalPlayer.Name].HumanoidRootPart.CFrame = CFrame.new(X, Y, Z)
                            Heartbeat:Wait()
                        end
                    end) 

        elseif FlyCheatOn == true then
            FlyCheatOn = false

        FLYINPUTVALUE = false


            Fly.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            Fly.TextColor3 = Color3.fromRGB(0, 0, 0)

            workspace.Gravity = 196.2

        end
    end
end)


LongJump.Name = "LongJump"
LongJump.Parent = Movement
LongJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
LongJump.BorderColor3 = Color3.fromRGB(0, 0, 0)
LongJump.BorderSizePixel = 0
LongJump.Position = UDim2.new(0, 0, 0.53264606, 0)
LongJump.Size = UDim2.new(0, 204, 0, 31)
LongJump.Font = Enum.Font.Roboto
LongJump.Text = "LongJump"
LongJump.TextColor3 = Color3.fromRGB(0, 0, 0)
LongJump.TextSize = 25.000
LongJump.MouseButton1Down:connect(function()
    if not LongJumpCheatOn == true then
        LongJumpCheatOn = true

        LongJump.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        LongJump.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()

        longjumpfreezecazspeedstop = false

            if SpeedCheatOn == true and LongJumpCheatOn == true and BlatantModeCheatOn == false then
                spawn(function()
                    game.StarterGui:SetCore("SendNotification", {
                        Title = "LongJump Warning";
                        Text = "Using speed right before longjump requires optimization. therefore the fly could go further.";
                        Icon = "";
                        Duration = 4;
                    })

                    local character = game.Players.LocalPlayer.Character
                    character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 0, 1))

                    xfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.X  
                    yfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.Y  
                    zfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.Z  

                    while true do
                        lplr.Character.HumanoidRootPart.CFrame = CFrame.new(xfreezeifspeedlongjump, yfreezeifspeedlongjump, zfreezeifspeedlongjump)

                        if longjumpfreezecazspeedstop == true then
                            break
                        end
                        Heartbeat:wait()
                    end
                end)
                wait(0.2) --0.06
                longjumpfreezecazspeedstop = true
                elseif SpeedCheatOn == true and LongJumpCheatOn == true and BlatantModeCheatOn == true then

            end 

        Hit:Play()
        workspace.Gravity = 30 -- 20
        lplr.Character.HumanoidRootPart.Velocity = Vector3.new(0, 20, 0)
        --game.Players.LocalPlayer.Character.Humanoid.Jump = true

        wait(2.5)
                
        workspace.Gravity = 196.2

        LongJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        LongJump.TextColor3 = Color3.fromRGB(0, 0, 0)
        LongJumpCheatOn = false

        --Off:Play()  

    elseif LongJumpCheatOn == true then
        LongJumpCheatOn = false

        workspace.Gravity = 196.2
        --getgenv().speedval = {["Value"] = 42}

        LongJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        LongJump.TextColor3 = Color3.fromRGB(0, 0, 0)

        --Off:Play()  
        
    end
end)

LongJumpKeyBind = Enum.KeyCode.Escape.Value

LongJump.MouseButton2Down:connect(function()
    print('clicked')
    LongJump.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                LongJumpKeyBind = Enum.KeyCode.Escape.Value
            else 
                LongJumpKeyBind = input.KeyCode.Value
            end
            selecting = false
            LongJump.Text = "LongJump"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == LongJumpKeyBind then
        if not LongJumpCheatOn == true then
            LongJumpCheatOn = true

            LongJump.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
            LongJump.TextColor3 = Color3.fromRGB(255, 255, 255)  

            --On:Play()

            longjumpfreezecazspeedstop = false

                if SpeedCheatOn == true and LongJumpCheatOn == true and BlatantModeCheatOn == false then
                    spawn(function()
                        game.StarterGui:SetCore("SendNotification", {
                            Title = "LongJump Warning";
                            Text = "Using speed right before longjump requires optimization. therefore the fly could go further.";
                            Icon = "";
                            Duration = 4;
                        })

                        local character = game.Players.LocalPlayer.Character
                        character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 0, 1))

                        xfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.X  
                        yfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.Y  
                        zfreezeifspeedlongjump = lplr.Character.HumanoidRootPart.CFrame.Z  

                        while true do
                            lplr.Character.HumanoidRootPart.CFrame = CFrame.new(xfreezeifspeedlongjump, yfreezeifspeedlongjump, zfreezeifspeedlongjump)

                            if longjumpfreezecazspeedstop == true then
                                break
                            end
                            Heartbeat:wait()
                        end
                    end)
                    wait(0.2) --0.06
                    longjumpfreezecazspeedstop = true
                    elseif SpeedCheatOn == true and LongJumpCheatOn == true and BlatantModeCheatOn == true then
                        
                end 

            Hit:Play()
            workspace.Gravity = 30 -- 20
            lplr.Character.HumanoidRootPart.Velocity = Vector3.new(0, 20, 0)
            --game.Players.LocalPlayer.Character.Humanoid.Jump = true

            wait(2.5)
        
            workspace.Gravity = 196.2

            LongJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
            LongJump.TextColor3 = Color3.fromRGB(0, 0, 0)
            LongJumpCheatOn = false

            --Off:Play()  

    elseif LongJumpCheatOn == true then
        LongJumpCheatOn = false

        workspace.Gravity = 196.2

        LongJump.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        LongJump.TextColor3 = Color3.fromRGB(0, 0, 0)

        --Off:Play()  
           
        end
    end
end)

Item.Name = "Item"
Item.Parent = Sigma_1
Item.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Item.BorderColor3 = Color3.fromRGB(255, 255, 255)
Item.BorderSizePixel = 0
Item.Position = UDim2.new(0.619985402, 0, 0.0327613354, 0)
Item.Size = UDim2.new(0, 204, 0, 291)
Item.Visible = true
Item.Active = true
Item.Draggable = true

Cover4.Name = "Cover4"
Cover4.Parent = Item
Cover4.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover4.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover4.BorderSizePixel = 0
Cover4.Position = UDim2.new(0.514705896, 0, 7.11552493e-05, 0)
Cover4.Size = UDim2.new(0, 99, 0, 63)

ItemText.Name = "ItemText"
ItemText.Parent = Item
ItemText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
ItemText.BorderColor3 = Color3.fromRGB(0, 0, 0)
ItemText.BorderSizePixel = 0
ItemText.Position = UDim2.new(0, 0, -0.00105012977, 0)
ItemText.Size = UDim2.new(0, 105, 0, 63)
ItemText.Font = Enum.Font.Roboto
ItemText.Text = "Item"
ItemText.TextColor3 = Color3.fromRGB(120, 120, 120)
ItemText.TextSize = 32.000

ChestStealer.Name = "ChestStealer"
ChestStealer.Parent = Item
ChestStealer.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChestStealer.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChestStealer.BorderSizePixel = 0
ChestStealer.Position = UDim2.new(0, 0, 0.319587618, 0)
ChestStealer.Size = UDim2.new(0, 204, 0, 31)
ChestStealer.Font = Enum.Font.Roboto
ChestStealer.Text = "ChestStealer"
ChestStealer.TextColor3 = Color3.fromRGB(0, 0, 0)
ChestStealer.TextSize = 25.000
ChestStealer.MouseButton1Down:connect(function()
    if not ChestStealerCheatOn == true then
        ChestStealerCheatOn = true

        ChestStealer.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChestStealer.TextColor3 = Color3.fromRGB(255, 255, 255) 

        ChestStealerCheatOnSave = true

        --On:Play()

        while ChestStealerCheatOn == true do
            local ChestStealerDistance = {["Value"] = 18}
            local ChestStealDelay = wait()
                    if isAlive() and ChestStealerCheatOn == true then
                        local rootpart = lplr.Character and lplr.Character:FindFirstChild("HumanoidRootPart")
                        for i,v in pairs(game:GetService("CollectionService"):GetTagged("chest")) do
                            if rootpart and (rootpart.Position - v.Position).magnitude <= ChestStealerDistance["Value"] and v:FindFirstChild("ChestFolderValue") then
                                local chest = v.ChestFolderValue.Value
                                local chestitems = chest and chest:GetChildren() or {}
                                if #chestitems > 0 then
                                    bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("SetObservedChest"):SendToServer(chest)
                                    for i3,v3 in pairs(chestitems) do
                                        if v3:IsA("Accessory") then
                                            spawn(function()
                                                bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("ChestGetItem"):CallServer(v.ChestFolderValue.Value, v3)
                                            end)
                                        end
                                    end
                                    bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("SetObservedChest"):SendToServer(nil)
                                end
                            end
                        end
                    end
                    Heartbeat:wait()
                end




    elseif ChestStealerCheatOn == true then
        ChestStealerCheatOn = false

        ChestStealer.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChestStealer.TextColor3 = Color3.fromRGB(0, 0, 0) 

        ChestStealerCheatOnSave = false

        --Off:Play()
        
    end
end)

ChestStealerKeyBind = Enum.KeyCode.Escape.Value

ChestStealer.MouseButton2Down:connect(function()
    print('clicked')
    ChestStealer.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChestStealerKeyBind = Enum.KeyCode.Escape.Value
            else 
                ChestStealerKeyBind = input.KeyCode.Value
            end
            selecting = false
            ChestStealer.Text = "ChestStealer"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChestStealerKeyBind then
        for i,v in pairs(getconnections(ChestStealer.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

BigSword.Name = "BigSword"
BigSword.Parent = Item
BigSword.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
BigSword.BorderColor3 = Color3.fromRGB(0, 0, 0)
BigSword.BorderSizePixel = 0
BigSword.Position = UDim2.new(0, 0, 0.213058412, 0)
BigSword.Size = UDim2.new(0, 204, 0, 31)
BigSword.Font = Enum.Font.Roboto
BigSword.Text = "BigSword"
BigSword.TextColor3 = Color3.fromRGB(0, 0, 0)
BigSword.TextSize = 25.000
BigSword.MouseButton1Down:connect(function()
    if not BigSwordCheatOn == true then
        BigSwordCheatOn = true

        lplr = game.Players.LocalPlayer
        --newsize = Vector3.new(17.6064, 53.9858, 5.80578)  

        BigSword.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        BigSword.TextColor3 = Color3.fromRGB(255, 255, 255) 

        BigSwordCheatOnSave = true

        --On:Play()

        while BigSwordCheatOn == true do
            spawn(function()
                lplr.Character:WaitForChild('wood_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)  
            end)

            spawn(function()
                lplr.Character:WaitForChild('stone_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
            end)

            spawn(function()
                lplr.Character:WaitForChild('iron_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
            end)

            spawn(function()
                lplr.Character:WaitForChild('diamond_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
            end)

            spawn(function()
                lplr.Character:WaitForChild('emerald_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
            end)
            wait()
        end


    elseif BigSwordCheatOn == true then
        BigSwordCheatOn = false

        BigSword.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        BigSword.TextColor3 = Color3.fromRGB(0, 0, 0)

        BigSwordCheatOnSave = false

        --Off:Play()

        spawn(function()
            lplr.Character:WaitForChild('wood_sword').Handle.Size = Vector3.new(1.76064, 5.39858, 0.580578)  
        end)

        spawn(function()
            lplr.Character:WaitForChild('stone_sword').Handle.Size = Vector3.new(1.76064, 5.39858, 0.580578)
        end)

        spawn(function()
            lplr.Character:WaitForChild('iron_sword').Handle.Size = Vector3.new(1.76064, 5.39858, 0.580578)
        end)

        spawn(function()
            lplr.Character:WaitForChild('diamond_sword').Handle.Size = Vector3.new(1.76064, 5.39858, 0.580578)
        end)

        spawn(function()
            lplr.Character:WaitForChild('emerald_sword').Handle.Size = Vector3.new(1.76064, 5.39858, 0.580578)
        end)

        
    end
end)

BigSwordKeyBind = Enum.KeyCode.Escape.Value

BigSword.MouseButton2Down:connect(function()
    print('clicked')
    BigSword.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                BigSwordKeyBind = Enum.KeyCode.Escape.Value
            else 
                BigSwordKeyBind = input.KeyCode.Value
            end
            selecting = false
            BigSword.Text = "BigSword"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == BigSwordKeyBind then
        for i,v in pairs(getconnections(BigSword.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

ScaffoldToggle = false

Scaffold.Name = "Scaffold"
Scaffold.Parent = Item
Scaffold.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Scaffold.BorderColor3 = Color3.fromRGB(0, 0, 0)
Scaffold.BorderSizePixel = 0
Scaffold.Position = UDim2.new(0, 0, 0.426116824, 0)
Scaffold.Size = UDim2.new(0, 204, 0, 31)
Scaffold.Font = Enum.Font.Roboto
Scaffold.Text = "Scaffold"
Scaffold.TextColor3 = Color3.fromRGB(0, 0, 0)
Scaffold.TextSize = 25.000
Scaffold.MouseButton1Down:connect(function()
    if ScaffoldCheatOn == false then
        ScaffoldCheatOn = true

        Scaffold.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Scaffold.TextColor3 = Color3.fromRGB(255, 255, 255) 

        ScaffoldCheatOnSave = true

        --On:Play()

        local InventoryUtil = require(game:GetService("ReplicatedStorage").TS.inventory["inventory-util"]).InventoryUtil

        local bedwars = {
            ["BlockController"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["block-engine"].out).BlockEngine,
            ["BlockController2"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["block-engine"].out.client.placement["block-placer"]).BlockPlacer,
            ["BlockEngine"] = require(lplr.PlayerScripts.TS.lib["block-engine"]["client-block-engine"]).ClientBlockEngine,
            ["getInventory"] = function(plr)
                local plr = plr or lplr
                local suc, result = pcall(function() return InventoryUtil.getInventory(plr) end)
                return (suc and result or {
                    ["items"] = {},
                    ["armor"] = {},
                    ["hand"] = nil
                })
            end,
        }


        local function getblockitem() 
            for i5, v5 in pairs(bedwars.getInventory(lplr).items) do
                if v5.itemType:match("wool") or v5.itemType:match("grass") or v5.itemType:match("stone_brick") or v5.itemType:match("wood_plank") or v5.itemType:match("stone") or v5.itemType:match("bedrock") then
                    return v5.itemType, v5.amount
                end
            end	
            return nil
        end

        local function get3Vector(p) 
            local x,y,z = p.X, p.Y,p.Z 
            x = math.floor((x) + 0.5)
            y = math.floor((y) + 0.5)
            z = math.floor((z) + 0.5)
            return Vector3.new(x,y,z)
        end

        local function isPointInMapOccupied(p)
            local region = Region3.new(p - Vector3.new(1, 1, 1), p + Vector3.new(1, 1, 1))
            local x = workspace:FindPartsInRegion3WithWhiteList(region, game:GetService("CollectionService"):GetTagged("block"))
            return (#x == 0)
        end

        local function getwool()
            for i5, v5 in pairs(bedwars["getInventory"](lplr)["items"]) do
                if v5.itemType:match("wool") then
                    return v5.itemType, v5.amount
                end
            end	
            return nil
        end

        local function getItem(itemName)
            for i5, v5 in pairs(bedwars["getInventory"](lplr)["items"]) do
                if v5.itemType == itemName then
                    return v5, i5
                end
            end
            return nil
        end


        local blocktable = bedwars["BlockController2"].new(bedwars["BlockEngine"], getwool())
        bedwars["placeBlock"] = function(newpos, customblock)
            local placeblocktype = (customblock or getwool())
            blocktable.blockType = placeblocktype
            if bedwars["BlockController"]:isAllowedPlacement(lplr, placeblocktype, Vector3.new(newpos.X/3, newpos.Y/3, newpos.Z/3)) and getItem(placeblocktype) then
                spawn(function()
                    return blocktable:placeBlock(Vector3.new(newpos.X/3, newpos.Y/3, newpos.Z/3))
                end)
            end
        end
        
        BindToStepped("Scaffold", function()
            if ScaffoldCheatOn == true then
                if isAlive() and lplr.Character:FindFirstChild("Humanoid") ~= nil then
                    local block = getblockitem()
                    --printtable(block)
                    local newpos = lplr.Character.HumanoidRootPart.Position
                    newpos = get3Vector( Vector3.new(newpos.X, lplr.Character.HumanoidRootPart.Position.Y - 4, newpos.Z) )
                    local movedir = lplr.Character:FindFirstChild("Humanoid").MoveDirection
                    if movedir.X==0 and movedir.Z==0 and lplr.Character:FindFirstChild("Humanoid").Jump==true  then 
                        local velo = lplr.Character.HumanoidRootPart.Velocity
                        lplr.Character.HumanoidRootPart.Velocity = Vector3.new(0, 25, 0) --the y is 25 normally
                    end
                    if not isPointInMapOccupied(newpos) then
                        spawn(function()
                            bedwars["placeBlock"](newpos, block)
                        end)
                    end

                    local expandpos = lplr.Character.HumanoidRootPart.Position + ((lplr.Character.Humanoid.MoveDirection.Unit))
                    expandpos = get3Vector( Vector3.new(expandpos.X, lplr.Character.HumanoidRootPart.Position.Y-4, expandpos.Z) )
                    if not isPointInMapOccupied(expandpos) then
                        spawn(function()
                            bedwars["placeBlock"](expandpos)
                        end)
                    end

                    local expandpos2 = lplr.Character.HumanoidRootPart.Position + ((lplr.Character.Humanoid.MoveDirection.Unit*2))
                    expandpos2 = get3Vector( Vector3.new(expandpos2.X, lplr.Character.HumanoidRootPart.Position.Y-4, expandpos2.Z) )
                    if not isPointInMapOccupied(expandpos2) then
                        spawn(function()
                            bedwars["placeBlock"](expandpos2)
                        end)
                    end
                end
            end
        end)
 

    elseif ScaffoldCheatOn == true then
        ScaffoldCheatOn = false

        Scaffold.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Scaffold.TextColor3 = Color3.fromRGB(0, 0, 0) 

        --Off:Play()
        if UnbindFromStepped then
            UnbindFromStepped("Scaffold")
        else
            print("BALLL:LOPLOLOSD")
        end

        

    end
end)

ScaffoldKeyBind = Enum.KeyCode.Escape.Value

Scaffold.MouseButton2Down:connect(function()
    print('clicked')
    Scaffold.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input.KeyCode)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ScaffoldKeyBind = Enum.KeyCode.Escape.Value
            else 
                ScaffoldKeyBind = input.KeyCode.Value
            end
            selecting = false
            Scaffold.Text = "Scaffold"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ScaffoldKeyBind then
        for i,v in pairs(getconnections(Scaffold.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

Gui.Name = "Gui"
Gui.Parent = Sigma_1
Gui.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Gui.BorderColor3 = Color3.fromRGB(255, 255, 255)
Gui.BorderSizePixel = 0
Gui.Position = UDim2.new(0.072209999, 0, 0.513260424, 0)
Gui.Size = UDim2.new(0, 204, 0, 291)
Gui.Visible = true
Gui.Active = true
Gui.Draggable = true

Cover5.Name = "Cover5"
Cover5.Parent = Gui
Cover5.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover5.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover5.BorderSizePixel = 0
Cover5.Position = UDim2.new(0.426470578, 0, 0, 0)
Cover5.Size = UDim2.new(0, 117, 0, 62)

GuiText.Name = "GuiText"
GuiText.Parent = Gui
GuiText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
GuiText.BorderColor3 = Color3.fromRGB(0, 0, 0)
GuiText.BorderSizePixel = 0
GuiText.Position = UDim2.new(0, 0, -0.00105018227, 0)
GuiText.Size = UDim2.new(0, 87, 0, 63)
GuiText.Font = Enum.Font.Roboto
GuiText.Text = "Gui"
GuiText.TextColor3 = Color3.fromRGB(120, 120, 120)
GuiText.TextSize = 32.000

HUD.Name = "HUD"
HUD.Parent = Gui
HUD.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
HUD.BorderColor3 = Color3.fromRGB(0, 0, 0)
HUD.BorderSizePixel = 0
HUD.Position = UDim2.new(0, 0, 0.213058412, 0)
HUD.Size = UDim2.new(0, 204, 0, 31)
HUD.Font = Enum.Font.Roboto
HUD.Text = "HUD"
HUD.TextColor3 = Color3.fromRGB(0, 0, 0)
HUD.TextSize = 25.000

ChillLofi1.Name = "ChillLofi1"
ChillLofi1.Parent = Gui
ChillLofi1.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChillLofi1.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi1.BorderSizePixel = 0
ChillLofi1.Position = UDim2.new(0, 0, 0.319587618, 0)
ChillLofi1.Size = UDim2.new(0, 204, 0, 31)
ChillLofi1.Font = Enum.Font.Roboto
ChillLofi1.Text = "ChillLofi1"
ChillLofi1.TextColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi1.TextSize = 25.000
ChillLofi1.MouseButton1Down:connect(function()
    if not ChillLofi1CheatOn == true then
        ChillLofi1CheatOn = true

        ChillLofi1.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChillLofi1.TextColor3 = Color3.fromRGB(255, 255, 255) 

        Lofi1:Play()

    elseif ChillLofi1CheatOn == true then
        ChillLofi1CheatOn = false

        ChillLofi1.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChillLofi1.TextColor3 = Color3.fromRGB(0, 0, 0)  

        Lofi1:Stop()
        
    end
end)

ChillLofi1KeyBind = Enum.KeyCode.Escape.Value

ChillLofi1.MouseButton2Down:connect(function()
    print('clicked')
    ChillLofi1.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChillLofi1KeyBind = Enum.KeyCode.Escape.Value
            else 
                ChillLofi1KeyBind = input.KeyCode.Value
            end
            selecting = false
            ChillLofi1.Text = "ChillLofi1"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChillLofi1KeyBind then
        for i,v in pairs(getconnections(ChillLofi1.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

ChillLofi4.Name = "ChillLofi4"
ChillLofi4.Parent = Gui
ChillLofi4.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChillLofi4.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi4.BorderSizePixel = 0
ChillLofi4.Position = UDim2.new(0, 0, 0.639175296, 0)
ChillLofi4.Size = UDim2.new(0, 204, 0, 31)
ChillLofi4.Font = Enum.Font.Roboto
ChillLofi4.Text = "ChillLofi4"
ChillLofi4.TextColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi4.TextSize = 25.000
ChillLofi4.MouseButton1Down:connect(function()
    if not ChillLofi4CheatOn == true then
        ChillLofi4CheatOn = true

        ChillLofi4.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChillLofi4.TextColor3 = Color3.fromRGB(255, 255, 255) 

        Lofi4:Play()

    elseif ChillLofi4CheatOn == true then
        ChillLofi4CheatOn = false

        ChillLofi4.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChillLofi4.TextColor3 = Color3.fromRGB(0, 0, 0)  

        Lofi4:Stop()
        
    end
end)

ChillLofi4KeyBind = Enum.KeyCode.Escape.Value

ChillLofi4.MouseButton2Down:connect(function()
    print('clicked')
    ChillLofi4.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChillLofi4KeyBind = Enum.KeyCode.Escape.Value
            else 
                ChillLofi4KeyBind = input.KeyCode.Value
            end
            selecting = false
            ChillLofi4.Text = "ChillLofi4"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChillLofi4KeyBind then
        for i,v in pairs(getconnections(ChillLofi4.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

ChillLofi5.Name = "ChillLofi5"
ChillLofi5.Parent = Gui
ChillLofi5.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChillLofi5.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi5.BorderSizePixel = 0
ChillLofi5.Position = UDim2.new(0, 0, 0.745704532, 0)
ChillLofi5.Size = UDim2.new(0, 204, 0, 31)
ChillLofi5.Font = Enum.Font.Roboto
ChillLofi5.Text = "ChillLofi5"
ChillLofi5.TextColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi5.TextSize = 25.000
ChillLofi5.MouseButton1Down:connect(function()
    if not ChillLofi5CheatOn == true then
        ChillLofi5CheatOn = true

        ChillLofi5.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChillLofi5.TextColor3 = Color3.fromRGB(255, 255, 255) 

        Lofi5:Play()

    elseif ChillLofi5CheatOn == true then
        ChillLofi5CheatOn = false

        ChillLofi5.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChillLofi5.TextColor3 = Color3.fromRGB(0, 0, 0)  

        Lofi5:Stop()
        
    end
end)

ChillLofi5KeyBind = Enum.KeyCode.Escape.Value

ChillLofi5.MouseButton2Down:connect(function()
    print('clicked')
    ChillLofi5.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChillLofi5KeyBind = Enum.KeyCode.Escape.Value
            else 
                ChillLofi5KeyBind = input.KeyCode.Value
            end
            selecting = false
            ChillLofi5.Text = "ChillLofi5"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChillLofi5KeyBind then
        for i,v in pairs(getconnections(ChillLofi5.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

ChillLofi2.Name = "ChillLofi2"
ChillLofi2.Parent = Gui
ChillLofi2.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChillLofi2.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi2.BorderSizePixel = 0
ChillLofi2.Position = UDim2.new(0, 0, 0.426116824, 0)
ChillLofi2.Size = UDim2.new(0, 204, 0, 31)
ChillLofi2.Font = Enum.Font.Roboto
ChillLofi2.Text = "ChillLofi2"
ChillLofi2.TextColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi2.TextSize = 25.000
ChillLofi2.MouseButton1Down:connect(function()
    if not ChillLofi2CheatOn == true then
        ChillLofi2CheatOn = true

        ChillLofi2.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChillLofi2.TextColor3 = Color3.fromRGB(255, 255, 255) 

        Lofi2:Play()

    elseif ChillLofi2CheatOn == true then
        ChillLofi2CheatOn = false

        ChillLofi2.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChillLofi2.TextColor3 = Color3.fromRGB(0, 0, 0)  

        Lofi2:Stop()
        
    end
end)

ChillLofi2KeyBind = Enum.KeyCode.Escape.Value

ChillLofi2.MouseButton2Down:connect(function()
    print('clicked')
    ChillLofi2.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChillLofi2KeyBind = Enum.KeyCode.Escape.Value
            else 
                ChillLofi2KeyBind = input.KeyCode.Value
            end
            selecting = false
            ChillLofi2.Text = "ChillLofi2"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChillLofi2KeyBind then
        for i,v in pairs(getconnections(ChillLofi2.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

ChillLofi3.Name = "ChillLofi3"
ChillLofi3.Parent = Gui
ChillLofi3.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ChillLofi3.BorderColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi3.BorderSizePixel = 0
ChillLofi3.Position = UDim2.new(0, 0, 0.53264606, 0)
ChillLofi3.Size = UDim2.new(0, 204, 0, 31)
ChillLofi3.Font = Enum.Font.Roboto
ChillLofi3.Text = "ChillLofi3"
ChillLofi3.TextColor3 = Color3.fromRGB(0, 0, 0)
ChillLofi3.TextSize = 25.000
ChillLofi3.MouseButton1Down:connect(function()
    if not ChillLofi3CheatOn == true then
        ChillLofi3CheatOn = true

        ChillLofi3.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ChillLofi3.TextColor3 = Color3.fromRGB(255, 255, 255) 

        Lofi3:Play()

    elseif ChillLofi3CheatOn == true then
        ChillLofi3CheatOn = false

        ChillLofi3.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ChillLofi3.TextColor3 = Color3.fromRGB(0, 0, 0)  

        Lofi3:Stop()
        
    end
end)

ChillLofi3KeyBind = Enum.KeyCode.Escape.Value

ChillLofi3.MouseButton2Down:connect(function()
    print('clicked')
    ChillLofi3.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChillLofi3KeyBind = Enum.KeyCode.Escape.Value
            else 
                ChillLofi3KeyBind = input.KeyCode.Value
            end
            selecting = false
            ChillLofi3.Text = "ChillLofi3"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChillLofi3KeyBind then
        for i,v in pairs(getconnections(ChillLofi3.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

World.Name = "World"
World.Parent = Sigma_1
World.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
World.BorderColor3 = Color3.fromRGB(255, 255, 255)
World.BorderSizePixel = 0
World.Position = UDim2.new(0.254558742, 0, 0.511700451, 0)
World.Size = UDim2.new(0, 204, 0, 291)
World.Visible = true
World.Active = true
World.Draggable = true

Cover6.Name = "Cover6"
Cover6.Parent = World
Cover6.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover6.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover6.BorderSizePixel = 0
Cover6.Position = UDim2.new(0.60799998, 0, 0, 0)
Cover6.Size = UDim2.new(0, 79, 0, 62)

WorldText.Name = "WorldText"
WorldText.Parent = World
WorldText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
WorldText.BorderColor3 = Color3.fromRGB(0, 0, 0)
WorldText.BorderSizePixel = 0
WorldText.Position = UDim2.new(0, 0, -0.00105012977, 0)
WorldText.Size = UDim2.new(0, 124, 0, 63)
WorldText.Font = Enum.Font.Roboto
WorldText.Text = "World"
WorldText.TextColor3 = Color3.fromRGB(120, 120, 120)
WorldText.TextSize = 32.000

AntiVoid.Name = "AntiVoid"
AntiVoid.Parent = World
AntiVoid.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AntiVoid.BorderColor3 = Color3.fromRGB(0, 0, 0)
AntiVoid.BorderSizePixel = 0
AntiVoid.Position = UDim2.new(0, 0, 0.213058412, 0)
AntiVoid.Size = UDim2.new(0, 204, 0, 31)
AntiVoid.Font = Enum.Font.Roboto
AntiVoid.Text = "AntiVoid"
AntiVoid.TextColor3 = Color3.fromRGB(0, 0, 0)
AntiVoid.TextSize = 25.000
AntiVoid.MouseButton1Down:connect(function()
    if not AntiVoidCheatOn == true then
        AntiVoidCheatOn = true

        AntiVoid.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AntiVoid.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play() 

        parts = {}

        lowest = math.huge

        for _, child in pairs(workspace:GetDescendants()) do
            if child:IsA("BasePart") then
                if child.Position.Y < lowest and child.Position.Y > -1000 then
                    lowest = child.Position.Y
                end
            end
        end

        print(lowest)

        local NewPart = Instance.new("Part")
        NewPart.Name = "ANTIVOID"
        NewPart.Position = Vector3.new(0,5,0) --Position of the part
        NewPart.Anchored = true
        NewPart.CanCollide = false
        NewPart.Size = Vector3.new(3000,1,3000) 
        NewPart.Color = Color3.fromRGB(255,0,0)
        NewPart.Transparency = 0.6
        NewPart.Parent = workspace

        local character = game.Players.LocalPlayer.Character
            
        local humanoid = character:WaitForChild("Humanoid")

        touching = false
        NeedReset = false

        local pos = {}

        local timePassed = tick()

        local function antiVoidFunction(newpart, limb)
            if newpart.Name == 'ANTIVOID' then
                if tick() - timePassed >= 1 then
                    timePassed = tick()
                    touching = true


                    spawn(function()
                        for i = 1, 25 do
                            task.wait(0.04)
                            if lplr.Character and lplr.Character.Parent ~= nil and lplr.Character:FindFirstChild("Humanoid") and lplr.Character.Humanoid.Health >= 0 and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                                --if lplr.Character.Humanoid.Health <= 0 then repeat task.wait() until lplr.isAlive and lplr.Character.Humanoid.Health > 0 break end
                                lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
                            end
                        end
                    end)

                    pos = {}
                    wait()
                    touching = false
                end
            end
        end

        game.Players.LocalPlayer.CharacterAdded:Connect(function()
            local humanoid = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
            humanoid.Touched:Connect(antiVoidFunction)
        end)

        humanoid.Touched:Connect(antiVoidFunction)


        spawn(function()
            --while AntiVoidCheatOn == true do
            if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character.Parent ~= nil and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") and game.Players.LocalPlayer.Character.Humanoid.Health >= 0 and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") or AntiCheatDisablerCheatOn == true then
                xcframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.X
                if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                    ycframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.Y
                end
                if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                    Zcframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.Z
                end
                if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                    NewPart.CFrame = CFrame.new(xcframe, 3, Zcframe) --5
                end
            end
                --wait()
            --end
        end)


    elseif AntiVoidCheatOn == true then
        AntiVoidCheatOn = false

        game.Workspace.ANTIVOID:Destroy()
        
        AntiVoid.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AntiVoid.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        
    end
end)

AntiVoidKeyBind = Enum.KeyCode.Escape.Value

AntiVoid.MouseButton2Down:connect(function()
    print('clicked')
    AntiVoid.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AntiVoidKeyBind = Enum.KeyCode.Escape.Value
            else 
                AntiVoidKeyBind = input.KeyCode.Value
            end
            selecting = false
            AntiVoid.Text = "AntiVoid"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == AntiVoidKeyBind then
        for i,v in pairs(getconnections(AntiVoid.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

Timer.Name = "Timer"
Timer.Parent = World
Timer.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Timer.BorderColor3 = Color3.fromRGB(0, 0, 0)
Timer.BorderSizePixel = 0
Timer.Position = UDim2.new(0, 0, 0.319587618, 0)
Timer.Size = UDim2.new(0, 204, 0, 31)
Timer.Font = Enum.Font.Roboto
Timer.Text = "Timer"
Timer.TextColor3 = Color3.fromRGB(0, 0, 0)
Timer.TextSize = 25.000

ClickTP.Name = "ClickTP"
ClickTP.Parent = World
ClickTP.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ClickTP.BorderColor3 = Color3.fromRGB(0, 0, 0)
ClickTP.BorderSizePixel = 0
ClickTP.Position = UDim2.new(0, 0, 0.639175296, 0)
ClickTP.Size = UDim2.new(0, 204, 0, 31)
ClickTP.Font = Enum.Font.Roboto
ClickTP.Text = "ClickTP"
ClickTP.TextColor3 = Color3.fromRGB(0, 0, 0)
ClickTP.TextSize = 25.000

AntiCheatDisabler.Name = "AntiCheatDisabler"
AntiCheatDisabler.Parent = World
AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AntiCheatDisabler.BorderColor3 = Color3.fromRGB(0, 0, 0)
AntiCheatDisabler.BorderSizePixel = 0
AntiCheatDisabler.Position = UDim2.new(0, 0, 0.745704532, 0)
AntiCheatDisabler.Size = UDim2.new(0, 204, 0, 31)
AntiCheatDisabler.Font = Enum.Font.Roboto
AntiCheatDisabler.Text = "AntiCheatDisabler"
AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)
AntiCheatDisabler.TextSize = 25.000
AntiCheatDisabler.MouseButton1Down:connect(function()
    if not AntiCheatDisablerCheatOn == true then
        --On:Play()

        repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
        if matchState == 1 then
            game.StarterGui:SetCore("SendNotification", {
                Title = "AntiCheatDisabler";
                Text = "Anticheatdisabler can't be used once the game has started.";
                Icon = "";
                Duration = 3;
            })

            AntiCheatDisablerCheatOn = false
            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
            AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
    elseif matchState == 0 then
            AntiCheatDisablerCheatOn = true
            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
            AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
             
            anticheatdisabler()
        end

    elseif AntiCheatDisablerCheatOn == true then
        AntiCheatDisablerCheatOn = false

        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0) 

        game.StarterGui:SetCore("SendNotification", {
            Title = "AntiCheatDisabler";
            Text = "Anticheatdisabler with turn off next round.";
            Icon = "";
            Duration = 3;
        })
    end
end)


AntiCheatDisablerKeyBind = Enum.KeyCode.Escape.Value

AntiCheatDisabler.MouseButton2Down:connect(function()
    print('clicked')
    AntiCheatDisabler.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AntiCheatDisablerKeyBind = Enum.KeyCode.Escape.Value
            else 
                AntiCheatDisablerKeyBind = input.KeyCode.Value
            end
            selecting = false
            AntiCheatDisabler.Text = "AntiCheatDisabler"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == AntiCheatDisablerKeyBind then
        --for i,v in pairs(getconnections(AntiCheatDisabler.MouseButton1Down)) do
            AntiCheatDisablerCheatOn = true
            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
            AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255)  

            --On:Play()

            repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
            if matchState == 1 then
                game.StarterGui:SetCore("SendNotification", {
                    Title = "AntiCheatDisabler";
                    Text = "Anticheatdisabler can't be used once the game has started.";
                    Icon = "";
                    Duration = 3;
                })
                wait(0.3)
                --AntiCheatDisablerCheatOn = false
                AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
        elseif matchState == 0 then
                AntiCheatDisablerCheatOn = true
                AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
                
                anticheatdisabler()
            end
        --end  
    end
end)

FPSBoost.Name = "FPSBoost"
FPSBoost.Parent = World
FPSBoost.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
FPSBoost.BorderColor3 = Color3.fromRGB(0, 0, 0)
FPSBoost.BorderSizePixel = 0
FPSBoost.Position = UDim2.new(0, 0, 0.426116824, 0)
FPSBoost.Size = UDim2.new(0, 204, 0, 31)
FPSBoost.Font = Enum.Font.Roboto
FPSBoost.Text = "FPSBoost"
FPSBoost.TextColor3 = Color3.fromRGB(0, 0, 0)
FPSBoost.TextSize = 25.000

FastFall.Name = "FastFall"
FastFall.Parent = World
FastFall.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
FastFall.BorderColor3 = Color3.fromRGB(0, 0, 0)
FastFall.BorderSizePixel = 0
FastFall.Position = UDim2.new(0, 0, 0.53264606, 0)
FastFall.Size = UDim2.new(0, 204, 0, 31)
FastFall.Font = Enum.Font.Roboto
FastFall.Text = "FastFall"
FastFall.TextColor3 = Color3.fromRGB(0, 0, 0)
FastFall.TextSize = 25.000
FastFall.MouseButton1Down:connect(function()
    if not FastFallCheatOn == true then
        FastFallCheatOn = true

        FastFall.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        FastFall.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()


        spawn(function()
            while FastFallCheatOn == true do
                if FastFallCheatOn == true and FlyCheatOn == false and Fly2CheatOn == false and LongJumpCheatOn == false and HighJumpCheatOn == false then
                    workspace.Gravity = 400
                    else
                end
                wait()
            end
        end)



    elseif FastFallCheatOn == true then
        FastFallCheatOn = false

        FastFall.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        FastFall.TextColor3 = Color3.fromRGB(0, 0, 0)

        workspace.Gravity = 196.2
        wait()
        workspace.Gravity = 196.2

        --Off:Play()  
        
    end
end)


FastFallKeyBind = Enum.KeyCode.Escape.Value

FastFall.MouseButton2Down:connect(function()
    print('clicked')
    FastFall.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                FastFallKeyBind = Enum.KeyCode.Escape.Value
            else 
                FastFallKeyBind = input.KeyCode.Value
            end
            selecting = false
            FastFall.Text = "FastFall"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == FastFallKeyBind then
        for i,v in pairs(getconnections(FastFall.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

Misc.Name = "Misc"
Misc.Parent = Sigma_1
Misc.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Misc.BorderColor3 = Color3.fromRGB(255, 255, 255)
Misc.BorderSizePixel = 0
Misc.Position = UDim2.new(0.435448587, 0, 0.511700451, 0)
Misc.Size = UDim2.new(0, 204, 0, 291)
Misc.Visible = true
Misc.Active = true
Misc.Draggable = true

MiscText.Name = "MiscText"
MiscText.Parent = Misc
MiscText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
MiscText.BorderColor3 = Color3.fromRGB(0, 0, 0)
MiscText.BorderSizePixel = 0
MiscText.Position = UDim2.new(0, 0, -0.00105018227, 0)
MiscText.Size = UDim2.new(0, 100, 0, 63)
MiscText.Font = Enum.Font.Roboto
MiscText.Text = "Misc"
MiscText.TextColor3 = Color3.fromRGB(120, 120, 120)
MiscText.TextSize = 32.000

Cover7.Name = "Cover7"
Cover7.Parent = Misc
Cover7.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover7.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover7.BorderSizePixel = 0
Cover7.Position = UDim2.new(0.490196079, 0, 7.12076799e-05, 0)
Cover7.Size = UDim2.new(0, 104, 0, 63)

AutoQueue.Name = "AutoQueue"
AutoQueue.Parent = Misc
AutoQueue.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AutoQueue.BorderColor3 = Color3.fromRGB(0, 0, 0)
AutoQueue.BorderSizePixel = 0
AutoQueue.Position = UDim2.new(0, 0, 0.213058412, 0)
AutoQueue.Size = UDim2.new(0, 204, 0, 31)
AutoQueue.Font = Enum.Font.Roboto
AutoQueue.Text = "AutoQueue"
AutoQueue.TextColor3 = Color3.fromRGB(0, 0, 0)
AutoQueue.TextSize = 25.000
AutoQueue.MouseButton1Down:connect(function()
    if  AutoQueueCheatOn == false then
        AutoQueueCheatOn = true

        AutoQueue.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AutoQueue.TextColor3 = Color3.fromRGB(255, 255, 255) 

        --On:Play() clientstorestate.Game.queueType


        while AutoQueueCheatOn == true do
            local clientstorestate = bedwars["ClientStoreHandler"]:getState()
            matchState = clientstorestate.Game.matchState or 0

            if (matchState == 2) or (game.PlaceId == 6872265039) or (lplr.Character.HumanoidRootPart == nil) then
                game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                queuedup = true
            end

            if queuedup == true then
                queuedup = false
                break
            end
            wait()
        end

        elseif AutoQueueCheatOn == true then
            AutoQueueCheatOn = false
            AutoQueue.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
            AutoQueue.TextColor3 = Color3.fromRGB(0, 0, 0) 

            --Off:Play()

            game:GetService("ReplicatedStorage"):FindFirstChild("events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events").leaveQueue:FireServer()

    end
end)

AutoQueueKeyBind = Enum.KeyCode.Escape.Value

AutoQueue.MouseButton2Down:connect(function()
    print('clicked')
    AutoQueue.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AutoQueueKeyBind = Enum.KeyCode.Escape.Value
            else 
                AutoQueueKeyBind = input.KeyCode.Value
            end
            selecting = false
            AutoQueue.Text = "AutoQueue"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == AutoQueueKeyBind then
        for i,v in pairs(getconnections(AutoQueue.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

AntiAFK.Name = "AntiAFK"
AntiAFK.Parent = Misc
AntiAFK.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AntiAFK.BorderColor3 = Color3.fromRGB(0, 0, 0)
AntiAFK.BorderSizePixel = 0
AntiAFK.Position = UDim2.new(0, 0, 0.319587618, 0)
AntiAFK.Size = UDim2.new(0, 204, 0, 31)
AntiAFK.Font = Enum.Font.Roboto
AntiAFK.Text = "AntiAFK"
AntiAFK.TextColor3 = Color3.fromRGB(0, 0, 0)
AntiAFK.TextSize = 25.000
AntiAFK.MouseButton1Down:connect(function()
    if not AntiAFKCheatOn == true then
        AntiAFKCheatOn = true

        spawn(function()
            while AntiAFKCheatOn == true do
                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                wait(1)
            end
        end)


        AntiAFK.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AntiAFK.TextColor3 = Color3.fromRGB(255, 255, 255)  

        AntiAFKCheatOn = true

        --On:Play()   

    elseif AntiAFKCheatOn == true then
        AntiAFKCheatOn = false
        
        AntiAFK.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AntiAFK.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        
    end
end)

AntiAFKKeyBind = Enum.KeyCode.Escape.Value

AntiAFK.MouseButton2Down:connect(function()
    print('clicked')
    AntiAFK.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AntiAFKKeyBind = Enum.KeyCode.Escape.Value
            else 
                AntiAFKKeyBind = input.KeyCode.Value
            end
            selecting = false
            AntiAFK.Text = "AntiAFK"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == AntiAFKKeyBind then
        for i,v in pairs(getconnections(AntiAFK.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

CopyDiscordLink.Name = "CopyDiscordLink"
CopyDiscordLink.Parent = Misc
CopyDiscordLink.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
CopyDiscordLink.BorderColor3 = Color3.fromRGB(0, 0, 0)
CopyDiscordLink.BorderSizePixel = 0
CopyDiscordLink.Position = UDim2.new(0, 0, 0.426116824, 0)
CopyDiscordLink.Size = UDim2.new(0, 204, 0, 31)
CopyDiscordLink.Font = Enum.Font.Roboto
CopyDiscordLink.Text = "Copy Discord Link"
CopyDiscordLink.TextColor3 = Color3.fromRGB(0, 0, 0)
CopyDiscordLink.TextSize = 25.000

DinoFly.Name = "DinoFly"
DinoFly.Parent = Misc
DinoFly.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
DinoFly.BorderColor3 = Color3.fromRGB(0, 0, 0)
DinoFly.BorderSizePixel = 0
DinoFly.Position = UDim2.new(0, 0, 0.53264603, 0)
DinoFly.Size = UDim2.new(0, 204, 0, 31)
DinoFly.Font = Enum.Font.Roboto
DinoFly.Text = "DinoFly"
DinoFly.TextColor3 = Color3.fromRGB(0, 0, 0)
DinoFly.TextSize = 25.000
DinoFly.MouseButton1Down:connect(function()
    if not DinoFlyCheatOn == true then
        DinoFlyCheatOn = true

        DinoFly.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        DinoFly.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play() 

    elseif DinoFlyCheatOn == true then
        DinoFlyCheatOn = false
        
        DinoFly.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        DinoFly.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        
    end
end)

DinoFlyKeyBind = Enum.KeyCode.Escape.Value

DinoFly.MouseButton2Down:connect(function()
    print('clicked')
    DinoFly.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                DinoFlyKeyBind = Enum.KeyCode.Escape.Value
            else 
                DinoFlyKeyBind = input.KeyCode.Value
            end
            selecting = false
            DinoFly.Text = "DinoFly"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == DinoFlyKeyBind then
        for i,v in pairs(getconnections(DinoFly.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

BlatantMode.Name = "BlatantMode"
BlatantMode.Parent = Misc
BlatantMode.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
BlatantMode.BorderColor3 = Color3.fromRGB(0, 0, 0)
BlatantMode.BorderSizePixel = 0
BlatantMode.Position = UDim2.new(0, 0, 0.639175206, 0)
BlatantMode.Size = UDim2.new(0, 204, 0, 31)
BlatantMode.Font = Enum.Font.Roboto
BlatantMode.Text = "BlatantMode"
BlatantMode.TextColor3 = Color3.fromRGB(0, 0, 0)
BlatantMode.TextSize = 25.000
BlatantMode.MouseButton1Down:connect(function()
    if not BlatantModeCheatOn == true then
        BlatantModeCheatOn = true

        BlatantMode.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        BlatantMode.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()

        wait(1)

        while BlatantModeCheatOn == true do
            local clientstorestate = bedwars["ClientStoreHandler"]:getState()
            matchState = clientstorestate.Game.matchState or 0

            if matchState == 1 and AntiCheatDisablerCheatOn == false or matchState == 0 and AntiCheatDisablerCheatOn == false then
                cantblatantmode = true
            end

            if cantblatantmode == true then
                cantblatantmode = false

                BlatantModeCheatOn = false
                BlatantMode.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                BlatantMode.TextColor3 = Color3.fromRGB(0, 0, 0)  

                game.StarterGui:SetCore("SendNotification", {
                    Title = "BlatantMode";
                    Text = "BlatantMode can't be enabled without anticheatdisabler.";
                    Icon = "";
                    Duration = 4;
                })

                break
            end
            wait()
        end  

    elseif BlatantModeCheatOn == true then
        BlatantModeCheatOn = false
        
        BlatantMode.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        BlatantMode.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        
    end
end)

BlatantModeKeyBind = Enum.KeyCode.Escape.Value

BlatantMode.MouseButton2Down:connect(function()
    print('clicked')
    BlatantMode.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                BlatantModeKeyBind = Enum.KeyCode.Escape.Value
            else 
                BlatantModeKeyBind = input.KeyCode.Value
            end
            selecting = false
            BlatantMode.Text = "BlatantMode"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == BlatantModeKeyBind then
        for i,v in pairs(getconnections(BlatantMode.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

AutoSkywarsWin.Name = "AutoSkywarsWin"
AutoSkywarsWin.Parent = Misc
AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
AutoSkywarsWin.BorderColor3 = Color3.fromRGB(0, 0, 0)
AutoSkywarsWin.BorderSizePixel = 0
AutoSkywarsWin.Position = UDim2.new(0, 0, 0.745704382, 0)
AutoSkywarsWin.Size = UDim2.new(0, 204, 0, 31)
AutoSkywarsWin.Font = Enum.Font.Roboto
AutoSkywarsWin.Text = "AutoSkywarsWin"
AutoSkywarsWin.TextColor3 = Color3.fromRGB(0, 0, 0)
AutoSkywarsWin.TextSize = 25.000
AutoSkywarsWin.MouseButton1Down:connect(function()
    if not AutoSkywarsWinCheatOn == true then
        AutoSkywarsWinCheatOn = true

        AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AutoSkywarsWin.TextColor3 = Color3.fromRGB(255, 255, 255)

        repeat wait() until matchState == 1

        if matchState == 1 and AntiCheatDisablerCheatOn == false then  
            game.StarterGui:SetCore("SendNotification", {
                Title = "AutoSkywarsWin";
                Text = "AutoSkywarsWin can only be started before the game begins.";
                Icon = "";
                Duration = 6;
            })

            AutoSkywarsWinCheatOn = false
            AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
            AutoSkywarsWin.TextColor3 = Color3.fromRGB(0, 0, 0) 

           elseif matchState == 0 and AntiCheatDisablerCheatOn == true or matchState == 0 and AntiCheatDisablerCheatOn == false or matchState == 1 and AntiCheatDisablerCheatOn == true then   
            --On:Play() 

            local humrp = lplr.Character.HumanoidRootPart

            players = game.Players:GetChildren()

            spawn(function()
                wait(70)
                if AutoSkywarsWinCheatOn then
                    game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                end
            end)

    --------------------------------------------------------loading other cheats
            spawn(function()
                if not MultiAuraCheatOn == true then
                    MultiAuraCheatOn = true

                    MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

                    --On:Play()

                    bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

                    repeat
                        task.wait(0.03)
                        local plrs = GetAllNearestHumanoidToPosition(18.8)
                        for i,plr in pairs(plrs) do
                            local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                            local newpos = plr.Character.HumanoidRootPart.Position
                            bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
                        end
                    until MultiAuraCheatOn == false 
                end
            end)

            spawn(function()
                if not AntiCheatDisablerCheatOn == true then
                    AntiCheatDisablerCheatOn = true
                    AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255)  

                    --On:Play()

                    repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
                    if matchState == 1 then
                        game.StarterGui:SetCore("SendNotification", {
                            Title = "AntiCheatDisabler";
                            Text = "Anticheatdisabler can't be used once the game has started.";
                            Icon = "";
                            Duration = 3;
                        })
                        wait(0.5)
                        --AntiCheatDisablerCheatOn = false
                        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                        AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
                elseif matchState == 0 then
                        AntiCheatDisablerCheatOn = true
                        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                        AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        
                        anticheatdisabler()
                    end
                end
            end)

            spawn(function()
                if not AutoQueueCheatOn == true then
                    AutoQueueCheatOn = true
                    AutoQueue.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    AutoQueue.TextColor3 = Color3.fromRGB(255, 255, 255) 

                    while AutoQueueCheatOn == true do
                        local clientstorestate = bedwars["ClientStoreHandler"]:getState()
                        matchState = clientstorestate.Game.matchState or 0

                        if (matchState == 2) or (game.PlaceId == 6872265039) or (lplr.Character.HumanoidRootPart == nil) then
                            game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                            queuedup = true
                        end

                        if queuedup == true then
                            queuedup = false
                            break
                        end
                        wait()
                    end
                end
            end)
    --------------------------------------------------------loading other cheats
            pcall(function()
                while AutoSkywarsWinCheatOn == true do
                    for i,v in pairs(players) do
                        if v.Character and AutoSkywarsWinCheatOn == true then
                            if not (v.Name == lplr.Name) and not (v.Team.TeamColor == lplr.Team.TeamColor) and v.Character:FindFirstChild("Humanoid") and not (v.Character.Humanoid.Health <= 0) and AutoSkywarsWinCheatOn == true then
                                repeat wait(0.1)
                                    if v.Character and (v.Character:FindFirstChild("Head")) and AutoSkywarsWinCheatOn == true then
                                        character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                                        --character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 4, 0))
                                    end
                                until v.Character.Humanoid.Health <= 0 or v.Character:FindFirstChild("HumanoidRootPart") == nil or AutoSkywarsWinCheatOn == false
                            end
                        end
                        wait()
                    end
                    wait()
                end
            end)
        end

    elseif AutoSkywarsWinCheatOn == true then
        AutoSkywarsWinCheatOn = false
        
        AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        AutoSkywarsWin.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        
    end
end)

AutoSkywarsWinKeyBind = Enum.KeyCode.Escape.Value

AutoSkywarsWin.MouseButton2Down:connect(function()
    print('clicked')
    AutoSkywarsWin.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                AutoSkywarsWinKeyBind = Enum.KeyCode.Escape.Value
            else 
                AutoSkywarsWinKeyBind = input.KeyCode.Value
            end
            selecting = false
            AutoSkywarsWin.Text = "AutoSkywarsWin"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == AutoSkywarsWinKeyBind then
        AutoSkywarsWinCheatOn = true
        AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        AutoSkywarsWin.TextColor3 = Color3.fromRGB(255, 255, 255)

        repeat wait() until matchState == 1

        if matchState == 1 and AntiCheatDisablerCheatOn == false then  
            game.StarterGui:SetCore("SendNotification", {
                Title = "AutoSkywarsWin";
                Text = "AutoSkywarsWin can only be started before the game begins.";
                Icon = "";
                Duration = 6;
            })

            AutoSkywarsWinCheatOn = false
            AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
            AutoSkywarsWin.TextColor3 = Color3.fromRGB(0, 0, 0)  

           elseif matchState == 0 and AntiCheatDisablerCheatOn == true or matchState == 0 and AntiCheatDisablerCheatOn == false or matchState == 1 and AntiCheatDisablerCheatOn == true then   
            --On:Play() 

            local humrp = lplr.Character.HumanoidRootPart

            players = game.Players:GetChildren()

            spawn(function()
                wait(70)
                if AutoSkywarsWinCheatOn then
                    game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                end
            end)

    --------------------------------------------------------loading other cheats
            spawn(function()
                if not MultiAuraCheatOn == true then
                    MultiAuraCheatOn = true

                    MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

                    --On:Play()

                    bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

                    repeat
                        task.wait(0.03)
                        local plrs = GetAllNearestHumanoidToPosition(18.8)
                        for i,plr in pairs(plrs) do
                            local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                            local newpos = plr.Character.HumanoidRootPart.Position
                            bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
                        end
                    until MultiAuraCheatOn == false 
                end
            end)

            spawn(function()
                if not AntiCheatDisablerCheatOn == true then
                    AntiCheatDisablerCheatOn = true
                    AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255)  

                    --On:Play()

                    repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
                    if matchState == 1 then
                        game.StarterGui:SetCore("SendNotification", {
                            Title = "AntiCheatDisabler";
                            Text = "Anticheatdisabler can't be used once the game has started.";
                            Icon = "";
                            Duration = 3;
                        })
                        wait(0.5)
                        --AntiCheatDisablerCheatOn = false
                        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                        AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
                elseif matchState == 0 then
                        AntiCheatDisablerCheatOn = true
                        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                        AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        
                        anticheatdisabler()
                    end
                end
            end)

            spawn(function()
                if not AutoQueueCheatOn == true then
                    AutoQueueCheatOn = true
                    AutoQueue.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                    AutoQueue.TextColor3 = Color3.fromRGB(255, 255, 255) 

                    while AutoQueueCheatOn == true do
                        local clientstorestate = bedwars["ClientStoreHandler"]:getState()
                        matchState = clientstorestate.Game.matchState or 0

                        if (matchState == 2) or (game.PlaceId == 6872265039) or (lplr.Character.HumanoidRootPart == nil) then
                            game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                            queuedup = true
                        end

                        if queuedup == true then
                            queuedup = false
                            break
                        end
                        wait()
                    end
                end
            end)
    --------------------------------------------------------loading other cheats
            pcall(function()
                while AutoSkywarsWinCheatOn == true do
                    for i,v in pairs(players) do
                        if v.Character and AutoSkywarsWinCheatOn == true then
                            if not (v.Name == lplr.Name) and not (v.Team.TeamColor == lplr.Team.TeamColor) and v.Character:FindFirstChild("Humanoid") and not (v.Character.Humanoid.Health <= 0) and AutoSkywarsWinCheatOn == true then
                                repeat wait(0.1)
                                    if v.Character and (v.Character:FindFirstChild("Head")) and AutoSkywarsWinCheatOn == true then
                                        character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                                        --character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 4, 0))
                                    end
                                until v.Character.Humanoid.Health <= 0 or v.Character:FindFirstChild("HumanoidRootPart") == nil or AutoSkywarsWinCheatOn == false
                            end
                        end
                        wait()
                    end
                    wait()
                end
            end)
        end 
    end
end)

Render.Name = "Render"
Render.Parent = Sigma_1
Render.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Render.BorderColor3 = Color3.fromRGB(255, 255, 255)
Render.BorderSizePixel = 0
Render.Position = UDim2.new(0.619985402, 0, 0.511700451, 0)
Render.Size = UDim2.new(0, 204, 0, 291)
Render.Visible = true
Render.Active = true
Render.Draggable = true

Cover8.Name = "Cover8"
Cover8.Parent = Render
Cover8.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
Cover8.BorderColor3 = Color3.fromRGB(0, 0, 0)
Cover8.BorderSizePixel = 0
Cover8.Position = UDim2.new(0.588235319, 0, 7.12076799e-05, 0)
Cover8.Size = UDim2.new(0, 84, 0, 63)

RenderText.Name = "RenderText"
RenderText.Parent = Render
RenderText.BackgroundColor3 = Color3.fromRGB(243, 243, 243)
RenderText.BorderColor3 = Color3.fromRGB(0, 0, 0)
RenderText.BorderSizePixel = 0
RenderText.Position = UDim2.new(0, 0, -0.00105018227, 0)
RenderText.Size = UDim2.new(0, 120, 0, 63)
RenderText.Font = Enum.Font.Roboto
RenderText.Text = "Render"
RenderText.TextColor3 = Color3.fromRGB(120, 120, 120)
RenderText.TextSize = 32.000

local Player = game:GetService("Players").LocalPlayer
local Camera = game:GetService("Workspace").CurrentCamera
local Mouse = Player:GetMouse()

local function Dist(pointA, pointB) -- magnitude errors for some reason : (
    return math.sqrt(math.pow(pointA.X - pointB.X, 2) + math.pow(pointA.Y - pointB.Y, 2))
end

local function GetClosest(points, dest)
    local min = math.huge
    local closest = nil
    for _, v in pairs(points) do
        local dist = Dist(v, dest)
        if dist < min then
            min = dist
            closest = v
        end
    end
    return closest
end

local function DrawESP(plr)
    local Box = Drawing.new("Quad")
    Box.Visible = false
    Box.PointA = Vector2.new(0, 0)
    Box.PointB = Vector2.new(0, 0)
    Box.PointC = Vector2.new(0, 0)
    Box.PointD = Vector2.new(0, 0)
    if game.Players.LocalPlayer.Team == plr.Team then
        Box.Color = Color3.fromRGB(0, 255, 0)
    else
        Box.Color = Color3.fromRGB(255, 0, 0)
    end
    Box.Thickness = 2
    Box.Transparency = 1

    local function Update()
        local c
        c =
            game:GetService("RunService").RenderStepped:Connect(
            function()
                if ESPCheatOn == false then
                    Box.Visible = false
                else
                    if
                        plr.Character ~= nil and plr.Character:FindFirstChildOfClass("Humanoid") ~= nil and
                            plr.Character:FindFirstChild("HumanoidRootPart") ~= nil and
                            plr.Character:FindFirstChildOfClass("Humanoid").Health > 0 and
                            plr.Character:FindFirstChild("Head") ~= nil
                    then
                        local pos, vis = Camera:WorldToViewportPoint(plr.Character.HumanoidRootPart.Position)
                        if vis then
                            local points = {}
                            local c = 0
                            for _, v in pairs(plr.Character:GetChildren()) do
                                if v:IsA("BasePart") then
                                    c = c + 1
                                    local p = Camera:WorldToViewportPoint(v.Position)
                                    if v.Name == "HumanoidRootPart" then
                                        p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(0, 0, -v.Size.Z)).p)
                                    elseif v.Name == "Head" then
                                        p =
                                            Camera:WorldToViewportPoint(
                                            (v.CFrame * CFrame.new(0, v.Size.Y / 2, v.Size.Z / 1.25)).p
                                        )
                                    elseif string.match(v.Name, "Left") then
                                        p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(-v.Size.X / 2, 0, 0)).p)
                                    elseif string.match(v.Name, "Right") then
                                        p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(v.Size.X / 2, 0, 0)).p)
                                    end
                                    points[c] = p
                                end
                            end
                            local Left = GetClosest(points, Vector2.new(0, pos.Y))
                            local Right = GetClosest(points, Vector2.new(Camera.ViewportSize.X, pos.Y))
                            local Top = GetClosest(points, Vector2.new(pos.X, 0))
                            local Bottom = GetClosest(points, Vector2.new(pos.X, Camera.ViewportSize.Y))

                            if Left ~= nil and Right ~= nil and Top ~= nil and Bottom ~= nil then
                                Box.PointA = Vector2.new(Right.X, Top.Y)
                                Box.PointB = Vector2.new(Left.X, Top.Y)
                                Box.PointC = Vector2.new(Left.X, Bottom.Y)
                                Box.PointD = Vector2.new(Right.X, Bottom.Y)

                                Box.Visible = true
                            else
                                Box.Visible = false
                            end
                        else
                            Box.Visible = false
                        end
                    else
                        Box.Visible = false
                        if game.Players:FindFirstChild(plr.Name) == nil then
                            c:Disconnect()
                        end
                    end
                end
            end
        )
    end
    coroutine.wrap(Update)()

end

for _, v in pairs(game:GetService("Players"):GetChildren()) do
    if v.Name ~= Player.Name then
        DrawESP(v)
    end
end

game:GetService("Players").PlayerAdded:Connect(
    function(v)
        DrawESP(v)
    end
)


ESP.Name = "ESP"
ESP.Parent = Render
ESP.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
ESP.BorderColor3 = Color3.fromRGB(0, 0, 0)
ESP.BorderSizePixel = 0
ESP.Position = UDim2.new(0, 0, 0.213058412, 0)
ESP.Size = UDim2.new(0, 204, 0, 31)
ESP.Font = Enum.Font.Roboto
ESP.Text = "ESP"
ESP.TextColor3 = Color3.fromRGB(0, 0, 0)
ESP.TextSize = 25.000
ESP.MouseButton1Down:connect(function()
    if not ESPCheatOn == true then
        ESPCheatOn = true

        ESP.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        ESP.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()  

    elseif ESPCheatOn == true then
        ESPCheatOn = false
        
        ESP.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        ESP.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()


        
    end
end)

ESPKeyBind = Enum.KeyCode.Escape.Value

ESP.MouseButton2Down:connect(function()
    print('clicked')
    ESP.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ESPKeyBind = Enum.KeyCode.Escape.Value
            else 
                ESPKeyBind = input.KeyCode.Value
            end
            selecting = false
            ESP.Text = "ESP"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == ESPKeyBind then
        for i,v in pairs(getconnections(ESP.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

Tracers.Name = "Tracers"
Tracers.Parent = Render
Tracers.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Tracers.BorderColor3 = Color3.fromRGB(0, 0, 0)
Tracers.BorderSizePixel = 0
Tracers.Position = UDim2.new(0, 0, 0.319587618, 0)
Tracers.Size = UDim2.new(0, 204, 0, 31)
Tracers.Font = Enum.Font.Roboto
Tracers.Text = "Tracers"
Tracers.TextColor3 = Color3.fromRGB(0, 0, 0)
Tracers.TextSize = 25.000
Tracers.MouseButton1Down:connect(function()
    if not TracersCheatOn == true then
        TracersCheatOn = true

        Tracers.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Tracers.TextColor3 = Color3.fromRGB(255, 255, 255)     

        TracersCheatOnSave = true

        --On:Play()

        local function API_Check()
            if Drawing == nil then
                return "No"
            else
                return "Yes"
            end
        end

        local Find_Required = API_Check()

        if Find_Required == "No" then
            game:GetService("StarterGui"):SetCore("SendNotification",{
                Title = "[ESP Cheat]";
                Text = "Your Executor Is Not Supported";
                Duration = math.huge;
                Button1 = "OK"
            })

            return
        end



        local RunService = game:GetService("RunService")
        local Players = game:GetService("Players")
        local Camera = game:GetService("Workspace").CurrentCamera
        local UserInputService = game:GetService("UserInputService")
        local TestService = game:GetService("TestService")

        local Typing = false

        _G.SendNotifications = false   -- If set to true then the script would notify you frequently on any changes applied and when loaded / errored. (If a game can detect this, it is recommended to set it to false)
        _G.DefaultSettings = false   -- If set to true then the tracer script would run with default settings regardless of any changes you made.

        _G.TeamCheck = false   -- If set to true then the script would create tracers only for the enemy team members.

        --[!]-- ONLY ONE OF THESE VALUES SHOULD BE SET TO TRUE TO NOT ERROR THE SCRIPT --[!]--

        _G.FromMouse = false   -- If set to true, the tracers will come from the position of your mouse curson on your screen.
        _G.FromCenter = false   -- If set to true, the tracers will come from the center of your screen.
        _G.FromBottom = true   -- If set to true, the tracers will come from the bottom of your screen.

        _G.TracersVisible = true   -- If set to true then the tracers will be visible and vice versa.
        _G.TracerColor = Color3.fromRGB(255, 255, 255)   -- The color that the tracers would appear as.
        _G.TracerThickness = 3   -- The thickness of the tracers.
        _G.TracerTransparency = 0.7   -- The transparency of the tracers.

        _G.ModeSkipKey = Enum.KeyCode.E   -- The key that changes between modes that indicate where will the tracers come from.
        _G.DisableKey = Enum.KeyCode.Q   -- The key that disables / enables the tracers.


        local function CreateTracers()
            for _, v in next, Players:GetPlayers() do
                if v.Name ~= game.Players.LocalPlayer.Name then
                    local TracerLine = Drawing.new("Line")
            
                    RunService.RenderStepped:Connect(function()
                        pcall(function()
                            if game.Workspace:FindFirstChild(v.Name) ~= nil and game.Workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil and (v.Character.Humanoid.Health == 0) then
                                local HumanoidRootPart_Position, HumanoidRootPart_Size = game.Workspace[v.Name].HumanoidRootPart.CFrame, game.Workspace[v.Name].HumanoidRootPart.Size * 1
                                local Vector, OnScreen = Camera:WorldToViewportPoint(HumanoidRootPart_Position * CFrame.new(0, -HumanoidRootPart_Size.Y, 0).p)
                                
                                TracerLine.Thickness = _G.TracerThickness
                                TracerLine.Transparency = _G.TracerTransparency
                                TracerLine.Color = _G.TracerColor

                                TracerLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)

                                if OnScreen == true then
                                    TracerLine.To = Vector2.new(Vector.X, Vector.Y)
                                    
                                    TracerLine.Visible = _G.TracersVisible
                                else
                                    TracerLine.Visible = false
                                end
                            else
                                TracerLine.Visible = false
                            end
                        end)
                    end)

                    Players.PlayerRemoving:Connect(function()
                        TracerLine.Visible = false
                    end)
                end
            end

            Players.PlayerAdded:Connect(function(Player)
                Player.CharacterAdded:Connect(function(v)
                    if v.Name ~= game.Players.LocalPlayer.Name then
                        local TracerLine = Drawing.new("Line")
                
                        RunService.RenderStepped:Connect(function()
                            pcall(function()
                                if workspace:FindFirstChild(v.Name) ~= nil and workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil and (v.Character.Humanoid.Health == 0) then
                                    local HumanoidRootPart_Position, HumanoidRootPart_Size = workspace[v.Name].HumanoidRootPart.CFrame, workspace[v.Name].HumanoidRootPart.Size * 1
                                    local Vector, OnScreen = Camera:WorldToViewportPoint(HumanoidRootPart_Position * CFrame.new(0, -HumanoidRootPart_Size.Y, 0).p)
                                    
                                    TracerLine.Thickness = _G.TracerThickness
                                    TracerLine.Transparency = _G.TracerTransparency
                                    TracerLine.Color = _G.TracerColor

                                    TracerLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)

                                    if OnScreen == true  then
                                        TracerLine.To = Vector2.new(Vector.X, Vector.Y)
                                        
                                        TracerLine.Visible = _G.TracersVisible
                                    else
                                        TracerLine.Visible = false
                                    end
                                else
                                    TracerLine.Visible = false
                                end
                            end)
                        end)

                        Players.PlayerRemoving:Connect(function()
                            TracerLine.Visible = false
                        end)
                    end
                end)
            end)
        end


        UserInputService.TextBoxFocused:Connect(function()
            Typing = true
        end)

        UserInputService.TextBoxFocusReleased:Connect(function()
            Typing = false
        end)



        UserInputService.InputBegan:Connect(function(Input)
        pcall(function()
            if (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == _G.DisableKey and Typing == false then
                    _G.TracersVisible = not _G.TracersVisible
                    
                    if _G.SendNotifications == true then
                        
                        if _G.TracersVisible == true then
                            text_to_send = "ESP Is Enabled"
                        else
                            text_to_send = "ESP Is Disabled"
                        end
                        

                        game:GetService("StarterGui"):SetCore("SendNotification",{
                            Title = "[ESP Cheat]";
                            Text = text_to_send;
                            Duration = 5;
                        })
                    end
                end
            end)
        end)

        local Success, Errored = pcall(function()
            CreateTracers()
        end)

        if Success and not Errored then
            if _G.SendNotifications == true then
                game:GetService("StarterGui"):SetCore("SendNotification",{
                    Title = "[ESP Cheat]";
                    Text = "ESP Loaded";
                    Duration = 5;
                })
            end

        elseif Errored and not Success then
            if _G.SendNotifications == true then
                game:GetService("StarterGui"):SetCore("SendNotification",{
                    Title = "[ESP Cheat]";
                    Text = "ESP Had An Error While Loading";
                    Duration = 5;
                })
            end
        end


    elseif TracersCheatOn == true then
        TracersCheatOn = false

        _G.TracersVisible = false   -- If set to true then the tracers will be visible and vice versa.
        --_G.TracerColor = Color3.fromRGB(255, 255, 255)   -- The color that the tracers would appear as.
        _G.TracerThickness = 0   -- The thickness of the tracers.
        _G.TracerTransparency = 1 

        Tracers.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Tracers.TextColor3 = Color3.fromRGB(0, 0, 0) 

        TracersCheatOnSave = false

        --Off:Play() 
        
    end
end)

TracersKeyBind = Enum.KeyCode.Escape.Value

Tracers.MouseButton2Down:connect(function()
    print('clicked')
    Tracers.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                TracersKeyBind = Enum.KeyCode.Escape.Value
            else 
                TracersKeyBind = input.KeyCode.Value
            end
            selecting = false
            Tracers.Text = "Tracers"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == TracersKeyBind then
        for i,v in pairs(getconnections(Tracers.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

CameraNoClip.Name = "CameraNoClip"
CameraNoClip.Parent = Render
CameraNoClip.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
CameraNoClip.BorderColor3 = Color3.fromRGB(0, 0, 0)
CameraNoClip.BorderSizePixel = 0
CameraNoClip.Position = UDim2.new(0, 0, 0.639175296, 0)
CameraNoClip.Size = UDim2.new(0, 204, 0, 31)
CameraNoClip.Font = Enum.Font.Roboto
CameraNoClip.Text = "CameraNoClip"
CameraNoClip.TextColor3 = Color3.fromRGB(0, 0, 0)
CameraNoClip.TextSize = 25.000
CameraNoClip.MouseButton1Down:connect(function()
    if not CameraNoClipCheatOn == true then
        CameraNoClipCheatOn = true

        CameraNoClip.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        CameraNoClip.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play() 

        lplr.DevCameraOcclusionMode = "Invisicam"

    elseif CameraNoClipCheatOn == true then
        CameraNoClipCheatOn = false
        
        CameraNoClip.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        CameraNoClip.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()

        lplr.DevCameraOcclusionMode = "Zoom"
        
    end
end)

CameraNoClipKeyBind = Enum.KeyCode.Escape.Value

CameraNoClip.MouseButton2Down:connect(function()
    print('clicked')
    CameraNoClip.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                CameraNoClipKeyBind = Enum.KeyCode.Escape.Value
            else 
                CameraNoClipKeyBind = input.KeyCode.Value
            end
            selecting = false
            CameraNoClip.Text = "CameraNoClip"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if input.KeyCode == CameraNoClipKeyBind then
        for i,v in pairs(getconnections(CameraNoClip.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)


local connection
local old

--CREDITS TO VAPEV4 FOR THE FOV
FOV.Name = "FOV"
FOV.Parent = Render
FOV.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
FOV.BorderColor3 = Color3.fromRGB(0, 0, 0)
FOV.BorderSizePixel = 0
FOV.Position = UDim2.new(0, 0, 0.426116824, 0)
FOV.Size = UDim2.new(0, 204, 0, 31)
FOV.Font = Enum.Font.Roboto
FOV.Text = "FOV"
FOV.TextColor3 = Color3.fromRGB(0, 0, 0)
FOV.TextSize = 25.000
FOV.MouseButton1Down:connect(function()
    if not FOVCheatOn == true then
        FOVCheatOn = true
        
        FOV.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        FOV.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play()  

        FOV.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        FOV.TextColor3 = Color3.fromRGB(255, 255, 255)
        
        old = old or cam.FieldOfView
        cam.FieldOfView = 120
        connection = cam:GetPropertyChangedSignal("FieldOfView"):Connect(function() 
            cam.FieldOfView = 120
        end) 

    elseif FOVCheatOn == true then
        FOVCheatOn = false
        if connection then 
            connection:Disconnect() 
        end
        cam.FieldOfView = old
        
        FOV.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        FOV.TextColor3 = Color3.fromRGB(0, 0, 0)  

        FOVCheatOnSave = false

        --Off:Play()

        --getgenv().speedval = {["Value"] = 20}
        
    end
end)

FOVKeyBind = Enum.KeyCode.Escape.Value

FOV.MouseButton2Down:connect(function()
    print('clicked')
    FOV.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                FOVKeyBind = Enum.KeyCode.Escape.Value
            else 
                FOVKeyBind = input.KeyCode.Value
            end
            selecting = false
            FOV.Text = "FOV"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == FOVKeyBind then
        for i,v in pairs(getconnections(FOV.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)

game.Players.PlayerAdded:Connect(function(Player)
    Player.CharacterAdded:Connect(function(v)
        if ChamsCheatOn == true then
            local champart = Instance.new("Highlight")
            champart.Name = "ChamPartEZ"
            champart.Enabled = true
            champart.FillColor = Color3.fromRGB(255,0,0)
            champart.Parent=v
            champart.OutlineTransparency = 1
        end
    end)
end)  


Chams.Name = "Chams"
Chams.Parent = Render
Chams.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
Chams.BorderColor3 = Color3.fromRGB(0, 0, 0)
Chams.BorderSizePixel = 0
Chams.Position = UDim2.new(0, 0, 0.53264606, 0)
Chams.Size = UDim2.new(0, 204, 0, 31)
Chams.Font = Enum.Font.Roboto
Chams.Text = "Chams"
Chams.TextColor3 = Color3.fromRGB(0, 0, 0)
Chams.TextSize = 25.000
Chams.MouseButton1Down:connect(function()
    if not ChamsCheatOn == true then
        ChamsCheatOn = true  

        Chams.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        Chams.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play() 

        game.StarterGui:SetCore("SendNotification", {
            Title = "Chams";
            Text = "Some Players Might Not Be Highlighted Because Roblox Set A Limit.";
            Icon = "";
            Duration = 5;
        })

        for _, v in next, game.Players:GetPlayers() do
            if v ~= game.Players.LocalPlayer then 
                if not (v.Character == nil) then
                    local champart = Instance.new("Highlight")
                    champart.Enabled = true
                    if v.Team ~= game.Players.LocalPlayer.Team then
                        champart.FillColor = Color3.fromRGB(255,0,0)
                    else 
                        champart.FillColor = Color3.fromRGB(0,255,0)
                    end
                    champart.Parent=v.Character
                    champart.OutlineTransparency = 1
                end
            end
        end

    elseif ChamsCheatOn == true then
        ChamsCheatOn = false
        
        Chams.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        Chams.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()
        for _, v in next, game.Players:GetPlayers() do
            if v ~= game.Players.LocalPlayer then 
                if not (v.Character == nil) then
                    if v.Character.Highlight ~= nil then 
                        v.Character.Highlight:Destroy()
                    end
                end
            end
        end
    end
end)

ChamsKeyBind = Enum.KeyCode.Escape.Value

Chams.MouseButton2Down:connect(function()
    print('clicked')
    Chams.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                ChamsKeyBind = Enum.KeyCode.Escape.Value
            else 
                ChamsKeyBind = input.KeyCode.Value
            end
            selecting = false
            Chams.Text = "Chams"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == ChamsKeyBind then
        for i,v in pairs(getconnections(Chams.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)


EmeraldESP.Name = "EmeraldESP"
EmeraldESP.Parent = Render
EmeraldESP.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
EmeraldESP.BorderColor3 = Color3.fromRGB(0, 0, 0)
EmeraldESP.BorderSizePixel = 0
EmeraldESP.Position = UDim2.new(0, 0, 0.745704532, 0)
EmeraldESP.Size = UDim2.new(0, 204, 0, 31)
EmeraldESP.Font = Enum.Font.Roboto
EmeraldESP.Text = "EmeraldESP"
EmeraldESP.TextColor3 = Color3.fromRGB(0, 0, 0)
EmeraldESP.TextSize = 25.000
EmeraldESP.MouseButton1Down:connect(function()
    if not EmeraldESPCheatOn == true then
        EmeraldESPCheatOn = true  

        EmeraldESP.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
        EmeraldESP.TextColor3 = Color3.fromRGB(255, 255, 255)  

        --On:Play() 

        -- EMERALD CHAMS ONLY WORKS WITH SKYWARS
        coroutine.wrap(function()
            while EmeraldESPCheatOn do
                for i,v in pairs(game:GetService("CollectionService"):GetTagged("chest")) do
                    local chest = v.ChestFolderValue.Value
                    local chestitems = chest and chest:GetChildren() or {}

                    
                    if not (v.Model:FindFirstChild("Highlight") == nil) then
                        v.Model.Highlight:Destroy()
                    end

                    for index, data in ipairs(chestitems) do
                        if string.find(tostring(data), "emerald") then
                            local a = Instance.new("Highlight")
                            a.Enabled = true
                            a.FillColor = Color3.fromRGB(57,255,20)
                            a.Parent=v.Model
                        end
                    end
                end
                wait(.1)
            end
        end)()

    elseif EmeraldESPCheatOn == true then
        EmeraldESPCheatOn = false
        
        EmeraldESP.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
        EmeraldESP.TextColor3 = Color3.fromRGB(0, 0, 0)  

        --Off:Play()

        deletingemeraldchests = true
        
        coroutine.wrap(function()
            while deletingemeraldchests do
                for i,v in pairs(game:GetService("CollectionService"):GetTagged("chest")) do
                    local chest = v.ChestFolderValue.Value
                    local chestitems = chest and chest:GetChildren() or {}

                    
                    if not (v.Model:FindFirstChild("Highlight") == nil) then
                        v.Model.Highlight:Destroy()
                    end

                    pcall(function()
                        if v.Model.Highlight ~= nil then
                            v.Model.Highlight:Destroy()
                        end
                    end)
            
                end
                Heartbeat:wait()
            end
        end)()

        wait(0.3)

        deletingemeraldchests = false
    end
end)


EmeraldESPKeyBind = Enum.KeyCode.Escape.Value

EmeraldESP.MouseButton2Down:connect(function()
    print('clicked')
    EmeraldESP.Text = "Select a Keybind"
    local selecting = true

    wait()

    UIS.InputBegan:Connect(function(input)
        if selecting == true then
            wait()
            print(input)
            if input.KeyCode == Enum.KeyCode.Escape or input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.MouseButton2 then
                EmeraldESPKeyBind = Enum.KeyCode.Escape.Value
            else 
                EmeraldESPKeyBind = input.KeyCode.Value
            end
            selecting = false
            EmeraldESP.Text = "EmeraldESP"
        end
    end)
end)

UIS.InputBegan:Connect(function(input)
    if  (input.KeyCode ~= Enum.KeyCode.Escape) and input.KeyCode.Value == EmeraldESPKeyBind then
        for i,v in pairs(getconnections(EmeraldESP.MouseButton1Down)) do
            v:Fire() 
        end  
    end
end)


coroutine.wrap(function()
    -- PlayerFace.LocalScript is disabled.
    local function QETGD_fake_script() -- HealthBar.LocalScript 
    end

print(2)
coroutine.wrap(QETGD_fake_script)()
-- MainGui.LocalScript is disabled.

end)()
print(3)

local shiftKeyL = Enum.KeyCode.LeftShift
local shiftKeyR = Enum.KeyCode.RightShift

local function toggleGUI(inputObject, gameProcessed)
	if (inputObject.KeyCode == shiftKeyR) then
		if game.CoreGui.Sigma_1.Enabled == true then
			game.CoreGui.Sigma_1.Enabled = false
		else
			game.CoreGui.Sigma_1.Enabled = true
		end
	end
end

UIS.InputBegan:connect(toggleGUI)


coroutine.wrap(function()
    repeat wait() until game:IsLoaded()
    local filename = "roblox_bedwars_cheats_settings_save.txt"
    local loadedCheats = false

    function saveSettings()
        settingToSave = {}
        settingToSave.settingsTable = {
            NoFallCheatOn,
            AutoSprintCheatOn,
            SpinBotCheatOn,
            FastDropCheatOn,
            AutoToxicCheatOn,
            AutoSpamCheatOn,
            CapeCheatOn,
            KillAuraCheatOn,
            ReachCheatOn,
            AntiKnockBackCheatOn,
            AutoClickerCheatOn,
            SpeedCheatOn,
            BhopCheatOn,
            LongJumpCheatOn,
            HighJumpCheatOn,
            PhaseCheatOn,
            BigSwordCheatOn,
            ChestStealerCheatOn,
            ScaffoldCheatOn,
            HUDCheatOn,
            ChillLofi1CheatOn,
            ChillLofi2CheatOn,
            ChillLofi3CheatOn,
            ChillLofi4CheatOn,
            ChillLofi5CheatOn,
            AntiVoidCheatOn,
            TimerCheatOn,
            FPSBoostCheatOn,
            FastFallCheatOn,
            ClickTPCheatOn,
            AntiCheatDisablerCheatOn,
            AutoQueueCheatOn,
            AntiAFKCheatOn,
            CopyDiscordLinkCheatOn,
            DinoFlyCheatOn,
            BlatantModeCheatOn,
            AutoSkywarsWinCheatOn,
            ESPCheatOn,
            TracersCheatOn,
            FOVCheatOn,
            ChamsCheatOn,
            CameraNoClipCheatOn,
            EmeraldESPCheatOn,
            NoClickDelayCheatOn,
            MultiAuraCheatOn,
        }
        settingToSave.KeyBinds = { 
            NoFallKeyBind,
            AutoSprintKeyBind,
            SpinBotKeyBind,
            FastDropKeyBind,
            AutoToxicKeyBind,
            AutoSpamKeyBind,
            CapeKeyBind,
            KillAuraKeyBind,
            ReachKeyBind,
            AntiKnockBackKeyBind,
            AutoClickerKeyBind,
            SpeedKeyBind,
            BhopKeyBind,
            FlyKeyBind,
            LongJumpKeyBind,
            HighJumpKeyBind,
            PhaseKeyBind,
            BigSwordKeyBind,
            ChestStealerKeyBind,
            ScaffoldKeyBind,
            HUDOnKeyBind,
            ChillLofi1KeyBind,
            ChillLofi2KeyBind,
            ChillLofi3KeyBind,
            ChillLofi4KeyBind,
            ChillLofi5KeyBind,
            AntiVoidKeyBind,
            TimerKeyBind,
            FPSBoostKeyBind,
            FastFallKeyBind,
            ClickTPKeyBind,
            AntiCheatDisablerKeyBind,
            AutoQueueKeyBind,
            AntiAFKKeyBind,
            CopyDiscordLinkKeyBind,
            DinoFlyKeyBind,
            BlatantModeKeyBind,
            AutoSkywarsWinKeyBind,
            ESPKeyBind,
            TracersKeyBind,
            FOVKeyBind,
            ChamsKeyBind,
            CameraNoClipKeyBind,
            EmeraldESPKeyBind,
            NoClickDelayKeyBind,
            MultiAuraKeyBind,
        }

        local json
        local HttpService = game:GetService("HttpService")
        if (writefile) then
            json = HttpService:JSONEncode(settingToSave.settingsTable)
            writefile(filename, json)
            json = HttpService:JSONEncode(settingToSave.KeyBinds)
            writefile("keybinds.txt", json)
        else
            print(" -- Sorry, Your Executor Failed To Save Your Settings, Please Use Another Executor Like KRNL Or SYNAPSE. Otherwise, This Is Not Much Of A Concern. -- ")
        end
    end


    function loadSettings()
        repeat wait() until game:IsLoaded()
        -- print('loading settings...')
        local HttpService = game:GetService("HttpService")
        if (readfile and isfile and isfile(filename)) then
            -- print()
            validFile1 = pcall(function() HttpService:JSONDecode(readfile(filename))end)
            validFile2= pcall(function() HttpService:JSONDecode(readfile("keybinds.txt"))end)

            if validFile1 and validFile2 then
                loadedValues = HttpService:JSONDecode(readfile(filename))
                keyBinds = HttpService:JSONDecode(readfile("keybinds.txt"))
                    
                if loadedCheats == false then
                    loadedCheats = true
                    NoFallCheatOn = loadedValues[1]
                    AutoSprintCheatOn = loadedValues[2]
                    SpinBotCheatOn = loadedValues[3]
                    FastDropCheatOn = loadedValues[4]
                    AutoToxicCheatOn = loadedValues[5]
                    AutoSpamCheatOn = loadedValues[6]
                    CapeCheatOn = loadedValues[7]
                    KillAuraCheatOn = loadedValues[8]
                    ReachCheatOn = loadedValues[9]
                    AntiKnockBackCheatOn = loadedValues[10]
                    AutoClickerCheatOn = loadedValues[11]
                    SpeedCheatOn = loadedValues[12]
                    BhopCheatOn = loadedValues[13]
                    LongJumpCheatOn = loadedValues[14]
                    HighJumpCheatOn = loadedValues[15]
                    PhaseCheatOn = loadedValues[16]
                    BigSwordCheatOn = loadedValues[17]
                    ChestStealerCheatOn = loadedValues[18]
                    ScaffoldCheatOn = loadedValues[19]
                    HUDCheatOn = loadedValues[20]
                    ChillLofi1CheatOn = loadedValues[21]
                    ChillLofi2CheatOn = loadedValues[22]
                    ChillLofi3CheatOn = loadedValues[23]
                    ChillLofi4CheatOn = loadedValues[24]
                    ChillLofi5CheatOn = loadedValues[25]
                    AntiVoidCheatOn = loadedValues[26]
                    TimerCheatOn = loadedValues[27]
                    FPSBoostCheatOn = loadedValues[28]
                    FastFallCheatOn = loadedValues[29]
                    ClickTPCheatOn = loadedValues[30]
                    AntiCheatDisablerCheatOn = loadedValues[31]
                    AutoQueueCheatOn = loadedValues[32]
                    AntiAFKCheatOn = loadedValues[33]
                    CopyDiscordLinkCheatOn = loadedValues[34]
                    DinoFlyCheatOn = loadedValues[35]
                    BlatantModeCheatOn = loadedValues[36]
                    AutoSkywarsWinCheatOn = loadedValues[37]
                    ESPCheatOn = loadedValues[38]
                    TracersCheatOn = loadedValues[39]
                    FOVCheatOn = loadedValues[40]
                    ChamsCheatOn = loadedValues[41]
                    CameraNoClipCheatOn = loadedValues[42]
                    EmeraldESPCheatOn = loadedValues[43]
                    NoClickDelayCheatOn = loadedValues[44]
                    MultiAuraCheatOn = loadedValues[45]

                                    
                    NoFallKeyBind = keyBinds[1]
                    AutoSprintKeyBind = keyBinds[2]
                    SpinBotKeyBind = keyBinds[3]
                    FastDropKeyBind = keyBinds[4]
                    AutoToxicKeyBind = keyBinds[5]
                    AutoSpamKeyBind = keyBinds[6]
                    CapeKeyBind = keyBinds[7]
                    KillAuraKeyBind = keyBinds[8]
                    ReachKeyBind = keyBinds[9]
                    AntiKnockBackKeyBind = keyBinds[10]
                    AutoClickerKeyBind = keyBinds[11]
                    SpeedKeyBind = keyBinds[12]
                    BhopKeyBind = keyBinds[13]
                    FlyKeyBind = keyBinds[14]
                    LongJumpKeyBind = keyBinds[15]
                    HighJumpKeyBind = keyBinds[16]
                    PhaseKeyBind = keyBinds[17]
                    BigSwordKeyBind = keyBinds[18]
                    ChestStealerKeyBind = keyBinds[19]
                    ScaffoldKeyBind = keyBinds[20]
                    HUDOnKeyBind = keyBinds[21]
                    ChillLofi1KeyBind = keyBinds[22]
                    ChillLofi2KeyBind = keyBinds[23]
                    ChillLofi3KeyBind = keyBinds[24]
                    ChillLofi4KeyBind = keyBinds[25]
                    ChillLofi5KeyBind = keyBinds[26]
                    AntiVoidKeyBind = keyBinds[27]
                    TimerKeyBind = keyBinds[28]
                    FPSBoostKeyBind = keyBinds[29]
                    FastFallKeyBind = keyBinds[30]
                    ClickTPKeyBind = keyBinds[31]
                    AntiCheatDisablerKeyBinds = keyBinds[32]
                    AutoQueueKeyBind = keyBinds[33]
                    AntiAFKKeyBind = keyBinds[34]
                    CopyDiscordLinkKeyBind = keyBinds[35]
                    DinoFlyKeyBind = keyBinds[36]
                    BlatantModeKeyBind = keyBinds[37]
                    AutoSkywarsWinKeyBind = keyBinds[38]
                    ESPKeyBind = keyBinds[39]
                    TracersKeyBind = keyBinds[40]
                    FOVKeyBind = keyBinds[41]
                    ChamsKeyBind = keyBinds[42]
                    CameraNoClipKeyBind = keyBinds[43]
                    EmeraldESPKeyBind = keyBinds[44]
                    NoClickDelayKeyBind = keyBinds[45]
                    MultiAuraKeyBind = keyBinds[46]


                    print(1)
                    repeat wait() until not (game.Players.LocalPlayer.Character == nil)
                    print(2)
                    repeat wait() until not (game.Players.LocalPlayer.Character.Humanoid == nil)
                    print(3)
                    repeat wait() until not (game.Players.LocalPlayer.Character.HumanoidRootPart == nil)
                
                    spawn(function()
                        if NoFallCheatOn then
                            -- wait(1)
                            spawn(function()
                                repeat task.wait(1) 
                                    game:GetService("ReplicatedStorage").rbxts_include.node_modules.net.out._NetManaged.GroundHit:FireServer(WORKSPACE.Map,999999999999999.00069)
                                until NoFallCheatOn == false
                            end)
                    

                            NoFall.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            NoFall.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)

                    spawn(function()
                        if AutoSprintCheatOn then
                            -- wait(1)
                            AutoSprint.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AutoSprint.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            spawn(function()
                                while AutoSprintCheatOn == true do
                                    bedwars["sprintTable"]:startSprinting()

                                    if AutoSprintCheatOn == false then
                                        break
                                    end
                                    wait()
                                end
                            end)
                        end
                    end)

                    spawn(function()
                        if SpinBotCheatOn then
                        end
                    end)

                    spawn(function()
                        if FastDropCheatOn then
                        end
                    end)

                    spawn(function()
                        if AutoToxicCheatOn then
                        end
                    end)

                    spawn(function()
                        if AutoSpamCheatOn then
                            AutoSpam.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AutoSpam.TextColor3 = Color3.fromRGB(255, 255, 255) 
                            spawn(function()
                                while AutoSpamCheatOn == true do
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Only Sigma Client But Roblox Users Don't Know What A Lagback Is", "All")
                                    wait(3)
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Sigma Client But Roblox > Every Single Roblox Bedwars Script", "All")
                                    wait(3)
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Have You Heard Of Lunar Client On Minecraft? Sigma But Roblox Is Pretty Much The Same Thing!!1", "All")
                                    wait(3)
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("If You're Tired Of Lagging Back Always Pull Up Sigma Client", "All")
                                    wait(3)
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                    game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Imagine Wasting 4 Years To Script Something And It's Still Bad, Not Me.", "All")
                                    wait(3)
                                    if AutoSpamCheatOn == false then
                                        break
                                    end
                                end
                            end)
                        end
                    end)

                    spawn(function()
                        if CapeCheatOn then

                            Cape.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Cape.TextColor3 = Color3.fromRGB(255, 255, 255)

                            if lplr.Character:FindFirstChild("Torso") then
                            torso = lplr.Character.Torso
                        else
                            torso = lplr.Character.UpperTorso
                        end
                        local p = Instance.new("Part", torso.Parent)
                        p.Name = "Cape"
                        p.Anchored = false
                        p.CanCollide = false
                        p.TopSurface = 0
                        p.BottomSurface = 0
                        p.Color = Color3.new(40, 166, 255)
                        p.Material = Enum.Material.Plastic
                        p.FormFactor = "Custom"
                        p.Size = Vector3.new(0.2,0.2,0.2)
                        local msh = Instance.new("BlockMesh", p)
                        msh.Scale = Vector3.new(9,17.5,0.5)
                        local motor = Instance.new("Motor", p)
                        motor.Part0 = p
                        motor.Part1 = torso
                        motor.MaxVelocity = 0.01
                        motor.C0 = CFrame.new(0,1.75,0) * CFrame.Angles(0,math.rad(90),0)
                        motor.C1 = CFrame.new(0,1,0.45) * CFrame.Angles(0,math.rad(90),0)
                        p.Color = Color3.fromRGB(40, 166, 255)
                        local wave = false
                        repeat wait(1/44)
                            local ang = 0.1
                            local oldmag = torso.Velocity.magnitude
                            local mv = 0.002
                            if wave then
                                ang = ang + ((torso.Velocity.magnitude/10) * 0.05) + 0.05
                                wave = false
                            else
                                wave = true
                            end
                            ang = ang + math.min(torso.Velocity.magnitude/11, 0.5)
                            motor.MaxVelocity = math.min((torso.Velocity.magnitude/111), 0.04) + mv
                            motor.DesiredAngle = -ang
                            if motor.CurrentAngle < -0.2 and motor.DesiredAngle > -0.2 then
                                motor.MaxVelocity = 0.04
                            end
                            repeat wait() until motor.CurrentAngle == motor.DesiredAngle or math.abs(torso.Velocity.magnitude - oldmag) >= (torso.Velocity.magnitude/10) + 1
                            if torso.Velocity.magnitude < 0.1 then
                                wait(0.1)
                            end
                            until not p or p.Parent ~= torso.Parent	
                        end
                    end)

                    spawn(function()
                        if KillAuraCheatOn then
                                        
                        KillAura.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
                        KillAura.TextColor3 = Color3.fromRGB(255, 0, 0)

                        local playerentity
                        task.spawn(function()
                            repeat task.wait() plrentity = bedwars["getEntityTable"].getLocalPlayerEntity() until playerentity ~= nil
                        end)
                        local targetedplayer
                        RunLoops:BindToHeartbeat("Killaura", 1, function()
                        pcall(function()
                            if not (game.Players.LocalPlayer.Character == nil) then
                                if not (game.Players.LocalPlayer.Character.Humanoid == nil) then
                            if game.Players.LocalPlayer.Character.Humanoid.Health > 1 then
                                local Root = game.Players.LocalPlayer.Character.HumanoidRootPart
                                if Root then
                                    local Neck = game.Players.LocalPlayer.Character.Head:FindFirstChild("Neck")
                                    local LowerTorso = Root.Parent and Root.Parent:FindFirstChild("LowerTorso")
                                    local RootC0 = LowerTorso and LowerTorso:FindFirstChild("Root")
                                    if Neck and RootC0 then
                                        if orig == nil then
                                            orig = Neck.C0.p
                                        end
                                        if orig2 == nil then
                                            orig2 = RootC0.C0.p
                                        end
                                        if orig2 then
                                            if attacking ~= nil then 
                                                local targetPos = attacking.Character.HumanoidRootPart.Position + Vector3.new(0, 2, 0)
                                                local direction = (Vector3.new(targetPos.X, targetPos.Y, targetPos.Z) - game.Players.LocalPlayer.Character.Head.Position).Unit
                                                local direction2 = (Vector3.new(targetPos.X, Root.Position.Y, targetPos.Z) - Root.Position).Unit
                                                local lookCFrame = (CFrame.new(Vector3.zero, (Root.CFrame):VectorToObjectSpace(direction)))
                                                local lookCFrame2 = (CFrame.new(Vector3.zero, (Root.CFrame):VectorToObjectSpace(direction2)))
                                                Neck.C0 = CFrame.new(orig) * CFrame.Angles(lookCFrame.LookVector.Unit.y, 0, 0)
                                                RootC0.C0 = lookCFrame2 + orig2
                                            else
                                                Neck.C0 = CFrame.new(orig)
                                                RootC0.C0 = CFrame.new(orig2)
                                            end
                                        end
                                    end
                                end
                            end
                            end
                            end
                        end)
                    end)

                    --task.spawn(function()
                        repeat
                            --pcall(function()
                                task.wait(0.03)
                                if KillAuraCheatOn then


                                    local plrs = GetAllNearestHumanoidToPosition(18 - 0.0001)

                                    if plrs[1] == nil then
                                        attacking = nil
                                    end

                                    local attackedplayers = {}
                                    local firstplayercodedone = {done = false}
                                    for i,plr in pairs(plrs) do
                                        attacking = plr
                                        task.spawn(newAttackEntity, plr, firstplayercodedone, attackedplayers)
                                    end

                                end
                        until KillAuraCheatOn == false
                        end
                    end)

                    spawn(function()
                        if AntiKnockBackCheatOn then
                            print("COLOR")
                            -- wait(1)
                            AntiKnockBack.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AntiKnockBack.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            bedwars["KnockbackTable"]["kbDirectionStrength"] = 0
                            bedwars["KnockbackTable"]["kbUpwardStrength"] = 0

                            bedwars["VelocityUtil"].applyVelocity = function(...) end

                            bedwars["VelocityUtil"].applyVelocity = 0
                            bedwars["KnockbackTable"]["kbDirectionStrength"] = 0
                            bedwars["KnockbackTable"]["kbUpwardStrength"] = 0
                        end
                    end)

                    spawn(function()
                        if AutoClickerCheatOn then
                            -- wait(1)
                            AutoClicker.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AutoClicker.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            local UIS = game:GetService("UserInputService")
                            local held = false

                            UIS.InputBegan:Connect(function(input)
                                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                                    held = true
                                    while held == true do
                                        bedwars["SwordController"]:swingSwordAtMouse()
                                        game:GetService("RunService").RenderStepped:Wait()
                                    end
                                end
                            end)

                            UIS.InputEnded:Connect(function(input)
                                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                                    held = false
                                end
                            end)
                        end
                    end)

                    spawn(function()
                        if SpeedCheatOn then

                            Speed.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Speed.TextColor3 = Color3.fromRGB(255, 255, 255)  


                            game.StarterGui:SetCore("SendNotification", {
                                Title = "Speed";
                                Text = "Speed and/or Bhop goes best with a speed potion.";
                                Icon = "";
                                Duration = 5;
                            }) 

                            spawn(function()
                                while SpeedCheatOn == true do
                                    if SpeedCheatOn == true and BlatantModeCheatOn == false then
                                        getgenv().speedvalforspeed = {["Value"] = 35} --50
                                        wait(0.7)
                                    elseif SpeedCheatOn == true and BlatantModeCheatOn == true then
                                        getgenv().speedvalforspeed = {["Value"] = 250} --200
                                        wait()
                                    end
                                    if SpeedCheatOn == true and BlatantModeCheatOn == false then
                                        getgenv().speedvalforspeed = {["Value"] = 90} --70
                                        wait(0.2) --0.4
                                    elseif SpeedCheatOn == true and BlatantModeCheatOn == true then
                                        getgenv().speedvalforspeed = {["Value"] = 250} --200
                                        wait()
                                    end
                                end
                            end)
--]]
                            coroutine.wrap(function() 
                                repeat wait() until game:IsLoaded()

                                local UIS = game:GetService("UserInputService")
                                local TS = game:GetService("TweenService")
                                local WORKSPACE = game:GetService("Workspace")
                                local PLAYERS = game:GetService("Players")
                                local lplr = PLAYERS.LocalPlayer
                                local mouse = lplr:GetMouse()
                                local cam = WORKSPACE.CurrentCamera
                                local requestfunc = syn and syn.request or http and http.request or http_request or fluxus and fluxus.request or getgenv().request or request
                                local queueteleport = syn and syn.queue_on_teleport or queue_on_teleport or fluxus and fluxus.queue_on_teleport


                                stopSpeed = false

                                local speedsettings = {
                                    factor = 5.37,  
                                    velocitydivfactor = 2.9,
                                    wsvalue = 22.5
                                }

                                


                                BindToStepped("Speed", function(time, dt)
                                    if FLYINPUTVALUE == false then
                                        if SpeedCheatOn == true then
                                            if isAlive() and not stopSpeed or AntiCheatDisablerCheatOn == true then
                                                spawn(function()
                                                    wait(0.1)
                                                    lplr.Character.Humanoid.WalkSpeed = speedsettings.wsvalue
                                                    local velo = lplr.Character.Humanoid.MoveDirection * (getgenv().speedvalforspeed["Value"]*((isnetworkowner and isnetworkowner(lplr.Character.HumanoidRootPart)) and speedsettings.factor or 0)) * dt
                                                    velo = Vector3.new(velo.x / 45, 0, velo.z / 45) -- originally 11 and 11
                                                    lplr.Character:TranslateBy(velo)
                                                end)
                                                
                                                Heartbeat:wait()

                                                pcall(function()
                                                    local velo2 = (lplr.Character.Humanoid.MoveDirection * getgenv().speedvalforspeed["Value"]) / speedsettings.velocitydivfactor
                                                    lplr.Character.HumanoidRootPart.Velocity = Vector3.new(velo2.X, lplr.Character.HumanoidRootPart.Velocity.Y, velo2.Z)
                                                end)

                                            end
                                        end
                                            
                                    elseif FLYINPUTVALUE == true then end
                                end)
                            end)()
                        end
                    end)

                    spawn(function()
                        if BhopCheatOn then
                            Bhop.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Bhop.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            game.StarterGui:SetCore("SendNotification", {
                                Title = "Bhop";
                                Text = "Bhop and/or Speed goes best with a speed potion.";
                                Icon = "";
                                Duration = 5;
                            })    

                            coroutine.wrap(function()
                                while BhopCheatOn == true do
                                    if humanoid.MoveDirection.X > 0 or humanoid.MoveDirection.X < 0 or humanoid.MoveDirection.Z > 0 or humanoid.MoveDirection.Z < 0 or humanoid.MoveDirection.Y > 0 or humanoid.MoveDirection.Y < 0   then -- as you can see it's scuff since I didn't know what I was doing much back then
                                        game.Players.LocalPlayer.Character.Humanoid.Jump = true
                                        if FlyCheatOn == false and LongJumpCheatOn == false and HighJumpCheatOn == false and FastFallCheatOn == false then
                                            workspace.Gravity = 70
                                        end
                                    end
                                    wait()		
                                end
                            end)()
                        end
                    end)

                    spawn(function()
                        if FlyCheatOn then
                            -- wait(1)
                            Fly.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Fly.TextColor3 = Color3.fromRGB(255, 255, 255)
                        end
                    end)

                    spawn(function()
                        if LongJumpCheatOn then
                            LongJump.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            LongJump.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            workspace.Gravity = 30 -- 20
                            lplr.Character.HumanoidRootPart.Velocity = Vector3.new(0, 20, 0)
                            --game.Players.LocalPlayer.Character.Humanoid.Jump = true
                        end
                    end)

                    spawn(function()
                        if HighJumpCheatOn then
                            --MAKE IT SO IT SAVES KEYBINDS ONCE U ADD THEM
                        end
                    end)

                    spawn(function()
                        if PhaseCheatOn then
                            Phase.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Phase.TextColor3 = Color3.fromRGB(255, 255, 255) 
                            

                            BindToSteppedPhase("Phase", function()
                                if isAlive() then
                                    for i,v in next, lplr.Character:GetDescendants() do 
                                        if v:IsA("BasePart") and v.CanCollide then 
                                            cachedparts[v] = v
                                            v.CanCollide = false
                                        end
                                    end
                                end
                            end)
                        end
                    end)

                    spawn(function()
                        if BigSwordCheatOn then
                            --wait(1)
                            lplr = game.Players.LocalPlayer

                            BigSword.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            BigSword.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            while BigSwordCheatOn == true do
                                spawn(function()
                                    lplr.Character:WaitForChild('wood_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)  
                                end)

                                spawn(function()
                                    lplr.Character:WaitForChild('stone_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
                                end)

                                spawn(function()
                                    lplr.Character:WaitForChild('iron_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
                                end)

                                spawn(function()
                                    lplr.Character:WaitForChild('diamond_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
                                end)

                                spawn(function()
                                    lplr.Character:WaitForChild('emerald_sword').Handle.Size = Vector3.new(17.6064, 53.9858, 5.80578)
                                end)
                                wait()
                            end
                        end
                    end)

                    spawn(function()
                        if ChestStealerCheatOn then
                                --wait(1)
                                ChestStealer.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                ChestStealer.TextColor3 = Color3.fromRGB(255, 255, 255) 

                                while ChestStealerCheatOn == true do
                                    local ChestStealerDistance = {["Value"] = 18}
                                    local ChestStealDelay = wait()
                                        -- print('EEE1')
                                            if isAlive() and ChestStealerCheatOn == true then
                                                local rootpart = lplr.Character and lplr.Character:FindFirstChild("HumanoidRootPart")
                                                for i,v in pairs(game:GetService("CollectionService"):GetTagged("chest")) do
                                                    if rootpart and (rootpart.Position - v.Position).magnitude <= ChestStealerDistance["Value"] and v:FindFirstChild("ChestFolderValue") then
                                                        local chest = v.ChestFolderValue.Value
                                                        local chestitems = chest and chest:GetChildren() or {}
                                                        --print('EEE3')
                                                        if #chestitems > 0 then
                                                            bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("SetObservedChest"):SendToServer(chest)
                                                            for i3,v3 in pairs(chestitems) do
                                                                if v3:IsA("Accessory") then
                                                                    spawn(function()
                                                                        bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("ChestGetItem"):CallServer(v.ChestFolderValue.Value, v3)
                                                                    end)
                                                                end
                                                            end
                                                            bedwars["ClientHandler"]:GetNamespace("Inventory"):Get("SetObservedChest"):SendToServer(nil)
                                                        end
                                                    end
                                                end
                                            end
                                            Heartbeat:wait()
                                        end
                        end
                    end)

                    spawn(function()
                        if ScaffoldCheatOn then
                            --wait(1)
                            Scaffold.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Scaffold.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            if game.PlaceId ~= 6872265039 then
                                local InventoryUtil = require(game:GetService("ReplicatedStorage").TS.inventory["inventory-util"]).InventoryUtil

                                local bedwars = {
                                    ["BlockController"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["block-engine"].out).BlockEngine,
                                    ["BlockController2"] = require(game:GetService("ReplicatedStorage")["rbxts_include"]["node_modules"]["@easy-games"]["block-engine"].out.client.placement["block-placer"]).BlockPlacer,
                                    ["BlockEngine"] = require(lplr.PlayerScripts.TS.lib["block-engine"]["client-block-engine"]).ClientBlockEngine,
                                    ["getInventory"] = function(plr)
                                        local plr = plr or lplr
                                        local suc, result = pcall(function() return InventoryUtil.getInventory(plr) end)
                                        return (suc and result or {
                                            ["items"] = {},
                                            ["armor"] = {},
                                            ["hand"] = nil
                                        })
                                    end,
                                }


                                local function getblockitem() 
                                    for i5, v5 in pairs(bedwars.getInventory(lplr).items) do
                                        if v5.itemType:match("wool") or v5.itemType:match("grass") or v5.itemType:match("stone_brick") or v5.itemType:match("wood_plank") or v5.itemType:match("stone") or v5.itemType:match("bedrock") then
                                            return v5.itemType, v5.amount
                                        end
                                    end	
                                    return nil
                                end

                                local function get3Vector(p) 
                                    local x,y,z = p.X, p.Y,p.Z 
                                    x = math.floor((x) + 0.5)
                                    y = math.floor((y) + 0.5)
                                    z = math.floor((z) + 0.5)
                                    return Vector3.new(x,y,z)
                                end

                                local function isPointInMapOccupied(p)
                                    local region = Region3.new(p - Vector3.new(1, 1, 1), p + Vector3.new(1, 1, 1))
                                    local x = workspace:FindPartsInRegion3WithWhiteList(region, game:GetService("CollectionService"):GetTagged("block"))
                                    return (#x == 0)
                                end

                                local function getwool()
                                    for i5, v5 in pairs(bedwars["getInventory"](lplr)["items"]) do
                                        if v5.itemType:match("wool") then
                                            return v5.itemType, v5.amount
                                        end
                                    end	
                                    return nil
                                end

                                local function getItem(itemName)
                                    for i5, v5 in pairs(bedwars["getInventory"](lplr)["items"]) do
                                        if v5.itemType == itemName then
                                            return v5, i5
                                        end
                                    end
                                    return nil
                                end


                                local blocktable = bedwars["BlockController2"].new(bedwars["BlockEngine"], getwool())
                                bedwars["placeBlock"] = function(newpos, customblock)
                                    local placeblocktype = (customblock or getwool())
                                    blocktable.blockType = placeblocktype
                                    if bedwars["BlockController"]:isAllowedPlacement(lplr, placeblocktype, Vector3.new(newpos.X/3, newpos.Y/3, newpos.Z/3)) and getItem(placeblocktype) then
                                        spawn(function()
                                            return blocktable:placeBlock(Vector3.new(newpos.X/3, newpos.Y/3, newpos.Z/3))
                                        end)
                                    end
                                end
                                
                                BindToStepped("Scaffold", function()
                                    if ScaffoldCheatOn == true then
                                        if isAlive() and lplr.Character:FindFirstChild("Humanoid") ~= nil then
                                            local block = getblockitem()
                                            --printtable(block)
                                            local newpos = lplr.Character.HumanoidRootPart.Position
                                            newpos = get3Vector( Vector3.new(newpos.X, lplr.Character.HumanoidRootPart.Position.Y - 4, newpos.Z) )
                                            local movedir = lplr.Character:FindFirstChild("Humanoid").MoveDirection
                                            if movedir.X==0 and movedir.Z==0 and lplr.Character:FindFirstChild("Humanoid").Jump==true  then 
                                                local velo = lplr.Character.HumanoidRootPart.Velocity
                                                lplr.Character.HumanoidRootPart.Velocity = Vector3.new(0, 25, 0) -- the y is normally 25
                                            end
                                            if not isPointInMapOccupied(newpos) then
                                                spawn(function()
                                                    bedwars["placeBlock"](newpos, block)
                                                end)
                                            end

                                            local expandpos = lplr.Character.HumanoidRootPart.Position + ((lplr.Character.Humanoid.MoveDirection.Unit))
                                            expandpos = get3Vector( Vector3.new(expandpos.X, lplr.Character.HumanoidRootPart.Position.Y-4, expandpos.Z) )
                                            if not isPointInMapOccupied(expandpos) then
                                                spawn(function()
                                                    bedwars["placeBlock"](expandpos)
                                                end)
                                            end

                                            local expandpos2 = lplr.Character.HumanoidRootPart.Position + ((lplr.Character.Humanoid.MoveDirection.Unit*2))
                                            expandpos2 = get3Vector( Vector3.new(expandpos2.X, lplr.Character.HumanoidRootPart.Position.Y-4, expandpos2.Z) )
                                            if not isPointInMapOccupied(expandpos2) then
                                                spawn(function()
                                                    bedwars["placeBlock"](expandpos2)
                                                end)
                                            end
                                        end
                                    end
                                end)
                            end
                        end
                    end)

                    spawn(function()
                        if HUDCheatOn then
                        end
                    end)

                    spawn(function()
                        if ChillLofi1CheatOn then
                            Lofi1:Play()

                            ChillLofi1.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ChillLofi1.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)

                    spawn(function()
                        if ChillLofi2CheatOn then
                            Lofi2:Play()

                            ChillLofi2.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ChillLofi2.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)

                    spawn(function()
                        if ChillLofi3CheatOn then
                            Lofi3:Play()

                            ChillLofi3.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ChillLofi3.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)

                    spawn(function()
                        if ChillLofi4CheatOn then
                            Lofi4:Play()

                            ChillLofi4.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ChillLofi4.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)

                    spawn(function()
                        if ChillLofi5CheatOn then
                            Lofi5:Play()

                            ChillLofi5.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ChillLofi5.TextColor3 = Color3.fromRGB(255, 255, 255) 
                        end
                    end)
--[[
                    spawn(function()
                        if HypeBeatCheatOn then
                        end
                    end)
--]]
                    spawn(function()
                        if AntiVoidCheatOn then
                            AntiVoid.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AntiVoid.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            parts = {}

                            lowest = math.huge

                            for _, child in pairs(workspace:GetDescendants()) do
                                if child:IsA("BasePart") then
                                    if child.Position.Y < lowest and child.Position.Y > -1000 then
                                        lowest = child.Position.Y
                                    end
                                end
                            end

                            print(lowest)

                            local NewPart = Instance.new("Part")
                            NewPart.Name = "ANTIVOID"
                            NewPart.Position = Vector3.new(0,5,0) --Position of the part
                            NewPart.Anchored = true
                            NewPart.CanCollide = false
                            NewPart.Size = Vector3.new(3000,1,3000) 
                            NewPart.Color = Color3.fromRGB(255,0,0)
                            NewPart.Transparency = 0.6
                            NewPart.Parent = workspace

                            local character = game.Players.LocalPlayer.Character
                                
                            local humanoid = character:WaitForChild("Humanoid")

                            touching = false
                            NeedReset = false

                            local pos = {}
                
                            local timePassed = tick()

                            local function antiVoidFunction(newpart, limb)
                                if newpart.Name == 'ANTIVOID' then
                                    if tick() - timePassed >= 1 then
                                        timePassed = tick()
                                        touching = true


                                        spawn(function()
                                            for i = 1, 25 do
                                                task.wait(0.04)
                                                if lplr.Character and lplr.Character.Parent ~= nil and lplr.Character:FindFirstChild("Humanoid") and lplr.Character.Humanoid.Health >= 0 and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                                                    --if lplr.Character.Humanoid.Health <= 0 then repeat task.wait() until lplr.isAlive and lplr.Character.Humanoid.Health > 0 break end
                                                    lplr.Character.HumanoidRootPart.Velocity = vec3(lplr.Character.HumanoidRootPart.Velocity.X, i * 3, lplr.Character.HumanoidRootPart.Velocity.Z)
                                                end
                                            end
                                        end)
                                        
                                        pos = {}
                                        wait()
                                        touching = false
                                    end
                                end
                            end

                                                
                            game.Players.LocalPlayer.CharacterAdded:Connect(function()
                                local humanoid = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
                                humanoid.Touched:Connect(antiVoidFunction)
                            end)

                            humanoid.Touched:Connect(antiVoidFunction)


                            spawn(function()
                                --while AntiVoidCheatOn == true do
                                if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character.Parent ~= nil and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") and game.Players.LocalPlayer.Character.Humanoid.Health >= 0 and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") or AntiCheatDisablerCheatOn == true then
                                    xcframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.X
                                    if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                                        ycframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.Y
                                    end
                                    if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                                        Zcframe = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.Z
                                    end
                                    if game.Players.LocalPlayer.Character.Humanoid.Health >= 1 then
                                        NewPart.CFrame = CFrame.new(xcframe, 3, Zcframe) --5
                                    end
                                end
                                    --wait()
                                --end
                            end)
                        end
                    end)

                    spawn(function()
                        if TimerCheatOn then
                        end
                    end)

                    spawn(function()
                        if FPSBoostCheatOn then
                        end
                    end)

                    spawn(function()
                        if FastFallCheatOn then
                            FastFall.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            FastFall.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            spawn(function()
                                while FastFallCheatOn == true do
                                    if FastFallCheatOn == true and FlyCheatOn == false and Fly2CheatOn == false and LongJumpCheatOn == false and HighJumpCheatOn == false then
                                        workspace.Gravity = 400
                                        else
                                    end
                                    wait()
                                end
                            end)       
                        end
                    end)

                    spawn(function()
                        if ClickTPCheatOn then
                        end
                    end)

                    spawn(function()
                        if AntiCheatDisablerCheatOn then
                            AntiCheatDisablerCheatOn = true
                            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            --On:Play()

                            repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
                            if matchState == 1 then
                                game.StarterGui:SetCore("SendNotification", {
                                    Title = "AntiCheatDisabler";
                                    Text = "Anticheatdisabler can't be used once the game has started.";
                                    Icon = "";
                                    Duration = 3;
                                })
                                wait(0.5)
                                --AntiCheatDisablerCheatOn = false
                                AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                                AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
                        elseif matchState == 0 then
                                AntiCheatDisablerCheatOn = true
                                AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
                                
                                anticheatdisabler()
                            end
                        end
                    end)

                    spawn(function()
                        if AutoQueueCheatOn then
                            AutoQueue.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AutoQueue.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            AutoQueueCheatOn = true

                            while AutoQueueCheatOn == true do
                                local clientstorestate = bedwars["ClientStoreHandler"]:getState()
                                matchState = clientstorestate.Game.matchState or 0

                                if (matchState == 2) or (game.PlaceId == 6872265039) or (lplr.Character.HumanoidRootPart == nil) then
                                    game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                                    queuedup = true
                                end

                                if queuedup == true then
                                    queuedup = false
                                    break
                                end
                                wait()
                            end               
                        end
                    end)
                    
                    spawn(function()
                        if AntiAFKCheatOn then
                        end
                    end)

                    spawn(function()
                        if CopyDiscordLinkCheatOn then
                        end
                    end)

                    spawn(function()
                        if DinoFlyCheatOn then
                            DinoFly.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            DinoFly.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            DinoFlyCheatOn = true
                        end
                    end)

                    spawn(function()
                        if BlatantModeCheatOn then
                            BlatantModeCheatOn = true
                            BlatantMode.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            BlatantMode.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            --On:Play() 

                            wait(1)

                            while BlatantModeCheatOn == true do
                                local clientstorestate = bedwars["ClientStoreHandler"]:getState()
                                matchState = clientstorestate.Game.matchState or 0

                                if matchState == 1 and AntiCheatDisablerCheatOn == false or matchState == 0 and AntiCheatDisablerCheatOn == false then
                                    cantblatantmode = true
                                end

                                if cantblatantmode == true then
                                    cantblatantmode = false

                                    BlatantModeCheatOn = false
                                    BlatantMode.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                                    BlatantMode.TextColor3 = Color3.fromRGB(0, 0, 0)  

                                    game.StarterGui:SetCore("SendNotification", {
                                        Title = "BlatantMode";
                                        Text = "BlatantMode can't be enabled without anticheatdisabler.";
                                        Icon = "";
                                        Duration = 4;
                                    })

                                    break
                                end
                                wait()
                            end  
                        end
                    end)

                    spawn(function()
                        if AutoSkywarsWinCheatOn then
                            AutoSkywarsWinCheatOn = true
                            AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            AutoSkywarsWin.TextColor3 = Color3.fromRGB(255, 255, 255)

                            repeat wait() until matchState == 1

                            if matchState == 1 and AntiCheatDisablerCheatOn == false then  
                                game.StarterGui:SetCore("SendNotification", {
                                    Title = "AutoSkywarsWin";
                                    Text = "AutoSkywarsWin can only be started before the game begins.";
                                    Icon = "";
                                    Duration = 6;
                                })

                                AutoSkywarsWinCheatOn = false
                                AutoSkywarsWin.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                                AutoSkywarsWin.TextColor3 = Color3.fromRGB(0, 0, 0) 

                            elseif matchState == 0 and AntiCheatDisablerCheatOn == true or matchState == 0 and AntiCheatDisablerCheatOn == false or matchState == 1 and AntiCheatDisablerCheatOn == true then     
                                --On:Play() 

                                local humrp = lplr.Character.HumanoidRootPart

                                players = game.Players:GetChildren()

                                spawn(function()
                                    wait(70)
                                    if AutoSkywarsWinCheatOn then
                                        game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                                    end
                                end)

                        --------------------------------------------------------loading other cheats
                                spawn(function()
                                    if not MultiAuraCheatOn == true then
                                        MultiAuraCheatOn = true

                                        MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                        MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

                                        --On:Play()

                                        bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

                                        repeat
                                            task.wait(0.03)
                                            local plrs = GetAllNearestHumanoidToPosition(18.8)
                                            for i,plr in pairs(plrs) do
                                                local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                                                local newpos = plr.Character.HumanoidRootPart.Position
                                                bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
                                            end
                                        until MultiAuraCheatOn == false 
                                    end
                                end)

                                spawn(function()
                                    if not AntiCheatDisablerCheatOn == true then
                                        AntiCheatDisablerCheatOn = true
                                        AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                        AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255)  

                                        --On:Play()

                                        repeat task.wait() until character and character:FindFirstChild("HumanoidRootPart") and character:FindFirstChild("Humanoid") -- and character:FindFirstChild("Humanoid").Health >= 1
                                        if matchState == 1 then
                                            game.StarterGui:SetCore("SendNotification", {
                                                Title = "AntiCheatDisabler";
                                                Text = "Anticheatdisabler can't be used once the game has started.";
                                                Icon = "";
                                                Duration = 3;
                                            })
                                            wait(0.3)
                                            --AntiCheatDisablerCheatOn = false
                                            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(250, 250, 250)
                                            AntiCheatDisabler.TextColor3 = Color3.fromRGB(0, 0, 0)  
                                    elseif matchState == 0 then
                                            AntiCheatDisablerCheatOn = true
                                            AntiCheatDisabler.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                            AntiCheatDisabler.TextColor3 = Color3.fromRGB(255, 255, 255) 
                                            
                                            anticheatdisabler()
                                        end
                                    end
                                end)

                                spawn(function()
                                    if not AutoQueueCheatOn == true then
                                        AutoQueueCheatOn = true
                                        AutoQueue.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                                        AutoQueue.TextColor3 = Color3.fromRGB(255, 255, 255) 

                                        while AutoQueueCheatOn == true do
                                            local clientstorestate = bedwars["ClientStoreHandler"]:getState()
                                            matchState = clientstorestate.Game.matchState or 0

                                            if (matchState == 2) or (game.PlaceId == 6872265039) or (lplr.Character.HumanoidRootPart == nil) then
                                                game:GetService("ReplicatedStorage")["events-@easy-games/lobby:shared/event/lobby-events@getEvents.Events"].joinQueue:FireServer({["queueType"] = "skywars_to2"})
                                                queuedup = true
                                            end

                                            if queuedup == true then
                                                queuedup = false
                                                break
                                            end
                                            wait()
                                        end
                                    end
                                end)
                        --------------------------------------------------------loading other cheats
                                pcall(function()
                                    while AutoSkywarsWinCheatOn == true do
                                        for i,v in pairs(players) do
                                            if v.Character and AutoSkywarsWinCheatOn == true then
                                                if not (v.Name == lplr.Name) and not (v.Team.TeamColor == lplr.Team.TeamColor) and v.Character:FindFirstChild("Humanoid") and not (v.Character.Humanoid.Health <= 0) and AutoSkywarsWinCheatOn == true then
                                                    repeat wait(0.1)
                                                        if v.Character and (v.Character:FindFirstChild("Head")) and AutoSkywarsWinCheatOn == true then
                                                            character.HumanoidRootPart.CFrame = v.Character.HumanoidRootPart.CFrame
                                                            --character:SetPrimaryPartCFrame(character:GetPrimaryPartCFrame()*CFrame.new(0, 4, 0))
                                                        end
                                                    until v.Character.Humanoid.Health <= 0 or v.Character:FindFirstChild("HumanoidRootPart") == nil or AutoSkywarsWinCheatOn == false
                                                end
                                            end
                                            wait()
                                        end
                                        wait()
                                    end
                                end)
                            end    
                        end
                    end)

                    spawn(function()
                        if ESPCheatOn then

                            ESP.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            ESP.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            local Player = game:GetService("Players").LocalPlayer
                            local Camera = game:GetService("Workspace").CurrentCamera
                            local Mouse = Player:GetMouse()

                            local function Dist(pointA, pointB) -- magnitude errors for some reason : (
                                return math.sqrt(math.pow(pointA.X - pointB.X, 2) + math.pow(pointA.Y - pointB.Y, 2))
                            end

                            local function GetClosest(points, dest)
                                local min = math.huge
                                local closest = nil
                                for _, v in pairs(points) do
                                    local dist = Dist(v, dest)
                                    if dist < min then
                                        min = dist
                                        closest = v
                                    end
                                end
                                return closest
                            end

                            local function DrawESP(plr)
                                local Box = Drawing.new("Quad")
                                Box.Visible = false
                                Box.PointA = Vector2.new(0, 0)
                                Box.PointB = Vector2.new(0, 0)
                                Box.PointC = Vector2.new(0, 0)
                                Box.PointD = Vector2.new(0, 0)
                                if game.Players.LocalPlayer.Team == plr.Team then
                                    Box.Color = Color3.fromRGB(0, 255, 0)
                                else
                                    Box.Color = Color3.fromRGB(255, 0, 0)
                                end
                                Box.Thickness = 2
                                Box.Transparency = 1

                                local function Update()
                                    local c
                                    c =
                                        game:GetService("RunService").RenderStepped:Connect(
                                        function()
                                            if ESPCheatOn == false then
                                                Box.Visible = false
                                            else
                                                if
                                                    plr.Character ~= nil and plr.Character:FindFirstChildOfClass("Humanoid") ~= nil and
                                                        plr.Character:FindFirstChild("HumanoidRootPart") ~= nil and
                                                        plr.Character:FindFirstChildOfClass("Humanoid").Health > 0 and
                                                        plr.Character:FindFirstChild("Head") ~= nil
                                                then
                                                    local pos, vis = Camera:WorldToViewportPoint(plr.Character.HumanoidRootPart.Position)
                                                    if vis then
                                                        local points = {}
                                                        local c = 0
                                                        for _, v in pairs(plr.Character:GetChildren()) do
                                                            if v:IsA("BasePart") then
                                                                c = c + 1
                                                                local p = Camera:WorldToViewportPoint(v.Position)
                                                                if v.Name == "HumanoidRootPart" then
                                                                    p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(0, 0, -v.Size.Z)).p)
                                                                elseif v.Name == "Head" then
                                                                    p =
                                                                        Camera:WorldToViewportPoint(
                                                                        (v.CFrame * CFrame.new(0, v.Size.Y / 2, v.Size.Z / 1.25)).p
                                                                    )
                                                                elseif string.match(v.Name, "Left") then
                                                                    p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(-v.Size.X / 2, 0, 0)).p)
                                                                elseif string.match(v.Name, "Right") then
                                                                    p = Camera:WorldToViewportPoint((v.CFrame * CFrame.new(v.Size.X / 2, 0, 0)).p)
                                                                end
                                                                points[c] = p
                                                            end
                                                        end
                                                        local Left = GetClosest(points, Vector2.new(0, pos.Y))
                                                        local Right = GetClosest(points, Vector2.new(Camera.ViewportSize.X, pos.Y))
                                                        local Top = GetClosest(points, Vector2.new(pos.X, 0))
                                                        local Bottom = GetClosest(points, Vector2.new(pos.X, Camera.ViewportSize.Y))

                                                        if Left ~= nil and Right ~= nil and Top ~= nil and Bottom ~= nil then
                                                            Box.PointA = Vector2.new(Right.X, Top.Y)
                                                            Box.PointB = Vector2.new(Left.X, Top.Y)
                                                            Box.PointC = Vector2.new(Left.X, Bottom.Y)
                                                            Box.PointD = Vector2.new(Right.X, Bottom.Y)

                                                            Box.Visible = true
                                                        else
                                                            Box.Visible = false
                                                        end
                                                    else
                                                        Box.Visible = false
                                                    end
                                                else
                                                    Box.Visible = false
                                                    if game.Players:FindFirstChild(plr.Name) == nil then
                                                        c:Disconnect()
                                                    end
                                                end
                                            end
                                        end
                                    )
                                end
                                coroutine.wrap(Update)()

                            end

                            for _, v in pairs(game:GetService("Players"):GetChildren()) do
                                if v.Name ~= Player.Name then
                                    DrawESP(v)
                                end
                            end

                            game:GetService("Players").PlayerAdded:Connect(
                                function(v)
                                    DrawESP(v)
                                end
                            )
                        end
                    end)

                    spawn(function()
                        if TracersCheatOn then
                            --wait(1)
                            Tracers.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Tracers.TextColor3 = Color3.fromRGB(255, 255, 255)     


                            local function API_Check()
                                if Drawing == nil then
                                    return "No"
                                else
                                    return "Yes"
                                end
                            end

                            local Find_Required = API_Check()

                            if Find_Required == "No" then
                                game:GetService("StarterGui"):SetCore("SendNotification",{
                                    Title = "[ESP Cheat]";
                                    Text = "Your Executor Is Not Supported";
                                    Duration = math.huge;
                                    Button1 = "OK"
                                })

                                return
                            end



                            local RunService = game:GetService("RunService")
                            local Players = game:GetService("Players")
                            local Camera = game:GetService("Workspace").CurrentCamera
                            local UserInputService = game:GetService("UserInputService")
                            local TestService = game:GetService("TestService")

                            local Typing = false

                            _G.SendNotifications = false   -- If set to true then the script would notify you frequently on any changes applied and when loaded / errored. (If a game can detect this, it is recommended to set it to false)
                            _G.DefaultSettings = false   -- If set to true then the tracer script would run with default settings regardless of any changes you made.

                            _G.TeamCheck = false   -- If set to true then the script would create tracers only for the enemy team members.

                            --[!]-- ONLY ONE OF THESE VALUES SHOULD BE SET TO TRUE TO NOT ERROR THE SCRIPT --[!]--

                            _G.FromMouse = false   -- If set to true, the tracers will come from the position of your mouse curson on your screen.
                            _G.FromCenter = false   -- If set to true, the tracers will come from the center of your screen.
                            _G.FromBottom = true   -- If set to true, the tracers will come from the bottom of your screen.

                            _G.TracersVisible = true   -- If set to true then the tracers will be visible and vice versa.
                            _G.TracerColor = Color3.fromRGB(255, 255, 255)   -- The color that the tracers would appear as.
                            _G.TracerThickness = 3   -- The thickness of the tracers.
                            _G.TracerTransparency = 0.7   -- The transparency of the tracers.

                            _G.ModeSkipKey = Enum.KeyCode.E   -- The key that changes between modes that indicate where will the tracers come from.
                            _G.DisableKey = Enum.KeyCode.Q   -- The key that disables / enables the tracers.


                            local function CreateTracers()
                                for _, v in next, Players:GetPlayers() do
                                    if v.Name ~= game.Players.LocalPlayer.Name then
                                        local TracerLine = Drawing.new("Line")
                                
                                        RunService.RenderStepped:Connect(function()
                                            if game.Workspace:FindFirstChild(v.Name) ~= nil and game.Workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                                                local HumanoidRootPart_Position, HumanoidRootPart_Size = game.Workspace[v.Name].HumanoidRootPart.CFrame, game.Workspace[v.Name].HumanoidRootPart.Size * 1
                                                local Vector, OnScreen = Camera:WorldToViewportPoint(HumanoidRootPart_Position * CFrame.new(0, -HumanoidRootPart_Size.Y, 0).p)
                                                
                                                TracerLine.Thickness = _G.TracerThickness
                                                TracerLine.Transparency = _G.TracerTransparency
                                                TracerLine.Color = _G.TracerColor

                                                TracerLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)

                                                if OnScreen == true then
                                                    TracerLine.To = Vector2.new(Vector.X, Vector.Y)
                                                    
                                                    TracerLine.Visible = _G.TracersVisible
                                                else
                                                    TracerLine.Visible = false
                                                end
                                            else
                                                TracerLine.Visible = false
                                            end
                                        end)

                                        Players.PlayerRemoving:Connect(function()
                                            TracerLine.Visible = false
                                        end)
                                    end
                                end

                                Players.PlayerAdded:Connect(function(Player)
                                    Player.CharacterAdded:Connect(function(v)
                                        if v.Name ~= game.Players.LocalPlayer.Name then
                                            local TracerLine = Drawing.new("Line")
                                            RunService.RenderStepped:Connect(function()
                                                pcall(function()
                                                    if game.Workspace:FindFirstChild(v.Name) ~= nil and game.Workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil and (v.Character.Humanoid.Health == 0) then
                                                        local HumanoidRootPart_Position, HumanoidRootPart_Size = game.Workspace[v.Name].HumanoidRootPart.CFrame, game.Workspace[v.Name].HumanoidRootPart.Size * 1
                                                        local Vector, OnScreen = Camera:WorldToViewportPoint(HumanoidRootPart_Position * CFrame.new(0, -HumanoidRootPart_Size.Y, 0).p)
                                                        
                                                        TracerLine.Thickness = _G.TracerThickness
                                                        TracerLine.Transparency = _G.TracerTransparency
                                                        TracerLine.Color = _G.TracerColor

                                                        TracerLine.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)

                                                        if OnScreen == true then
                                                            TracerLine.To = Vector2.new(Vector.X, Vector.Y)
                                                            
                                                            TracerLine.Visible = _G.TracersVisible
                                                        else
                                                            TracerLine.Visible = false
                                                        end
                                                    else
                                                        TracerLine.Visible = false
                                                    end
                                                end)
                                            end)

                                            Players.PlayerRemoving:Connect(function()
                                                TracerLine.Visible = false
                                            end)
                                        end
                                    end)
                                end)
                            end


                            UserInputService.TextBoxFocused:Connect(function()
                                Typing = true
                            end)

                            UserInputService.TextBoxFocusReleased:Connect(function()
                                Typing = false
                            end)



                            UserInputService.InputBegan:Connect(function(Input)
                            pcall(function()
                                if Input.KeyCode == _G.DisableKey and Typing == false then
                                        _G.TracersVisible = not _G.TracersVisible
                                        
                                        if _G.SendNotifications == true then
                                            
                                            if _G.TracersVisible == true then
                                                text_to_send = "ESP Is Enabled"
                                            else
                                                text_to_send = "ESP Is Disabled"
                                            end
                                            

                                            game:GetService("StarterGui"):SetCore("SendNotification",{
                                                Title = "[ESP Cheat]";
                                                Text = text_to_send;
                                                Duration = 5;
                                            })
                                        end
                                    end
                                end)
                            end)

                            local Success, Errored = pcall(function()
                                CreateTracers()
                            end)

                            if Success and not Errored then
                                if _G.SendNotifications == true then
                                    game:GetService("StarterGui"):SetCore("SendNotification",{
                                        Title = "[ESP Cheat]";
                                        Text = "ESP Loaded";
                                        Duration = 5;
                                    })
                                end

                            elseif Errored and not Success then
                                if _G.SendNotifications == true then
                                    game:GetService("StarterGui"):SetCore("SendNotification",{
                                        Title = "[ESP Cheat]";
                                        Text = "ESP Had An Error While Loading";
                                        Duration = 5;
                                    })
                                end
                            end    
                        end
                    end)

                    spawn(function()
                        if FOVCheatOn then
                            FOV.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            FOV.TextColor3 = Color3.fromRGB(255, 255, 255)

                            old = old or cam.FieldOfView
                            cam.FieldOfView = 120
                            connection = cam:GetPropertyChangedSignal("FieldOfView"):Connect(function() 
                                cam.FieldOfView = 120
                            end)
                        end
                    end)

                    spawn(function()
                        if ChamsCheatOn then
                            Chams.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            Chams.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            game.StarterGui:SetCore("SendNotification", {
                                Title = "Chams";
                                Text = "Some Players Might Not Be Highlighted Because Roblox Set A Limit.";
                                Icon = "";
                                Duration = 5;
                            })

                            for _, v in next, game.Players:GetPlayers() do
                                if v ~= game.Players.LocalPlayer then 
                                    if not (v.Character == nil) then
                                        local champart = Instance.new("Highlight")
                                        champart.Enabled = true
                                        if v.Team ~= game.Players.LocalPlayer.Team then
                                            champart.FillColor = Color3.fromRGB(255,0,0)
                                        else 
                                            champart.FillColor = Color3.fromRGB(0,255,0)
                                        end
                                        champart.Parent=v.Character
                                        champart.OutlineTransparency = 1
                                    end
                                end
                            end
                        end
                    end)

                    spawn(function()
                        if CameraNoClipCheatOn then
                            CameraNoClip.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            CameraNoClip.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            lplr.DevCameraOcclusionMode = "Invisicam"
                        end
                    end)

                    spawn(function() 
                        if NoClickDelayCheatOn then
                            --wait(1)
                            NoClickDelay.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            NoClickDelay.TextColor3 = Color3.fromRGB(255, 255, 255) 


                            getmetatable(bedwars["SwordController"]).isClickingTooFast = function(...) 
                                return false
                            end
                        end
                    end)

                    spawn(function() 
                        if MultiAuraCheatOn then
                            --wait(1)
                            MultiAura.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            MultiAura.TextColor3 = Color3.fromRGB(255, 255, 255) 

                            bedwars["PaintRemote"] = getremote(debug.getconstants(KnitClient.Controllers.PaintShotgunController.fire))

                            repeat
                                task.wait(0.03)
                                local plrs = GetAllNearestHumanoidToPosition(18.8)
                                for i,plr in pairs(plrs) do
                                    local selfpos = game.Players.LocalPlayer.Character.HumanoidRootPart.Position
                                    local newpos = plr.Character.HumanoidRootPart.Position
                                    bedwars["ClientHandler"]:Get(bedwars["PaintRemote"]):SendToServer(selfpos, CFrame.lookAt(selfpos, newpos).lookVector)
                                end
                            until MultiAuraCheatOn == false
                            
                        end
                    end)

                    spawn(function()
                        if EmeraldESPCheatOn then
                            EmeraldESP.BackgroundColor3 = Color3.fromRGB(41, 166, 255)
                            EmeraldESP.TextColor3 = Color3.fromRGB(255, 255, 255)  

                            -- EMERALD CHAMS ONLY WORKS WITH SKYWARS
                            coroutine.wrap(function()
                                while EmeraldESPCheatOn do
                                    for i,v in pairs(game:GetService("CollectionService"):GetTagged("chest")) do
                                        local chest = v.ChestFolderValue.Value
                                        local chestitems = chest and chest:GetChildren() or {}

                                        
                                        if not (v.Model:FindFirstChild("Highlight") == nil) then
                                            v.Model.Highlight:Destroy()
                                        end

                                        for index, data in ipairs(chestitems) do
                                            if string.find(tostring(data), "emerald") then
                                                local a = Instance.new("Highlight")
                                                a.Enabled = true
                                                a.FillColor = Color3.fromRGB(57,255,20)
                                                a.Parent=v.Model
                                            end
                                        end
                                    end
                                    wait(.1)
                                end
                            end)()
                        end
                    end)
                end
            else
                print('try')
                saveSettings()
                return loadSettings()
            end

            return {loadedValues, keyBinds}
        else 
            print('g')
            saveSettings()
            return loadSettings()
        end
    end

    function tablesEqual(a, b) 
        for i=1, #a, 1 do
            if not (a[i] == b[i]) then
                return false
            end
        end
        return true
    end

    
    old = loadSettings()
    saveSettings()
    
    if not pcall(tablesEqual(old[1], loadSettings())) then
        old = loadSettings()
    end
    if not pcall(tablesEqual(old[2], loadSettings())) then
        old = loadSettings()
    end



    while true do
        saveSettings()
        if not (tablesEqual(old[1], loadSettings())) then
            old = loadSettings()
        end
        if not (tablesEqual(old[2], loadSettings())) then
            old = loadSettings()
        end

        wait()
    end
    -- loadSettings()

end)()
end
