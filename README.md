-- =========================
-- F Z STUDIO HUB - BY YOUSTZZX
-- =========================

-- ======= BLOQUEIO DE JOGO (SÓ FUNCIONA NO FISH IT!) =======
local ALLOWED_PLACE_ID = 121864768012064  -- ID do Fish It!

if game.PlaceId ~= ALLOWED_PLACE_ID then
    warn("Este hub só funciona no Fish It!")
    return  -- Para o script se não for o jogo certo
end

print("Hub liberado! Você está no Fish It.")
-- ========================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer

-- =========================
-- LOAD WINDUI
-- =========================
local WindUI = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/Footagesus/WindUI/main/dist/main.lua"
))()

local Window = WindUI:CreateWindow({
    Title = "F z Hub BETA - [🎣] Fish It",
    Author = "         By YouStzzx",
    Folder = "FzStudiosHub",
    Size = UDim2.fromOffset(580,460),
    Theme = "Midnight",
    Transparent = true,
    Resizable = true,
    User = { Enabled = true, Anonymous = false }
})

-- =========================
--  TABS
-- =========================
local DiscordTab   = Window:Tab({ Title = "Discord", Icon = "info" })
local AutoFarmTab    = Window:Tab({ Title = "Auto Farm", Icon = "zap" })
local TeleportesTab    = Window:Tab({ Title = "Teleports", Icon = "locate" })
local PlayersTab = Window:Tab({ Title = "Players", Icon = "user" })
local ThemeTab    = Window:Tab({ Title = "Theme", Icon = "palette" })
local MiscTab   = Window:Tab({ Title = "Misc", Icon = "settings" })




-- =========================
-- STATE
-- =========================
local State = {}
local LastPosition = nil

-- =========================
-- ANTI-AFK SEMPRE ATIVO
-- =========================
local VirtualUser = game:GetService("VirtualUser")
player.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new())
end)

RunService.Heartbeat:Connect(function()
    pcall(function()
        player.Idled:Fire()
    end)
end)

-- =========================
-- DISCORD TAB - 
-- =========================
DiscordTab:Section({
    Title = "Link Discord F z Studio",
    Icon = "link"
})

DiscordTab:Image({
    Image = "rbxassetid://110374623626433",  -- ícone do Discord (pode trocar pelo ID exato da sua foto se for diferente)
    AspectRatio = 1,
    Radius = 5
})

DiscordTab:Paragraph({
    Title = "Join our F z Studios",
    Desc = "Clique no botão abaixo para copiar o link do Discord e entrar na comunidade!"
})

DiscordTab:Button({
    Title = "Copy Invite Link",
    Description = "Copia o link de convite pro seu clipboard",
    Callback = function()
        setclipboard("https://discord.gg/4EEYPWwp")  -- ← COLOQUE SEU LINK REAL AQUI
        WindUI:Notify({
            Title = "Copiado!",
            Content = "Link do Discord copiado para a área de transferência!",
            Duration = 3
        })
    end
})

-- =========================
-- TELEPORTES - TELEPORTE SLANDS AND PERIFERICOS AND VENDORS SHOP ETC
-- =========================
TeleportesTab:Section({
    Title = "Teleport Slands"
})

-- Lista de teleports com nome e coordenadas
-- Adicione ou remova quantos quiser! Só seguir o modelo:
-- ["Nome do Lugar"] = Vector3.new(x, y, z),

local Teleports = {
    ["Lost Isle"] = Vector3.new(-3653, 5, -1051),
    ["Kohana"] = Vector3.new(-684, 3, 791),              -- exemplo
    ["Kohana Volcano"] = Vector3.new(-590, 59, 106),
    [" Ancient Jungle"] = Vector3.new(1496, 8, -476),
    ["Esoteric Depths"] = Vector3.new(2041, 27, 1386),
    ["Crater Island"] = Vector3.new(1014, 23, 5079),
    ["Tropical Grove"] = Vector3.new(-2060, 54, 3772),
    ["Coral Reefs"] = Vector3.new(-2930, 146, 2329),
    -- Adicione mais aqui se quiser!
}

