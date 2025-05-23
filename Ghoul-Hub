
-- Ro-Ghoul Hub by ChatGPT
-- OrionLib GUI + Full Automation Features (Toggle-Based)

local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HRP = Character:WaitForChild("HumanoidRootPart")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local Window = OrionLib:MakeWindow({Name = "Ro-Ghoul Hub", HidePremium = false, SaveConfig = true, ConfigFolder = "RoGhoulHub"})

-- Global Toggles
local targetGroup = "Aogiri"
local autoFarm = false
local autoBossFarm = false
local autoStats = false
local autoTrainer = false
local useSkills = false
local autoKick = false
local whitelist = {}

local bossTargets = {
    ["Gyakusatsu"] = 1250,
    ["Eto Yoshimura"] = 1250,
    ["Nishiki Nishio"] = 250,
    ["Koutarou Amon"] = 750,
}

-- Utility
function isWhitelisted(name)
    for _, n in ipairs(whitelist) do
        if n == name then return true end
    end
    return false
end

function autoKickPlayers()
    for _, p in ipairs(game.Players:GetPlayers()) do
        if p ~= Player and not isWhitelisted(p.Name) then
            p:Kick("Auto-kicked by script.")
        end
    end
end

function addStats()
    local e = ReplicatedStorage:WaitForChild("StatsEvent")
    e:FireServer("Add", "Kagune", 1)
    e:FireServer("Add", "Durability", 1)
end

function attackWithSkills()
    local UserInputService = game:GetService("UserInputService")
    for _, key in ipairs({"E", "R", "C", "F"}) do
        UserInputService.InputBegan:Fire({KeyCode = Enum.KeyCode[key], UserInputType = Enum.UserInputType.Keyboard})
    end
end

function findNPC(name)
    for _, npc in ipairs(workspace.NPCs:GetChildren()) do
        if npc.Name:find(name) and npc:FindFirstChild("HumanoidRootPart") then
            return npc
        end
    end
    return nil
end

function moveToTarget(target, offset)
    if Character and target then
        HRP.CFrame = target.HumanoidRootPart.CFrame * CFrame.new(0, 0, offset)
    end
end

function doAutoFarm()
    local t = findNPC(targetGroup)
    if t then
        moveToTarget(t, 6)
        if useSkills then attackWithSkills() end
    end
end

function doBossFarm()
    for name, level in pairs(bossTargets) do
        local t = findNPC(name)
        if t then
            moveToTarget(t, 8)
            if useSkills then attackWithSkills() end
        end
    end
end

function doTrainer()
    local t = workspace:FindFirstChild("Nishiki Nishio")
    if t then
        moveToTarget(t, 3)
        wait(1)
        ReplicatedStorage:WaitForChild("TrainerEvent"):FireServer("Progress")
    end
end

-- Tabs
local mainTab = Window:MakeTab({Name = "Main", Icon = "rbxassetid://4483345998", PremiumOnly = false})
mainTab:AddDropdown({
    Name = "Select Target",
    Default = "Aogiri",
    Options = {"Aogiri", "Human Investigator"},
    Callback = function(value) targetGroup = value end
})
mainTab:AddToggle({
    Name = "Start Farming",
    Default = false,
    Callback = function(value) autoFarm = value end
})

local farmTab = Window:MakeTab({Name = "Farm Options", Icon = "rbxassetid://4483345998", PremiumOnly = false})
farmTab:AddToggle({
    Name = "Boss Farm",
    Default = false,
    Callback = function(value) autoBossFarm = value end
})
farmTab:AddToggle({
    Name = "Auto Use Skills (E,R,C,F)",
    Default = true,
    Callback = function(value) useSkills = value end
})

local trainerTab = Window:MakeTab({Name = "Trainer", Icon = "rbxassetid://4483345998", PremiumOnly = false})
trainerTab:AddToggle({
    Name = "Auto Trainer",
    Default = false,
    Callback = function(value) autoTrainer = value end
})

local miscTab = Window:MakeTab({Name = "Misc", Icon = "rbxassetid://4483345998", PremiumOnly = false})
miscTab:AddToggle({
    Name = "Auto Add Stats",
    Default = false,
    Callback = function(value) autoStats = value end
})
miscTab:AddToggle({
    Name = "Auto Kick Non-Whitelist",
    Default = false,
    Callback = function(value) autoKick = value end
})
miscTab:AddTextbox({
    Name = "Whitelist (comma separated)",
    Default = "",
    TextDisappear = false,
    Callback = function(txt)
        whitelist = {}
        for word in txt:gmatch("[^,%s]+") do
            table.insert(whitelist, word)
        end
    end
})

-- Looping Function
game:GetService("RunService").RenderStepped:Connect(function()
    if autoFarm then doAutoFarm() end
    if autoBossFarm then doBossFarm() end
    if autoStats then addStats() end
    if autoTrainer then doTrainer() end
    if autoKick then autoKickPlayers() end
end)

OrionLib:Init()