-- Cria o dropdown com os nomes dos lugares
local TeleportDropdown = TeleportesTab:Dropdown({
    Title = "Select Location",
    Description = "Escolha um lugar para teleportar",
    Values = {},
    Callback = function(placeName)
        -- Não precisa fazer nada aqui, só salva o nome (opcional)
    end
})

-- Atualiza a lista do dropdown com os nomes dos teleports
local function UpdateTeleportList()
    local names = {}
    for name, _ in pairs(Teleports) do
        table.insert(names, name)
    end
    table.sort(names)
    TeleportDropdown:Refresh(names)
end

-- Atualiza uma vez no início
task.spawn(UpdateTeleportList)

-- Botão para teleportar pro lugar selecionado
TeleportesTab:Button({
    Title = "Teleport to Location",
    Description = "Teleporta para o lugar selecionado",
    Callback = function()
        local selectedName = TeleportDropdown.Value  -- Pega o nome selecionado
        if selectedName and Teleports[selectedName] then
            local targetPos = Teleports[selectedName]
            local char = player.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                char.HumanoidRootPart.CFrame = CFrame.new(targetPos)
                WindUI:Notify({
                    Title = "Teleport",
                    Content = "Teleportado para: " .. selectedName,
                    Duration = 3
                })
            else
                WindUI:Notify({
                    Title = "Erro",
                    Content = "Personagem não carregado!",
                    Duration = 3
                })
            end
        else
            WindUI:Notify({
                Title = "Erro",
                Content = "Selecione um lugar válido!",
                Duration = 3
            })
        end
    end
})
TeleportesTab:Section({
    Title = "Teleport Fisher And Alls"
})

-- Lista de teleports com nome e coordenadas
-- Adicione ou remova quantos quiser! Só seguir o modelo:
-- ["Nome do Lugar"] = Vector3.new(x, y, z),

local Teleports = {
    ["Sell Here"] = Vector3.new(49, 17, 2867),
    ["Buy Iscas"] = Vector3.new(112, 17, 2871),              -- exemplo
    ["Buy Fishing Rods"] = Vector3.new(151, 20, 2833),
    ["Breed Fishing Rod Skins"] = Vector3.new(80, 17, 2868),
    ["Buy Equipment"] = Vector3.new(-37, 20, 2875),
    ["Traveling"] = Vector3.new(-135, 3, 2766),
    ["Prize Roulette "] = Vector3.new(-140, 17, 2824),
    -- Adicione mais aqui se quiser!
}

-- Cria o dropdown com os nomes dos lugares
local TeleportDropdown = TeleportesTab:Dropdown({
    Title = "Select Location",
    Description = "Escolha um lugar para teleportar",
    Values = {},
    Callback = function(placeName)
        -- Não precisa fazer nada aqui, só salva o nome (opcional)
    end
})

-- Atualiza a lista do dropdown com os nomes dos teleports
local function UpdateTeleportList()
    local names = {}
    for name, _ in pairs(Teleports) do
        table.insert(names, name)
    end
    table.sort(names)
    TeleportDropdown:Refresh(names)
end

-- Atualiza uma vez no início
task.spawn(UpdateTeleportList)

-- Botão para teleportar pro lugar selecionado
TeleportesTab:Button({
    Title = "Teleport to Location",
    Description = "Teleporta para o lugar selecionado",
    Callback = function()
        local selectedName = TeleportDropdown.Value  -- Pega o nome selecionado
        if selectedName and Teleports[selectedName] then
            local targetPos = Teleports[selectedName]
            local char = player.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                char.HumanoidRootPart.CFrame = CFrame.new(targetPos)
                WindUI:Notify({
                    Title = "Teleport to Peripherals ",
                    Content = "Teleportado para: " .. selectedName,
                    Duration = 3
                })
            else
                WindUI:Notify({
                    Title = "Erro",
                    Content = "Personagem não carregado!",
                    Duration = 3
                })
            end
        else
            WindUI:Notify({
                Title = "Erro",
                Content = "Selecione um lugar válido!",
                Duration = 3
            })
        end
    end
})

-- =========================
-- AUTO FARM TAB - AUTO FISHING (ANTI BAN)
-- =========================
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Net = ReplicatedStorage
    :WaitForChild("Packages")
    :WaitForChild("_Index")
    :WaitForChild("sleitnick_net@0.2.0")
    :WaitForChild("net")

local AutoFishEnabled = false
local AutoFishLoop = nil

local function EquipRod()
    pcall(function()
        Net:WaitForChild("RE/EquipToolFromHotbar"):FireServer(1)
    end)
end

local function UnequipRod()
    pcall(function()
        Net:WaitForChild("RE/UnequipToolFromHotbar"):FireServer()
    end)
end

local function ChargeRod()
    pcall(function()
        Net:WaitForChild("RF/ChargeFishingRod"):InvokeServer()
    end)
end

local function SpamLucky()
    for i = 1, 10 do
        pcall(function()
            local args = {
                -1.233184814453125,
                0.9408596542227852,
                os.clock()
            }
            Net:WaitForChild("RF/RequestFishingMinigameStarted"):InvokeServer(unpack(args))
        end)
        task.wait(0.1)
    end
end

local function FinishFishing()
    pcall(function()
        Net:WaitForChild("RE/FishingCompleted"):FireServer()
    end)
end

local function StartAutoFish()
    if AutoFishLoop then return end

    EquipRod()
    task.wait(1)

    AutoFishLoop = task.spawn(function()
        while AutoFishEnabled do
            ChargeRod()
            task.wait(1.5)
            SpamLucky(8)
            task.wait(1)
            FinishFishing()
            task.wait(2)
        end
        UnequipRod()
        AutoFishLoop = nil
    end)
end

local function StopAutoFish()
    AutoFishEnabled = false
    if AutoFishLoop then task.cancel(AutoFishLoop) end
    task.wait(0.5)
    UnequipRod()
end
AutoFarmTab:Section({ 
    Title = "Fishing Settings" 
})

AutoFarmTab:Toggle({
    Title = "Auto Fish",
    Description = "Pesca automaticamente e tenta o Lucky multiplier",
    Default = false,
    Callback = function(value)
        AutoFishEnabled = value
        if value then
            StartAutoFish()
            WindUI:Notify({
                Title = "Auto Fish",
                Content = "ATIVADO!",
                Duration = 3
            })
        else
            StopAutoFish()
            WindUI:Notify({
                Title = "Auto Fish",
                Content = "DESATIVADO!",
                Duration = 3
            })
        end
    end
})

AutoFarmTab:Button({
    Title = "Sell All The Fishs",
    Description = "Vender todos os peixes",
    Callback = function()
        game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_net@0.2.0"):WaitForChild("net"):WaitForChild("RF/SellAllItems"):InvokeServer()
    end
})

-- =========================
-- PLAYERS TAB
-- =========================
local SelectedPlayer = nil
local PlayerDropdown

local function UpdatePlayerList()
    local names = {}
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= player then table.insert(names, p.Name) end
    end
    table.sort(names)
    PlayerDropdown:Refresh(names)
end

PlayerDropdown = PlayersTab:Dropdown({
    Title = "Select Player",
    Description = "Choose a player to target",
    Values = {},
    Callback = function(n)
        SelectedPlayer = Players:FindFirstChild(n)
    end
})

Players.PlayerAdded:Connect(UpdatePlayerList)
Players.PlayerRemoving:Connect(UpdatePlayerList)
task.spawn(UpdatePlayerList)

PlayersTab:Button({
    Title = "Teleport to Player",
    Description = "Teleport behind the selected player",
    Callback = function()
        if SelectedPlayer and SelectedPlayer.Character and SelectedPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local char = player.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local targetHRP = SelectedPlayer.Character.HumanoidRootPart
                char.HumanoidRootPart.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 4)
            end
        end
    end
})

PlayersTab:Toggle({
    Title = "Loop Teleport",
    Description = "Continuously follow the selected player",
    Key = "LoopTeleport",
    Default = false,
    Callback = function(v)
        State.LoopTeleport = v

        if v then
            local char = player.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                LastPosition = char.HumanoidRootPart.CFrame
            end
        else
            if LastPosition and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = LastPosition
            end
            LastPosition = nil
        end

        if State.LoopTeleportConn then State.LoopTeleportConn:Disconnect() State.LoopTeleportConn = nil end
        if v then
            State.LoopTeleportConn = RunService.Heartbeat:Connect(function()
                if not State.LoopTeleport or not SelectedPlayer or not SelectedPlayer.Character or not SelectedPlayer.Character:FindFirstChild("HumanoidRootPart") then return end
                local char = player.Character
                if char and char:FindFirstChild("HumanoidRootPart") then
                    local targetHRP = SelectedPlayer.Character.HumanoidRootPart
                    char.HumanoidRootPart.CFrame = targetHRP.CFrame * CFrame.new(0, 0, 4)
                end
            end)
        end
    end
})

-- =========================
-- THEME TAB
-- =========================
ThemeTab:Dropdown({
    Title = "Select Theme",
    Description = "Choose your preferred UI theme",
    Values = {
        "Dark", "Light", "Rose", "Plant", "Red", "Indigo", "Sky",
        "Violet", "Amber", "Emerald", "Midnight", "Crimson",
        "MonokaiPro", "CottonCandy", "Rainbow"
    },
    Default = "Midnight",
    Callback = function(theme)
        WindUI:SetTheme(theme)
    end
})

-- =========================
-- MISC TAB
-- =========================
local OriginalLighting = {
    Ambient = Lighting.Ambient,
    Brightness = Lighting.Brightness,
    ClockTime = Lighting.ClockTime,
    FogEnd = Lighting.FogEnd,
    FogStart = Lighting.FogStart,
    GlobalShadows = Lighting.GlobalShadows,
    OutdoorAmbient = Lighting.OutdoorAmbient
}

local function ApplyFullbright()
    Lighting.Ambient = Color3.fromRGB(255, 255, 255)
    Lighting.Brightness = 2
    Lighting.ClockTime = 12
    Lighting.FogEnd = 100000
    Lighting.FogStart = 0
    Lighting.GlobalShadows = false
    Lighting.OutdoorAmbient = Color3.fromRGB(255, 255, 255)
end

local function RestoreOriginalLighting()
    Lighting.Ambient = OriginalLighting.Ambient
    Lighting.Brightness = OriginalLighting.Brightness
    Lighting.ClockTime = OriginalLighting.ClockTime
    Lighting.FogEnd = OriginalLighting.FogEnd
    Lighting.FogStart = OriginalLighting.FogStart
    Lighting.GlobalShadows = OriginalLighting.GlobalShadows
    Lighting.OutdoorAmbient = OriginalLighting.OutdoorAmbient
end

MiscTab:Toggle({
    Title = "Fullbright",
    Description = "Removes darkness and fog",
    Key = "FullbrightToggle",
    Default = false,
    Callback = function(v)
        State.Fullbright = v
        if v then
            ApplyFullbright()
        else
            RestoreOriginalLighting()
        end
    end
})

MiscTab:Button({
    Title = "Infinite Yield",
    Description = "Loads Infinite Yield admin commands",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()
    end
})

-- DESYNC ATUALIZADO COM O NOVO LINK
MiscTab:Button({
    Title = "Desync",
    Description = "Loads Desync script (By slowzzx4)",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/slowzzxX/Desyncv2/refs/heads/main/By%20slowzzx4"))()
    end
})

MiscTab:Button({
    Title = "Fly V5",
    Description = "Loads Fly V5 script",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/slowzzxX/Flyv12/main/Flyv12.lua"))()
    end
})


-- =========================
-- FINALIZATION
-- =========================
if player.Character then task.spawn(ApplySpeedJump) end
Window:SelectTab(1)
