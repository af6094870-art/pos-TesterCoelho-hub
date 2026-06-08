local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = " Coelho Hub",
    SubTitle = "by mr",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.RightControl
})
local ScreenGui = Instance.new("ScreenGui")
local ToggleBtn = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

ToggleBtn.Parent = ScreenGui
ToggleBtn.Size = UDim2.new(0, 60, 0, 60)
ToggleBtn.Position = UDim2.new(0, 20, 0.5, 0)
ToggleBtn.BackgroundTransparency = 1
ToggleBtn.Image = "rbxassetid://94453919385793"

UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ToggleBtn

-- Abre/fecha igual RightControl
ToggleBtn.MouseButton1Click:Connect(function()
    game:GetService("VirtualInputManager"):SendKeyEvent(true, Enum.KeyCode.RightControl, false, game)
    task.wait(0.1)
    game:GetService("VirtualInputManager"):SendKeyEvent(false, Enum.KeyCode.RightControl, false, game)
end)

-- Arrastar a bolinha
local dragging, dragStart, startPos

ToggleBtn.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = ToggleBtn.Position
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        ToggleBtn.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        ToggleBtn.Position = UDim2.new(
            startPos.X.Scale, 
            startPos.X.Offset + delta.X, 
            startPos.Y.Scale, 
            startPos.Y.Offset + delta.Y
        )
    end
end)

game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
-- ========================================================
-- 1. PRIMEIRO: CRIA TODAS AS ABAS DO MENU
-- ========================================================
local Tabs = {
    ShopTab = Window:AddTab({ Title = "ShopTab", Icon = "" }),
    Race = Window:AddTab({ Title = "Race", Icon = "" }),
    Others = Window:AddTab({ Title = "Others", Icon = "" }),
    Status = Window:AddTab({ Title = "Status and Server", Icon = "" }),
    Config = Window:AddTab({ Title = "config", Icon = "" }),
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
    Items = Window:AddTab({ Title = "Items", Icon = "" }),
    Stack = Window:AddTab({ Title = "Stack farm", Icon = "" }),
    Fruit = Window:AddTab({ Title = "Fruit", Icon = "" }),
    Teleport = Window:AddTab({ Title = "Teleport", Icon = "" }),
    Creditos = Window:AddTab({ Title = "Creditos", Icon = "" }),
    seaevent = Window:AddTab({ Title = "Sea Event", Icon = "" }),
    PvpTab = Window:AddTab({ Title = "PvpTab", Icon = "" })
}

-- [[ GERENCIADORES INTERNOS ]]
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)
SaveManager:SetIgnoreIndexes({})


-- ========================================================
-- ABA CONFIG
-- ========================================================

local replicated = game:GetService("ReplicatedStorage")

local Time = Tabs.Status:AddParagraph({ Title = "Time Zone", Content = "" })
local Timmessss = Tabs.Status:AddParagraph({ Title = "Game Time", Content = "" })
local Miragecheck = Tabs.Status:AddParagraph({ Title = "Mirage Island", Content = "Status: " })
local Kitsunecheck = Tabs.Status:AddParagraph({ Title = "Kitsune Island", Content = "Status: " })
local CPrehistoriccheck = Tabs.Status:AddParagraph({ Title = "Prehistoric Island", Content = "Status: " })
local FrozenIsland = Tabs.Status:AddParagraph({ Title = "Frozen Dimension", Content = "Status: " })
local MobCakePrince = Tabs.Status:AddParagraph({ Title = "Cake Prince Kills", Content = "" })
local TyrantStatus = Tabs.Status:AddParagraph({ Title = "Tyrant of the Skies", Content = "Status: " })
local CheckRip = Tabs.Status:AddParagraph({ Title = "Rip_Indra", Content = "Status: " })
local CheckDoughKing = Tabs.Status:AddParagraph({ Title = "Dough King", Content = "Status: " })
local EliteHunter = Tabs.Status:AddParagraph({ Title = "Elite Hunter", Content = "Status: " })
local Pullever = Tabs.Status:AddParagraph({ Title = "Pull Lever", Content = "Status: " })
local FM = Tabs.Status:AddParagraph({ Title = "Full Moon", Content = "" })
local LegendarySword = Tabs.Status:AddParagraph({ Title = "Legendary Sword", Content = "Status: " })
local Bone = Tabs.Status:AddParagraph({ Title = "Bones", Content = "" })
local CheckFod = Tabs.Status:AddParagraph({ Title = "First Of Darkness", Content = "Status: " })
local CheckChalice = Tabs.Status:AddParagraph({ Title = "God Chalice", Content = "Status: " })
local FrutasSpawn = Tabs.Status:AddParagraph({ Title = "Frutas no Mapa", Content = "carregando..." })

task.spawn(function()
    while task.wait(1) do
        -- Time Zone
        pcall(function()
            local date = os.date("*t")
            local hour = date.hour % 24
            local ampm = hour < 12 and "AM" or "PM"
            local timezone = string.format("%02i:%02i:%02i %s", ((hour - 1) % 12) + 1, date.min, date.sec, ampm)
            local datetime = string.format("%02d/%02d/%04d", date.day, date.month, date.year)
            local ok, code = pcall(function()
                return game:GetService("LocalizationService"):GetCountryRegionForPlayerAsync(game.Players.LocalPlayer)
            end)
            Time:SetDesc(datetime .. " - " .. timezone .. " [ " .. (ok and code or "??") .. " ]")
        end)

        -- Game Time
        pcall(function()
            local GameTime = math.floor(workspace.DistributedGameTime + 0.5)
            local Hour = math.floor(GameTime / 3600) % 24
            local Minute = math.floor(GameTime / 60) % 60
            local Second = GameTime % 60
            Timmessss:SetDesc(Hour .. "h " .. Minute .. "m " .. Second .. "s")
        end)

        -- Mirage Island
        pcall(function()
            Miragecheck:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Mirage Island") and "✅" or "❌")
        end)

        -- Kitsune Island
        pcall(function()
            Kitsunecheck:SetDesc(workspace.Map:FindFirstChild("KitsuneIsland") and "✅" or "❌")
        end)

        -- Prehistoric Island
        pcall(function()
            CPrehistoriccheck:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Prehistoric Island") and "✅" or "❌")
        end)

        -- Frozen Dimension
        pcall(function()
            FrozenIsland:SetDesc(workspace._WorldOrigin.Locations:FindFirstChild("Frozen Dimension") and "✅" or "❌")
        end)

        -- Cake Prince
        pcall(function()
            local response = replicated.Remotes.CommF_:InvokeServer("CakePrinceSpawner")
            local len = string.len(tostring(response))
            if len == 88 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 41))
            elseif len == 87 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 40))
            elseif len == 86 then
                MobCakePrince:SetDesc("Kill : " .. string.sub(response, 39, 39))
            else
                MobCakePrince:SetDesc("Cake Prince : ✅")
            end
        end)

        -- Tyrant
        pcall(function()
            TyrantStatus:SetDesc(workspace.Enemies:FindFirstChild("Tyrant of the Skies") and "✅" or "❌")
        end)

        -- Rip Indra
        pcall(function()
            CheckRip:SetDesc((replicated:FindFirstChild("rip_indra True Form") or workspace.Enemies:FindFirstChild("rip_indra")) and "✅" or "❌")
        end)

        -- Dough King
        pcall(function()
            CheckDoughKing:SetDesc((replicated:FindFirstChild("Dough King") or workspace.Enemies:FindFirstChild("Dough King")) and "✅" or "❌")
        end)

        -- Elite Hunter
        pcall(function()
            local progress = replicated.Remotes.CommF_:InvokeServer("EliteHunter", "Progress")
            local eliteFound = replicated:FindFirstChild("Diablo") or replicated:FindFirstChild("Deandre") or replicated:FindFirstChild("Urban")
                or workspace.Enemies:FindFirstChild("Diablo") or workspace.Enemies:FindFirstChild("Deandre") or workspace.Enemies:FindFirstChild("Urban")
            EliteHunter:SetDesc((eliteFound and "✅" or "❌") .. " | Killed: " .. tostring(progress))
        end)

        -- Pull Lever
        pcall(function()
            Pullever:SetDesc(replicated.Remotes.CommF_:InvokeServer("CheckTempleDoor") and "✅" or "❌")
        end)

       pcall(function()
    local sky = game:GetService("Lighting"):FindFirstChildOfClass("Sky")
    if sky then
        local moonPhases = {
            ["http://www.roblox.com/asset/?id=9709149431"] = "Moon: 5/5 🌕",
            ["http://www.roblox.com/asset/?id=9709149052"] = "Moon: 4/5",
            ["http://www.roblox.com/asset/?id=9709143733"] = "Moon: 3/5",
            ["http://www.roblox.com/asset/?id=9709150401"] = "Moon: 2/5",
            ["http://www.roblox.com/asset/?id=9709149680"] = "Moon: 1/5",
        }
        FM:SetDesc(moonPhases[sky.MoonTextureId] or "Moon: 0/5")
    else
        FM:SetDesc("Sky não encontrado")
    end
end)
        -- Legendary Sword
        pcall(function()
            local rs = replicated.Remotes.CommF_
            if rs:InvokeServer("LegendarySwordDealer", "1") then
                LegendarySword:SetDesc("Shisui ✅")
            elseif rs:InvokeServer("LegendarySwordDealer", "2") then
                LegendarySword:SetDesc("Wando ✅")
            elseif rs:InvokeServer("LegendarySwordDealer", "3") then
                LegendarySword:SetDesc("Saddi ✅")
            else
                LegendarySword:SetDesc("Not Found ❌")
            end
        end)

        -- Bones
        pcall(function()
            local bones = replicated.Remotes.CommF_:InvokeServer("Bones", "Check")
            Bone:SetDesc("You Have : " .. tostring(bones) .. " Bones")
        end)
    end
end)

-- Loop Separado: First Of Darkness
task.spawn(function()
	while true do
		task.wait(1)
		pcall(function()
			local found = false
			for _, v in pairs(workspace:GetDescendants()) do
				if v.Name:lower():find("first") and v.Name:lower():find("dark") then
					found = true
					break
				end
			end
			CheckFod:SetDesc(found and "Status: ✅" or "Status: ❌")
		end)
	end
end)

-- Loop Separado: God Chalice
task.spawn(function()
	while true do
		task.wait(1)
		pcall(function()
			local found = false
			for _, v in pairs(workspace:GetDescendants()) do
				if v.Name:lower():find("god") and v.Name:lower():find("chal") then
					found = true
					break
				end
			end
			CheckChalice:SetDesc(found and "Status: ✅" or "Status: ❌")
		end)
	end
end)

local FRUTAS_LISTA = {
    "Rocket", "Spin", "Chop", "Spring", "Bomb", "Spike", "Smoke", "Flame", "Eagle",
    "Ice", "Sand", "Dark", "Diamond", "Light", "Rubber", "Barrier", "Ghost", "Magma",
    "Quake", "Buddha", "Love", "Spider", "Sound", "Phoenix", "Portal", "Rumble",
    "Blizzard", "Pain", "Yeti", "Gravity", "Dough", "Shadow", "Venom", "Control",
    "Spirit", "Gas", "Mammoth", "T-Rex", "Leopard", "Dragon", "Kitsune"
}

-- Converte lista em set para busca O(1)
local FRUTAS_SET = {}
for _, nome in ipairs(FRUTAS_LISTA) do
    FRUTAS_SET[nome] = true
    FRUTAS_SET[nome .. " Fruit"] = true
end

task.spawn(function()
    while task.wait(1) do
        pcall(function()
            local frutasNoMapa = {}
            local jaAdicionado = {}

            for _, obj in ipairs(workspace:GetChildren()) do
                if obj:IsA("Model") and FRUTAS_SET[obj.Name] then
                    local nomeLimpo = obj.Name:gsub(" Fruit", "")
                    if not jaAdicionado[nomeLimpo] and obj:FindFirstChildWhichIsA("BasePart") then
                        jaAdicionado[nomeLimpo] = true
                        table.insert(frutasNoMapa, nomeLimpo)
                    end
                end
            end

            if #frutasNoMapa == 0 then
                FrutasSpawn:SetDesc("não há nada aqui...")
            else
                FrutasSpawn:SetDesc("🍎 " .. table.concat(frutasNoMapa, " | "))
            end
        end)
    end
end)

-- ========================================================
-- ABA CRÉDITOS
-- (A Fluent não suporta AddImageLabel nativamente,
--  então usamos Paragraph pra dar o crédito da imagem)
-- ========================================================
Tabs.Creditos:AddParagraph({
    Title = " Musa do Café (Fiscal de Código)",
    Content = "A guardiã que salvou o Coelho Hub de 60 erros fatais.\nID da imagem: 108660186329467"
})

Tabs.Creditos:AddParagraph({
    Title = "Agradecimentos Especiais",
    Content = "Criado por: by mr."
})

-- ========================================================
-- ATALHO PARA MINIMIZAR (RightAlt)
-- ========================================================
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.RightAlt then
        Fluent:ToggleVisibility()
    end
end)
-- =====================================================
-- Auto Attack - Corrigido pra parar de dar erro no Studio
-- Funções de executor (getupvalue, setupvalue, etc)
-- funcionam normal no executor, aqui só silenciamos o Luau
-- =====================================================

-- Fallbacks pra o Studio não surtar com funções de executor
local getupvalue   = getupvalue   or function() return nil end
local setupvalue   = setupvalue   or function() end
local getgc        = getgc        or function() return {} end
local hookfunction = hookfunction or function() end
local iscclosure   = iscclosure   or function() return false end
local getsenv      = getsenv      or function() return {} end
 
-- =====================================================
loadstring(game:HttpGet("https://raw.githubusercontent.com/AnhDzaiScript/Setting/refs/heads/main/FastMax.lua"))()
 
local function GetBladeHits()
    local targets = {}
    local function GetDistance(v)
        return (v.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
    end
 
    for _, part in pairs({game.Workspace.Enemies, game.Workspace.Characters}) do
        for _, v in pairs(part:GetChildren()) do
            if v:FindFirstChild("HumanoidRootPart") and v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") then
                if GetDistance(v.HumanoidRootPart) < 60 then
                    table.insert(targets, v)
                end
            end
        end
    end
 
    return targets
end
 
local function AttackAll()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if not character then return end
 
    local equippedWeapon = character:FindFirstChild("EquippedWeapon")
    if not equippedWeapon then return end
 
    local enemies = GetBladeHits()
    if #enemies > 0 then
        local netModule = game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("Net")
        netModule:WaitForChild("RE/RegisterAttack"):FireServer(-math.huge)
 
        local args = {nil, {}}
        for i, v in pairs(enemies) do
            if not args[1] then
                args[1] = v.Head
            end
            args[2][i] = {v, v.HumanoidRootPart}
        end
 
        netModule:WaitForChild("RE/RegisterHit"):FireServer(unpack(args))
    end
end
 
spawn(function()
    while task.wait() do AttackAll() end
end)
 
-- =====================================================
 
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local VirtualInputManager = game:GetService("VirtualInputManager")
 
local Player = Players.LocalPlayer
local Modules = ReplicatedStorage:WaitForChild("Modules")
local Net = Modules:WaitForChild("Net")
local RegisterAttack = Net:WaitForChild("RE/RegisterAttack")
local RegisterHit = Net:WaitForChild("RE/RegisterHit")
local ShootGunEvent = Net:WaitForChild("RE/ShootGunEvent")
local GunValidator = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Validator2")
 
local Config = {
    AttackDistance = 90,
    AttackMobs = true,
    AttackPlayers = true,
    AttackCooldown = 0.2,
    ComboResetTime = 0.3,
    MaxCombo = 4,
    HitboxLimbs = {"RightLowerArm", "RightUpperArm", "LeftLowerArm", "LeftUpperArm", "RightHand", "LeftHand"},
    AutoClickEnabled = true
}
 
local FastAttack = {}
FastAttack.__index = FastAttack
 
function FastAttack.new()
    local self = setmetatable({
        Debounce = 0,
        ComboDebounce = 0,
        ShootDebounce = 0,
        M1Combo = 0,
        EnemyRootPart = nil,
        Connections = {},
        Overheat = {Dragonstorm = {MaxOverheat = 3, Cooldown = 0, TotalOverheat = 0, Distance = 350, Shooting = false}},
        ShootsPerTarget = {["Dual Flintlock"] = 2},
        SpecialShoots = {["Skull Guitar"] = "TAP", ["Bazooka"] = "Position", ["Cannon"] = "Position", ["Dragonstorm"] = "Overheat"}
    }, FastAttack)
 
    pcall(function()
        self.CombatFlags = require(Modules.Flags).COMBAT_REMOTE_THREAD
        self.ShootFunction = getupvalue(require(ReplicatedStorage.Controllers.CombatController).Attack, 9)
        local LocalScript = Player:WaitForChild("PlayerScripts"):FindFirstChildOfClass("LocalScript")
        if LocalScript and getsenv then
            self.HitFunction = getsenv(LocalScript)._G.SendHitsToServer
        end
    end)
 
    return self
end
 
function FastAttack:IsEntityAlive(entity)
    local humanoid = entity and entity:FindFirstChild("Humanoid")
    return humanoid and humanoid.Health > 0
end
 
function FastAttack:CheckStun(Character, Humanoid, ToolTip)
    local Stun = Character:FindFirstChild("Stun")
    local Busy = Character:FindFirstChild("Busy")
    if Humanoid.Sit and (ToolTip == "Sword" or ToolTip == "Melee" or ToolTip == "Blox Fruit") then
        return false
    elseif Stun and Stun.Value > 0 or Busy and Busy.Value then
        return false
    end
    return true
end
 
function FastAttack:GetBladeHits(Character, Distance)
    local Position = Character:GetPivot().Position
    local BladeHits = {}
    Distance = Distance or Config.AttackDistance
 
    local function ProcessTargets(Folder)
        for _, Enemy in ipairs(Folder:GetChildren()) do
            if Enemy ~= Character and self:IsEntityAlive(Enemy) then
                local BasePart = Enemy:FindFirstChild(Config.HitboxLimbs[math.random(#Config.HitboxLimbs)]) or Enemy:FindFirstChild("HumanoidRootPart")
                if BasePart and (Position - BasePart.Position).Magnitude <= Distance then
                    if not self.EnemyRootPart then
                        self.EnemyRootPart = BasePart
                    else
                        table.insert(BladeHits, {Enemy, BasePart})
                    end
                end
            end
        end
    end
 
    if Config.AttackMobs then ProcessTargets(Workspace.Enemies) end
    if Config.AttackPlayers then ProcessTargets(Workspace.Characters) end
 
    return BladeHits
end
 
function FastAttack:GetClosestEnemy(Character, Distance)
    local BladeHits = self:GetBladeHits(Character, Distance)
    local Closest, MinDistance = nil, math.huge
 
    for _, Hit in ipairs(BladeHits) do
        local Magnitude = (Character:GetPivot().Position - Hit[2].Position).Magnitude
        if Magnitude < MinDistance then
            MinDistance = Magnitude
            Closest = Hit[2]
        end
    end
    return Closest
end
 
function FastAttack:GetCombo()
    local Combo = (tick() - self.ComboDebounce) <= Config.ComboResetTime and self.M1Combo or 0
    Combo = Combo >= Config.MaxCombo and 1 or Combo + 1
    self.ComboDebounce = tick()
    self.M1Combo = Combo
    return Combo
end
 
function FastAttack:ShootInTarget(TargetPosition)
    local Character = Player.Character
    if not self:IsEntityAlive(Character) then return end
 
    local Equipped = Character:FindFirstChildOfClass("Tool")
    if not Equipped or Equipped.ToolTip ~= "Gun" then return end
 
    local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or 0.3
    if (tick() - self.ShootDebounce) < Cooldown then return end
 
    local ShootType = self.SpecialShoots[Equipped.Name] or "Normal"
    if ShootType == "Position" or (ShootType == "TAP" and Equipped:FindFirstChild("RemoteEvent")) then
        Equipped:SetAttribute("LocalTotalShots", (Equipped:GetAttribute("LocalTotalShots") or 0) + 1)
        GunValidator:FireServer(self:GetValidator2())
 
        if ShootType == "TAP" then
            Equipped.RemoteEvent:FireServer("TAP", TargetPosition)
        else
            ShootGunEvent:FireServer(TargetPosition)
        end
        self.ShootDebounce = tick()
    else
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, true, game, 1)
        task.wait(0.05)
        VirtualInputManager:SendMouseButtonEvent(0, 0, 0, false, game, 1)
        self.ShootDebounce = tick()
    end
end
 
function FastAttack:GetValidator2()
    local v1 = getupvalue(self.ShootFunction, 15)
    local v2 = getupvalue(self.ShootFunction, 13)
    local v3 = getupvalue(self.ShootFunction, 16)
    local v4 = getupvalue(self.ShootFunction, 17)
    local v5 = getupvalue(self.ShootFunction, 14)
    local v6 = getupvalue(self.ShootFunction, 12)
    local v7 = getupvalue(self.ShootFunction, 18)
 
    local v8 = v6 * v2
    local v9 = (v5 * v2 + v6 * v1) % v3
    v9 = (v9 * v3 + v8) % v4
    v5 = math.floor(v9 / v3)
    v6 = v9 - v5 * v3
    v7 = v7 + 1
 
    setupvalue(self.ShootFunction, 15, v1)
    setupvalue(self.ShootFunction, 13, v2)
    setupvalue(self.ShootFunction, 16, v3)
    setupvalue(self.ShootFunction, 17, v4)
    setupvalue(self.ShootFunction, 14, v5)
    setupvalue(self.ShootFunction, 12, v6)
    setupvalue(self.ShootFunction, 18, v7)
 
    return math.floor(v9 / v4 * 16777215), v7
end
 
function FastAttack:UseNormalClick(Character, Humanoid, Cooldown)
    self.EnemyRootPart = nil
    local BladeHits = self:GetBladeHits(Character)
 
    if self.EnemyRootPart then
        RegisterAttack:FireServer(Cooldown)
        if self.CombatFlags and self.HitFunction then
            self.HitFunction(self.EnemyRootPart, BladeHits)
        else
            RegisterHit:FireServer(self.EnemyRootPart, BladeHits)
        end
    end
end
 
function FastAttack:UseFruitM1(Character, Equipped, Combo)
    local Targets = self:GetBladeHits(Character)
    if not Targets[1] then return end
 
    local Direction = (Targets[1][2].Position - Character:GetPivot().Position).Unit
    Equipped.LeftClickRemote:FireServer(Direction, Combo)
end
 
function FastAttack:Attack()
    if not Config.AutoClickEnabled or (tick() - self.Debounce) < Config.AttackCooldown then return end
    local Character = Player.Character
    if not Character or not self:IsEntityAlive(Character) then return end
 
    local Humanoid = Character.Humanoid
    local Equipped = Character:FindFirstChildOfClass("Tool")
    if not Equipped then return end
 
    local ToolTip = Equipped.ToolTip
    if not table.find({"Melee", "Blox Fruit", "Sword", "Gun"}, ToolTip) then return end
 
    local Cooldown = Equipped:FindFirstChild("Cooldown") and Equipped.Cooldown.Value or Config.AttackCooldown
    if not self:CheckStun(Character, Humanoid, ToolTip) then return end
 
    local Combo = self:GetCombo()
    Cooldown = Cooldown + (Combo >= Config.MaxCombo and 0.05 or 0)
    self.Debounce = Combo >= Config.MaxCombo and ToolTip ~= "Gun" and (tick() + 0.05) or tick()
 
    if ToolTip == "Blox Fruit" and Equipped:FindFirstChild("LeftClickRemote") then
        self:UseFruitM1(Character, Equipped, Combo)
    elseif ToolTip == "Gun" then
        local Target = self:GetClosestEnemy(Character, 120)
        if Target then
            self:ShootInTarget(Target.Position)
        end
    else
        self:UseNormalClick(Character, Humanoid, Cooldown)
    end
end
 
local AttackInstance = FastAttack.new()
table.insert(AttackInstance.Connections, RunService.Stepped:Connect(function()
    AttackInstance:Attack()
end))
 
-- getgc só existe no executor, no Studio retorna {} então o loop não faz nada
for _, v in pairs(getgc(true)) do
    if typeof(v) == "function" and iscclosure(v) then
        local name = debug.getinfo(v).name
        if name == "Attack" or name == "attack" or name == "RegisterHit" then
            hookfunction(v, function(...)
                AttackInstance:Attack()
                return v(...)
            end)
        end
    end
end
 
-- =====================================================
-- Fast 2
-- =====================================================
local Modules2 = game.ReplicatedStorage.Modules
local Net2 = Modules2.Net
local Register_Hit = Net2:WaitForChild("RE/RegisterHit")
local Register_Attack = Net2:WaitForChild("RE/RegisterAttack")
local Funcs = {}
 
local function GetAllBladeHits()
    local bladehits = {}
    for _, v in pairs(workspace.Enemies:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
 
local function Getplayerhit()
    local bladehits = {}
    for _, v in pairs(workspace.Characters:GetChildren()) do
        if v.Name ~= game.Players.LocalPlayer.Name and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0
        and (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 65 then
            table.insert(bladehits, v)
        end
    end
    return bladehits
end
 
function Funcs:Attack()
    local bladehits = {}
    for _, v in pairs(GetAllBladeHits()) do
        table.insert(bladehits, v)
    end
    for _, v in pairs(Getplayerhit()) do
        table.insert(bladehits, v)
    end
    if #bladehits == 0 then return end
 
    local args = {
        [1] = nil,
        [2] = {},
        [4] = "078da341"
    }
    for r, v in pairs(bladehits) do
        Register_Attack:FireServer(0)
        if not args[1] then
            args[1] = v.Head
        end
        args[2][r] = {
            [1] = v,
            [2] = v.HumanoidRootPart
        }
    end
    Register_Hit:FireServer(unpack(args))
end

Tabs.Config:AddSection("Settings / Configure")

local TeamDropdown

TeamDropdown = Tabs.Config:AddDropdown("TeamDropdown", {
    Title = "Selecionar Time",
    Values = {"---", "Marines", "Pirates"},
    Default = "---",
    Callback = function(Value)
        if Value == "-- Selecionar --" then return end
        local replicated = game:GetService("ReplicatedStorage")
        local success, err = pcall(function()
            replicated.Remotes.CommF_:InvokeServer("SetTeam", Value)
        end)
        if success then
            print("Entrou no time: " .. Value)
            task.wait(0.2)
            TeamDropdown:SetValue("---")
        else
            warn("Erro ao entrar no time: " .. tostring(err))
        end
    end
})

local selectedStats = {}
local plr = game.Players.LocalPlayer
local replicated = game:GetService("ReplicatedStorage")

Tabs.Config:AddDropdown("StatDropdown", {
    Title = "Stats ",
    Values = {"Melee", "Defense", "Sword", "Gun", "Devil"},
    Default = {},
    Multi = true,
    Callback = function(Value)
        selectedStats = {}
        for stat, enabled in pairs(Value) do
            if enabled then
                table.insert(selectedStats, stat)
            end
        end
    end
})

Tabs.Config:AddToggle("AutoStatToggle", {
    Title = "Auto Distribuir Stats",
    Default = false,
    Callback = function(Value)
        _G.AutoStat = Value
        if Value then
            task.spawn(function()
                while _G.AutoStat do
                    pcall(function()
                        if plr.Data.Points.Value ~= 0 then
                            local hasMelee = table.find(selectedStats, "Melee")
                            local hasDefense = table.find(selectedStats, "Defense")

                            if hasMelee and hasDefense then
                                -- prioridade melee mas ainda da pra defense
                                replicated.Remotes.CommF_:InvokeServer("AddPoint", "Melee", 2)
                                replicated.Remotes.CommF_:InvokeServer("AddPoint", "Defense", 1)
                            else
                                for _, stat in pairs(selectedStats) do
                                    local statName = stat == "Devil" and "Demon Fruit" or stat
                                    replicated.Remotes.CommF_:InvokeServer("AddPoint", statName, 1)
                                end
                            end
                        end
                    end)
                    task.wait(0)
                end
            end)
        end
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 1",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelMain")
        end)
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 2",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelDressrosa")
        end)
    end
})

Tabs.Teleport:AddButton({
    Title = "Teleport to Sea 3",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("TravelZou")
        end)
    end
})

Tabs.Config:AddToggle("Anti AFK",{
    Title = "Anti AFK",
    Default = true,
    Callback = function(Value)
        _G.AntiAFK = Value
    end
})

print("✅ SISTEMA UNIVERSAL DE EQUIP WEAPON CARREGADO!")
print("📌 O script vai equipar a arma automaticamente para TODAS as funções")

game.Players.LocalPlayer.Idled:Connect(function()
    if _G.AntiAFK then
        local mouse = game.Players.LocalPlayer:GetMouse()
        mouse:Button2Down()
        task.wait(1)
        mouse:Button2Up()
    end
end)

Tabs.Config:AddDropdown("WeaponDropdown", {
    Title = "Choose Weapon",
    Values = {"Melee", "Sword", "Fruit", "Gun"},
    Default = "Melee",
    Callback = function(value)
        _G.ChooseWP = value
    end
})

_G.ChooseWP = _G.ChooseWP or "Melee"

function _G.ChooseWP2()
    pcall(function()
        local plr = game.Players.LocalPlayer
        local char = plr.Character
        if not char then return end

        local equipped = char:FindFirstChildOfClass("Tool")
        if equipped and equipped.ToolTip == "Blox Fruit" then return end

        if not _G.ChooseWP then return end

        local tooltip = (_G.ChooseWP == "Fruit") and "Blox Fruit" or _G.ChooseWP

        for _, v in pairs(plr.Backpack:GetChildren()) do
            if v.ToolTip == tooltip then
                _G.SelectWeapon = v.Name
                char.Humanoid:EquipTool(v)
                break
            end
        end
    end)
end

Tabs.ShopTab:AddParagraph({
    Title = "Fighting Style",
    Content = "Compre estilos de luta abaixo"
})

-- ==============================================================
-- AUTO BUY ESTILOS DE LUTA - POSIÇÕES FIXAS OFICIAIS (COELHO HUB)
-- ==============================================================

-- Cache de Serviços e Configurações Globais
local Workspace = game:GetService("Workspace")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local plr = Players.LocalPlayer

_G.Settings = _G.Settings or {}
_G.Settings.Shop = _G.Settings.Shop or {}

-- BANCO DE DADOS DE CFRAMES (Atualizado com a sua coordenada exata do Shafi)
local NPC_POSITIONS = {
    ["DarkStep"]       = CFrame.new(-5048.45996, 371.354584, -3177.4939),
    ["Electro"]        = CFrame.new(-4994.93018, 314.557556, -3198.11987),
    ["WaterKungFu"]    = CFrame.new(-5019.89941, 371.354584, -3190.61987),
    ["DragonBreath"]   = CFrame.new(-4980.09473, 371.354584, -3206.14917), -- Sabi / Daermon
    ["Superhuman"]     = CFrame.new(-5005.6626,  374.42334,  -3195.02759),
    ["DeathStep"]      = CFrame.new(-5003.19482, 318.014496, -3222.10449),
    ["SharkmanKarate"] = CFrame.new(-4968.7417,  317.886932, -3219.92822),
    ["ElectricClaw"]   = CFrame.new(-10375.2676, 334.764008, -10132.6826),
    ["DragonTalon"]    = CFrame.new(5657.89844,  1214.87695, 863.23822),
    ["Godhuman"]       = CFrame.new(-13771.4043, 337.733002, -9876.94336),
    ["SanguineArt"]    = CFrame.new(-16511.1152, 26.8119965, -189.201233) -- Coordenada real do Shafi aplicada!
}

-- Função de movimentação por VOO CONTROLADO COM BARREIRA DE ALTURA (Y)
local function VoarParaPosicao(settingKey)
    local char = plr.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local cframeAlvo = NPC_POSITIONS[settingKey]

    if hrp and cframeAlvo then
        local velocidade = _G.VelocidadeFarmBone or 250
        local LIMITE_Y = 20 -- Defina aqui a altura mínima que o player pode alcançar (Impede Y negativo)
        
        if shouldTween ~= nil then shouldTween = true end
        
        -- Garante que a gravidade ou forças físicas não puxem o boneco para baixo
        hrp.AssemblyLinearVelocity = Vector3.new(0, 0, 0)

        -- CÁLCULO DO VOO SUAVE:
        local distancia = (hrp.Position - cframeAlvo.Position).Magnitude
        local deltaTime = task.wait() 
        local passo = (velocidade * deltaTime) / distancia
        
        if passo >= 1 then
            -- Se o alvo final tiver Y menor que o limite, barra ele no limite seguro
            local posFinal = cframeAlvo.Position
            if posFinal.Y < LIMITE_Y then
                posFinal = Vector3.new(posFinal.X, LIMITE_Y, posFinal.Z)
            end
            hrp.CFrame = CFrame.new(posFinal) * (cframeAlvo - cframeAlvo.Position)
            return true
        else
            -- Calcula o próximo CFrame do movimento suave
            local proximoCFrame = hrp.CFrame:Lerp(cframeAlvo, passo)
            local novaPosicao = proximoCFrame.Position
            
            -- BARREIRA INVISÍVEL: Se a próxima posição for menor que o limite, força o Y para cima
            if novaPosicao.Y < LIMITE_Y then
                novaPosicao = Vector3.new(novaPosicao.X, LIMITE_Y, novaPosicao.Z)
                -- Reconstrói o CFrame mantendo a rotação original, mas aplicando a trava no Y
                proximoCFrame = CFrame.new(novaPosicao) * (proximoCFrame - proximoCFrame.Position)
            end
            
            hrp.CFrame = proximoCFrame
        end
        
        if distancia < 15 then
            return true
        end
    end
    return false
end

-- Função centralizada que roda o loop de voo e compra sem travar
local function IniciarLoopEstilo(settingKey, buyCallback)
    task.spawn(function()
        -- Ativa o Noclip em segundo plano enquanto o toggle estiver ativo
        local noclipConnection
        noclipConnection = RunService.Stepped:Connect(function()
            if not _G.Settings.Shop[settingKey] then
                if noclipConnection then noclipConnection:Disconnect() end
                return
            end
            if plr.Character then
                for _, part in ipairs(plr.Character:GetDescendants()) do
                    if part:IsA("BasePart") then 
                        part.CanCollide = false 
                    end
                end
                
                -- CAMADA EXTRA DE SEGURANÇA NO NOCLIP:
                -- Se por algum motivo externo (ataque, bug) o boneco cair abaixo do limite, joga ele pra cima na hora
                local hrp = plr.Character:FindFirstChild("HumanoidRootPart")
                if hrp and hrp.Position.Y < 20 then
                    hrp.CFrame = CFrame.new(hrp.Position.X, 20, hrp.Position.Z) * (hrp.CFrame - hrp.CFrame.Position)
                end
            end
        end)

        -- Loop principal focado em voar
        while _G.Settings.Shop[settingKey] do
            RunService.Heartbeat:Wait()
            
            local chegouNoNpc = VoarParaPosicao(settingKey)
            
            if chegouNoNpc then
                task.spawn(function()
                    pcall(buyCallback)
                end)
                task.wait(0.5)
            end
        end
        
        if shouldTween ~= nil then shouldTween = false end
    end)
end

Tabs.ShopTab:AddToggle("ToggleBlackLeg", {
    Title = "Auto Dark Step (Black Leg)",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["DarkStep"] = Value
        if Value then
            IniciarLoopEstilo("DarkStep", function()
                replicated.Remotes.CommF_:InvokeServer("BuyBlackLeg")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleElectro", {
    Title = "Auto Electro",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["Electro"] = Value
        if Value then
            IniciarLoopEstilo("Electro", function()
                replicated.Remotes.CommF_:InvokeServer("BuyElectro")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleFishman", {
    Title = "Auto Water Kung Fu",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["WaterKungFu"] = Value
        if Value then
            IniciarLoopEstilo("WaterKungFu", function()
                replicated.Remotes.CommF_:InvokeServer("BuyFishmanKarate")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleDragonBreath", {
    Title = "Auto Dragon Breath",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["DragonBreath"] = Value
        if Value then
            IniciarLoopEstilo("DragonBreath", function()
                replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "1")
                replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "DragonClaw", "2")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleSuperhuman", {
    Title = "Auto Superhuman",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["Superhuman"] = Value
        if Value then
            IniciarLoopEstilo("Superhuman", function()
                replicated.Remotes.CommF_:InvokeServer("BuySuperhuman")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleDeathStep", {
    Title = "Auto Death Step",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["DeathStep"] = Value
        if Value then
            IniciarLoopEstilo("DeathStep", function()
                replicated.Remotes.CommF_:InvokeServer("BuyDeathStep")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleSharkman", {
    Title = "Auto Sharkman Karate",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["SharkmanKarate"] = Value
        if Value then
            IniciarLoopEstilo("SharkmanKarate", function()
                replicated.Remotes.CommF_:InvokeServer("BuySharkmanKarate", true)
                replicated.Remotes.CommF_:InvokeServer("BuySharkmanKarate")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleElectricClaw", {
    Title = "Auto Electric Claw",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["ElectricClaw"] = Value
        if Value then
            IniciarLoopEstilo("ElectricClaw", function()
                replicated.Remotes.CommF_:InvokeServer("BuyElectricClaw")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleDragonTalon", {
    Title = "Auto Dragon Talon",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["DragonTalon"] = Value
        if Value then
            IniciarLoopEstilo("DragonTalon", function()
                replicated.Remotes.CommF_:InvokeServer("BuyDragonTalon")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleGodhuman", {
    Title = "Auto Godhuman",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["Godhuman"] = Value
        if Value then
            IniciarLoopEstilo("Godhuman", function()
                replicated.Remotes.CommF_:InvokeServer("BuyGodhuman")
            end)
        end
    end
})

Tabs.ShopTab:AddToggle("ToggleSanguine", {
    Title = "Auto Sanguine Art",
    Default = false,
    Callback = function(Value)
        _G.Settings.Shop["SanguineArt"] = Value
        if Value then
            IniciarLoopEstilo("SanguineArt", function()
                replicated.Remotes.CommF_:InvokeServer("BuySanguineArt", true)
                replicated.Remotes.CommF_:InvokeServer("BuySanguineArt")
            end)
        end
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Abilities",
    Content = "Compre habilidades abaixo"
})

Tabs.ShopTab:AddButton({
    Title = "Skyjump [ 10,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Geppo") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buso Haki [ 25,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Buso") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Observation Haki [ 750,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("KenTalk", "Buy") end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Soru [ 100,000 Beli ]",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("BuyHaki", "Soru") end)
    end
})

Tabs.ShopTab:AddParagraph({
    Title = "Misc",
    Content = "Outros itens"
})

Tabs.ShopTab:AddButton({
    Title = "Buy Refund Stat (2500F)",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Refund", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Refund", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Reroll Race (3000F)",
    Callback = function()
        pcall(function()
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Reroll", "1")
            replicated.Remotes.CommF_:InvokeServer("BlackbeardReward", "Reroll", "2")
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Draco",
    Callback = function()
        pcall(function()
            local plr = game.Players.LocalPlayer
            local char = plr.Character
            _tp(CFrame.new(5814.42, 1208.32, 884.57))
            repeat task.wait() until (char.HumanoidRootPart.Position - Vector3.new(5814.42, 1208.32, 884.57)).Magnitude < 1
            game:GetService("ReplicatedStorage").Modules.Net:FindFirstChild("RF/InteractDragonQuest"):InvokeServer({
                ["NPC"] = "Dragon Wizard",
                ["Command"] = "DragonRace"
            })
        end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Ghoul Race",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("Ectoplasm", "Change", 4) end)
    end
})
Tabs.ShopTab:AddButton({
    Title = "Buy Cyborg Race (2500F)",
    Callback = function()
        pcall(function() replicated.Remotes.CommF_:InvokeServer("CyborgTrainer", "Buy") end)
    end
})

local SeaDois = {
    [79091703265657] = true,
    [996949360] = true,
}

Tabs.Others:AddToggle("AutoFactoryRaidToggle", {
    Title = "Auto Factory Raid",
    Default = false,
    Callback = function(Value)
        _G.AutoFactory = Value
        if Value then
            if not SeaDois[game.PlaceId] then
                _G.AutoFactory = false
                return
            end
            task.spawn(function()
                local FactoryDestino = CFrame.new(440.029694, 178.362595, -420.992828, 0.806989491, 4.07265439e-08, 0.590565801, -3.33742776e-08, 1, -2.33570123e-08, -0.590565801, -8.60842453e-10, 0.806989491)
                while _G.AutoFactory do
                    pcall(function()
                        local char = game.Players.LocalPlayer.Character
                        if not char then return end
                        local hrp = char:FindFirstChild("HumanoidRootPart")
                        if not hrp then return end
                        for _, part in pairs(char:GetDescendants()) do
                            if part:IsA("BasePart") then
                                part.CanCollide = false
                            end
                        end
                        local distancia = (hrp.Position - FactoryDestino.Position).Magnitude
                        if distancia > 5 then
                            local tempo = math.max(distancia / (_G.VelocidadeFarmBone or 350), 0.05)
                            game:GetService("TweenService"):Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = FactoryDestino}):Play()
                            task.wait(tempo)
                        else
                            hrp.CFrame = FactoryDestino
                            _G.ChooseWP2()
                        end
                    end)
                    task.wait(0.05)
                end
                local char = game.Players.LocalPlayer.Character
                if char then
                    for _, part in pairs(char:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = true
                        end
                    end
                end
            end)
        else
            local char = game.Players.LocalPlayer.Character
            if char then
                for _, part in pairs(char:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = true
                    end
                end
            end
        end
    end
})

-- 1. Deixe a variável de controle no topo do script (fora da toggle e do loop)
local Boud = false
local Sec = 1 -- Defina o tempo em segundos aqui

-- 2. O seu loop que executa a ação (ele fica rodando e checando a variável 'Boud')
task.spawn(function()
    while true do
        task.wait(Sec)
        if Boud then
            pcall(function()
                local I = { "HasBuso", "Buso" }
                if game.Players.LocalPlayer.Character and not game.Players.LocalPlayer.Character:FindFirstChild(I[1]) then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(I[2])
                end
            end)
        end
    end
end)

-- 3. A sua Toggle da UI (que controla o 'Boud')
Tabs.Config:AddToggle("AutoBusoHaki", {
    Title = "Auto turn Buso",
    Default = true,
    Callback = function(v)
        Boud = v
    end,
})

local ToggleFruit = Tabs.Fruit:AddToggle("ToggleGacha", {
    Title = "Random fruit", 
    Default = false,
    Callback = function(Value)
        _G.GachaAtivo = Value -- Usa uma variável global para controle
        
        if _G.GachaAtivo then
            -- Cria a thread para o loop não travar o resto do seu script
            task.spawn(function()
                local commF = game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_")
                local args = {"Cousin", "Buy", "DLCBoxData"}
                
                while _G.GachaAtivo do
                    pcall(function()
                        commF:InvokeServer(unpack(args))
                    end)
                    task.wait(1.0) -- Delay de segurança
                end
            end)
        end
    end
})

local ToggleAutoStore = Tabs.Fruit:AddToggle("AutoStoreFruit", {
    Title = "Auto Store Fruit",
    Description = "",
    Default = false,
    Callback = function(state)
        _G.AutoStoreFruitAtivo = state
    end
})

-- ==============================================================
-- MOTOR ULTRA OTIMIZADO DE ARMAZENAMENTO (Padrão Coelho Hub)
-- ==============================================================
task.spawn(function()
    local LocalPlayer = game.Players.LocalPlayer
    local CommF = game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_")

    while task.wait(0.5) do -- Meio segundo é perfeito para não floodar o servidor
        if _G.AutoStoreFruitAtivo then
            pcall(function()
                local backpack = LocalPlayer.Backpack
                if not backpack then return end

                for _, item in pairs(backpack:GetChildren()) do
                    -- Detecta se o item na mochila é uma fruta
                    if item:IsA("Tool") and (item.Name:find("Fruit") or item:FindFirstChild("FruitScript")) then
                        
                        -- Extrai o nome real da fruta (Ex: "Bomb Fruit" vira "Bomb")
                        local rawName = item.Name:gsub(" Fruit", ""):gsub("Fruta ", "")
                        
                        -- Monta o argumento exato que o servidor do Blox Fruits exige ("Bomb-Bomb")
                        local fruitID = rawName .. "-" .. rawName
                        
                        print("Coelho Hub guardando: " .. fruitID)
                        
                        -- Dispara o Remote original passando a ID formatada e o objeto real
                        CommF:InvokeServer("StoreFruit", fruitID, item)
                        
                        task.wait(0.2) -- Pequena pausa entre frutas para evitar anticheat
                    end
                end
            end)
        end
    end
end)

-- ==============================================================
-- TOGGLE NAS CONFIGS - COM PRECISÃO CIRÚRGICA
-- ==============================================================

local ToggleAntiNotif = Tabs.Config:AddToggle("AntiNotificationClear", {
    Title = "Anti Qualquer Notificação",
    Description = "Limpa spams de aviso e erros no meio da tela. Seguro para o HUD!",
    Default = false,
    Callback = function(state)
        _G.AntiNotificacaoGeral = state
    end
})

-- ==============================================================
-- O MATADOR DE SPAM (SÓ TEXTO CHATO)
-- ==============================================================
task.spawn(function()
    local LocalPlayer = game.Players.LocalPlayer
    local playerGui = LocalPlayer:WaitForChild("PlayerGui")

    local function limparElemento(v)
        if not _G.AntiNotificacaoGeral then return end
        
        -- 1. Se for o container de notificações flutuantes do Blox Fruits
        if v.Name == "NotificationElement" or v.Name == "Notifications" then
            v:Destroy()
        
        -- 2. Se for uma label de texto solta na interface principal que não seja do chat/HUD
        elseif v:IsA("TextLabel") and v.Visible then
            -- Só mata se o texto for aviso de armazenamento, erro ou texto solto no meio da tela
            if v.Text:find("store") or v.Text:find("only") or v.Text:find("limit") or v.Parent.Name == "Labels" then
                v:Destroy()
            end
        end
    end

    -- Loop de verificação contínua bem leve (Heartbeat)
    game:GetService("RunService").Heartbeat:Connect(function()
        if _G.AntiNotificacaoGeral then
            pcall(function()
                -- Limpa a pasta principal de notificações do jogo
                local notifContainer = playerGui:FindFirstChild("Notifications")
                if notifContainer then
                    notifContainer:ClearAllChildren()
                end
                
                -- Limpa as labels vermelhas soltas na Main sem quebrar o layout
                local mainGui = playerGui:FindFirstChild("Main")
                if mainGui then
                    for _, child in pairs(mainGui:GetChildren()) do
                        if child:IsA("TextLabel") and child.Visible then
                            -- Mata se for o texto de erro clássico do servidor
                            if child.Text:find("only store") or child.Text:find("Error") then
                                child:Destroy()
                            end
                        end
                    end
                end
            end)
        end
    end)

    -- Escuta para novas ameaças de texto
    playerGui.DescendantAdded:Connect(function(descendant)
        pcall(function()
            if _G.AntiNotificacaoGeral then
                limparElemento(descendant)
            end
        end)
    end)
end)

-- ==============================================================
-- SLIDER DE VELOCIDADE DO FARM NA ABA CONFIG
-- ==============================================================
_G.VelocidadeFarmBone = 350 -- Valor padrão inicial (equilibrado e seguro)

Tabs.Config:AddSlider("SliderVelocidadeBone", {
    Title = "Velocidade",
    Description = "Ajusta a velocidade do Tween entre os spots",
    Min = 100,
    Max = 500,
    Default = 350,
    Rounding = 0,
    Callback = function(Value)
        _G.VelocidadeFarmBone = Value
    end
})


-- ==============================================================
-- TOGGLE + MOTOR DO AUTO ACTIVE RACE (V3/V4)
-- ==============================================================

-- 1. O Botão para a sua Tab Main
local ToggleRaceAbility = Tabs.Config:AddToggle("AutoRaceAbilityToggle", {
    Title = "Auto Activate v3",
    Description = "",
    Default = false,
    Callback = function(Value)
        _G.RaceClickAutov3 = Value
    end
})

-- 2. O Motor que roda em segundo plano (Sem travar o clique)
task.spawn(function()
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    -- Busca o remote CommE de forma segura
    local CommE = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("CommE")
    
    local tempoEsperaMaximo = 30 -- Tempo de Cooldown da habilidade
    local tempoPassado = 0

    while task.wait(0.2) do
        if _G.RaceClickAutov3 then
            pcall(function()
                -- Dispara o remote oficial do jogo para ativar a raça
                CommE:FireServer("ActivateAbility")
                
                -- LOOP DE ESPERA INTELIGENTE:
                -- Em vez de dar um wait(30) seco, ele checa a cada 1 segundo se você desligou o botão
                tempoPassado = 0
                repeat
                    task.wait(1)
                    tempoPassado = tempoPassado + 1
                until not _G.RaceClickAutov3 or tempoPassado >= tempoEsperaMaximo
            end)
        end
    end
end)

-- ==============================================================
-- BOTÃO DE RESGATAR TODOS OS CÓDIGOS (ABA CONFIG / MISC)
-- ==============================================================
Tabs.ShopTab:AddButton({
    Title = "Redeem All Codes",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        
        -- Lista de códigos atualizada e limpa de duplicatas inúteis
        local Codes = {
            "REWARDMAN", "NEWTROLL", "KITT_RESET", "Sub2CaptainMaui", "DEVSCOOKING",
            "SUB2GAMERROBOT_RESET1", "sub2gamerrobot_exp1", "Sub2OfficialNoobie",
            "THEGREATACE", "SUB2NOOBMASTER123", "SUB2DAIGROCK", "AXIORE",
            "TANTAIGAMING", "STRAWHATMAINE", "BLUXXY", "FUDD10", "FUDD10_V2",
            "BIGNREWS", "SUB2UNCLEKIZARU", "ENYU_IS_PRO", "MAGICBUS", "JCWK",
            "STARCODEHEO", "KITTGAMING", "CHANDLER"
        }
        
        -- Caminho oficial e atualizado do Remote de códigos do Blox Fruits
        local CommF = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("CommF_")
        
        if not CommF then
            -- Fallback de aviso caso a biblioteca de notificação não esteja indexada como 'bearlib'
            print("Coelho Hub: Erro - Sistema de Remotes não encontrado.")
            return
        end
        
        print("Coelho Hub: Iniciando resgate automatizado de códigos...")
        
        for _, code in ipairs(Codes) do
            pcall(function()
                -- O Blox Fruits usa InvokeServer no CommF_ passando o argumento "RedeemCode"
                CommF:InvokeServer("RedeemCode", code)
            end)
            -- Delay de proteção de 0.5 segundos por código para o servidor processar sem dar lag ou kick
            task.wait(0.5) 
        end
        
        print("Coelho Hub: Todos os códigos disponíveis foram processados!")
        
        -- Se sua biblioteca de UI for do estilo Rayfield/Orion, você pode descomentar a linha abaixo para mandar um aviso visual:
        -- game:GetService("StarterGui"):SetCore("SendNotification", {Title = "Coelho Hub", Text = "Todos os códigos foram processados!", Duration = 5})
    end
})

-- =============================================
-- TOGGLE: Cole na sua aba Main
-- =============================================

-- ==============================================================
-- AUTO FARM ELITE HUNTER (LÓGICA UNIVERSAL DE 5 STUDS ACIMA)
-- ==============================================================

-- ==============================================================
-- MOTOR ELITE HUNTER - TRAVA TOTAL SE NÃO EXISTIR BOSS (VISUAL LIMPO)
-- ==============================================================

_G.FarmEliteHunt = false
_G.VelocidadeFarmBone = _G.VelocidadeFarmBone or 350

local function IrAteNPCSeguro(nomeDoNPC)
    local Players = game:GetService("Players")
    local Workspace = game:GetService("Workspace")
    local TweenService = game:GetService("TweenService")
    
    local plr = Players.LocalPlayer
    local char = plr.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if not hrp then return false end

    local npcAlvo = Workspace:FindFirstChild("Enemies") and Workspace.Enemies:FindFirstChild(nomeDoNPC) or Workspace:FindFirstChild(nomeDoNPC)

    if npcAlvo and npcAlvo:FindFirstChild("Humanoid") and npcAlvo.Humanoid.Health > 0 and npcAlvo:FindFirstChild("HumanoidRootPart") then
        local destinoSeguro = npcAlvo.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
        local distancia = (hrp.Position - destinoSeguro.Position).Magnitude
        local velocidade = _G.VelocidadeFarmBone or 350
        local tempo = math.max(distancia / velocidade, 0.05)

        local tween = TweenService:Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = destinoSeguro})
        tween:Play()

        if distancia < 12 then
            tween:Cancel()
            hrp.CFrame = destinoSeguro
        end
        return true
    end
    return false
end

local ToggleEliteHunt = Tabs.Main:AddToggle("EliteHuntToggle", {
    Title = "Auto Farm Elite Hunter",
    Description = "Killed: 0",
    Default = false,
    Callback = function(Value)
        _G.FarmEliteHunt = Value
        if not Value then
            local char = game.Players.LocalPlayer.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if hrp then
                game:GetService("TweenService"):Create(hrp, TweenInfo.new(0.1), {CFrame = hrp.CFrame}):Play()
            end
        end
    end
})

local _G_StatusBossDisponivel = false
game:GetService("RunService").Stepped:Connect(function()
    if _G.FarmEliteHunt and _G_StatusBossDisponivel and game.Players.LocalPlayer.Character then
        for _, part in ipairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

task.spawn(function()
    local plr = game:GetService("Players").LocalPlayer
    local replicated = game:GetService("ReplicatedStorage")
    local workspace = game:GetService("Workspace")
    local TweenService = game:GetService("TweenService")

    while task.wait(0.5) do
        if _G.FarmEliteHunt then
            local eliteFound = false
            
            pcall(function()
                local progress = replicated.Remotes.CommF_:InvokeServer("EliteHunter", "Progress")
                ToggleEliteHunt:SetDesc("Killed: " .. tostring(progress))
                
                local bossNoReplicated = replicated:FindFirstChild("Diablo") or replicated:FindFirstChild("Deandre") or replicated:FindFirstChild("Urban")
                local bossNoWorkspace = workspace:FindFirstChild("Enemies") and (workspace.Enemies:FindFirstChild("Diablo") or workspace.Enemies:FindFirstChild("Deandre") or workspace.Enemies:FindFirstChild("Urban"))
                
                if bossNoReplicated or bossNoWorkspace then
                    eliteFound = true
                end
            end)

            _G_StatusBossDisponivel = eliteFound

            if not eliteFound then
                pcall(function()
                    local char = plr.Character
                    local hrp = char and char:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        TweenService:Create(hrp, TweenInfo.new(0.1), {CFrame = hrp.CFrame}):Play()
                    end
                end)
                continue
            end

            pcall(function()
                local char = plr.Character
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                if not hrp then return end

                local questUI = plr.PlayerGui.Main.Quest

                if questUI and questUI.Visible == true then
                    local titulo = questUI.Container.QuestTitle.Title.Text
                    if string.find(titulo, "Diablo") or string.find(titulo, "Urban") or string.find(titulo, "Deandre") then
                        
                        _G.ChooseWP2()
                        local cacando = IrAteNPCSeguro("Diablo") or IrAteNPCSeguro("Urban") or IrAteNPCSeguro("Deandre")
                        
                        if not cacando then
                            for _, v in pairs(replicated:GetChildren()) do
                                if string.find(v.Name, "Diablo") or string.find(v.Name, "Urban") or string.find(v.Name, "Deandre") then
                                    if v:FindFirstChild("HumanoidRootPart") then
                                        _G.ChooseWP2()
                                        local destinoSeguro = v.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                                        local distancia = (hrp.Position - destinoSeguro.Position).Magnitude
                                        local tempo = math.max(distancia / (_G.VelocidadeFarmBone or 350), 0.05)
                                        TweenService:Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = destinoSeguro}):Play()
                                    end
                                end
                            end
                        end
                    end
                else
                    local coordenadaNPCSegura = CFrame.new(-5417.66, 313.06 + 5, -2822.91)
                    local distancia = (hrp.Position - coordenadaNPCSegura.Position).Magnitude

                    if distancia > 15 then
                        local tempo = math.max(distancia / (_G.VelocidadeFarmBone or 350), 0.1)
                        TweenService:Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = coordenadaNPCSegura}):Play()
                    else
                        TweenService:Create(hrp, TweenInfo.new(0.1, Enum.EasingStyle.Linear), {CFrame = coordenadaNPCSegura}):Play()
                        pcall(function()
                            local remote = replicated:WaitForChild("Remotes"):WaitForChild("CommF_")
                            remote:InvokeServer("EliteHunter")
                            task.wait(0.3)
                            remote:InvokeServer("EliteHunter", "Progress")
                        end)
                        task.wait(1.5)
                    end
                end
            end)
        end
    end
end)

-- Declaração da imagem
local BannerCreditos = nil

local function criarImagem()
    if BannerCreditos then return end
    
    local screenGui = game.Players.LocalPlayer.PlayerGui:FindFirstChild("Fluent")
    if not screenGui then return end

    BannerCreditos = Instance.new("ImageLabel")
    BannerCreditos.Image = "rbxassetid://94453919385793"
    BannerCreditos.Size = UDim2.new(0, 200, 0, 200)
    BannerCreditos.BackgroundTransparency = 1
    BannerCreditos.ZIndex = 999
    BannerCreditos.Parent = screenGui
end

local function destruirImagem()
    if BannerCreditos then
        BannerCreditos:Destroy()
        BannerCreditos = nil
    end
end

-- Posição da imagem acompanha o parágrafo de texto
task.spawn(function()
    while task.wait(0.1) do
        pcall(function()
            local screenGui = game.Players.LocalPlayer.PlayerGui:FindFirstChild("Fluent")
            if not screenGui then return end

            -- Verifica se a aba Creditos está visível
            local creditosAberto = false
            for _, v in ipairs(screenGui:GetDescendants()) do
                if v:IsA("Frame") and v.Visible and v.Name:lower():find("credito") then
                    creditosAberto = true
                    break
                end
            end

            if creditosAberto then
                criarImagem()

                -- Acompanha a posição do parágrafo
                if BannerCreditos then
                    for _, v in ipairs(screenGui:GetDescendants()) do
                        if v:IsA("TextLabel") and v.Text:find("Visite nosso server") then
                            local pos = v.AbsolutePosition
                            BannerCreditos.Position = UDim2.new(0, pos.X, 0, pos.Y - 210)
                            break
                        end
                    end
                end
            else
                destruirImagem()
            end
        end)
    end
end)

-- Creditos
Tabs.Creditos:AddParagraph({
    Title = "Discord",
    Content = "Visite nosso server"
})

Tabs.Creditos:AddButton({
    Title = "Copiar Link do Server",
    Description = "",
    Callback = function()
        setclipboard("https://discord.gg/tdSwHMhqZ")
        print("Coelho Hub: Link copiado!")
    end
})

-- ==============================================================
-- AUTO FARM CHEST - VERSÃO ULTRA EFICIENTE (COELHO HUB)
-- ==============================================================

local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")

local plr = Players.LocalPlayer

_G.Settings = _G.Settings or {}
_G.Settings.Farm = _G.Settings.Farm or {}
_G.Settings.Farm["Auto Farm Chest Tween"] = false
_G.ChestHopCount = _G.ChestHopCount or 0

local function TweenPlayer(TargetCFrame)
    local char = plr.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local distancia = (hrp.Position - TargetCFrame.Position).Magnitude
    if distancia < 5 then
        hrp.CFrame = TargetCFrame
        return
    end

    local velocidadeAtual = _G.VelocidadeFarmBone
    if type(velocidadeAtual) == "function" then velocidadeAtual = 350 end
    if not velocidadeAtual or velocidadeAtual <= 0 then velocidadeAtual = 350 end

    local tempoCalculado = distancia / velocidadeAtual
    local tween = TweenService:Create(hrp, TweenInfo.new(tempoCalculado, Enum.EasingStyle.Linear), {CFrame = TargetCFrame})
    tween:Play()

    repeat
        RunService.Heartbeat:Wait()
    until (hrp.Position - TargetCFrame.Position).Magnitude < 2 or not _G.Settings.Farm["Auto Farm Chest Tween"]

    if not _G.Settings.Farm["Auto Farm Chest Tween"] then
        tween:Cancel()
    end
end

local function _isSpecialChestItem()
    local character = plr.Character
    local backpack = plr:FindFirstChild("Backpack")
    if character and backpack then
        if character:FindFirstChild("Fist of Darkness") or backpack:FindFirstChild("Fist of Darkness") or
           character:FindFirstChild("Gods Chalice") or backpack:FindFirstChild("Gods Chalice") then
            return true
        end
    end
    return false
end

Tabs.Main:AddToggle("ToggleAutoChestOriginal", {
    Title = "Auto Farm Chest",
    Description = "",
    Default = false,
    Callback = function(Value)
        _G.Settings.Farm["Auto Farm Chest Tween"] = Value
        if not Value then
            local char = plr.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if hrp then
                TweenService:Create(hrp, TweenInfo.new(0.1), {CFrame = hrp.CFrame}):Play()
            end
        end
    end
})

RunService.Stepped:Connect(function()
    if _G.Settings.Farm["Auto Farm Chest Tween"] and plr.Character then
        for _, part in ipairs(plr.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

task.spawn(function()
    local chestModels = Workspace:WaitForChild("ChestModels", 5)

    while true do
        task.wait(0)

        if _G.Settings.Farm["Auto Farm Chest Tween"] then
            pcall(function()
                local char = plr.Character
                local hrp = char and char:FindFirstChild("HumanoidRootPart")
                if not hrp then return end

                if _isSpecialChestItem() then
                    _G.Settings.Farm["Auto Farm Chest Tween"] = false
                    StarterGui:SetCore("SendNotification", {
                        Title = "Coelho Hub",
                        Text = "Item especial encontrado! Farm de Chest parado por segurança.",
                        Duration = 6
                    })
                    return
                end

                chestModels = Workspace:FindFirstChild("ChestModels")
                if chestModels then
                    local chests = chestModels:GetChildren()

                    if #chests > 0 then
                        for i = 1, #chests do
                            if not _G.Settings.Farm["Auto Farm Chest Tween"] or _isSpecialChestItem() then break end

                            local v = chests[i]
                            if v and v.Parent and v.Name:find("Chest") and v:FindFirstChild("RootPart") then
                                local root = v.RootPart

                                -- voa até o baú
                                TweenPlayer(root.CFrame * CFrame.new(0, 2, 0))

                                -- depois faz o bypass de colisão
                                repeat
                                    RunService.Heartbeat:Wait()
                                    firetouchinterest(hrp, root, 0)
                                    firetouchinterest(hrp, root, 1)
                                until not _G.Settings.Farm["Auto Farm Chest Tween"] or not v.Parent or _isSpecialChestItem()

                                _G.ChestHopCount = _G.ChestHopCount + 1
                            end
                        end
                    else
                        task.wait(0.1)
                    end
                else
                    task.wait(0.3)
                end
            end)
        else
            task.wait(0.3)
        end
    end
end)

-- ==============================================================
-- AUTO FARM FRUIT - VERSÃO ULTRA EFICIENTE (COELHO HUB)
-- ==============================================================

-- Cache de Serviços e Variáveis Globais (Otimização de Memória)
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local plr = Players.LocalPlayer

_G.Settings = _G.Settings or {}
_G.Settings.Farm = _G.Settings.Farm or {}
_G.Settings.Farm["Auto Farm Fruit Tween"] = false
_G.VelocidadeFarmBone = _G.VelocidadeFarmBone or 350 

-- 1. FUNÇÃO TWEENPLAYER OPTIMIZADA (Idêntica ao seu padrão de Baús)
local function TweenPlayer(TargetCFrame)
    local char = plr.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local distancia = (hrp.Position - TargetCFrame.Position).Magnitude
    
    if distancia < 5 then
        hrp.CFrame = TargetCFrame
        return
    end

    local velocidadeAtual = _G.VelocidadeFarmBone
    if velocidadeAtual <= 0 then velocidadeAtual = 1 end 
    
    local tempoCalculado = distancia / velocidadeAtual
    local tween = TweenService:Create(hrp, TweenInfo.new(tempoCalculado, Enum.EasingStyle.Linear), {CFrame = TargetCFrame})
    tween:Play()
    
    -- Aguarda o término do movimento de forma eficiente
    repeat 
        RunService.Heartbeat:Wait() 
    until (hrp.Position - TargetCFrame.Position).Magnitude < 2 or not _G.Settings.Farm["Auto Farm Fruit Tween"]
    
    if not _G.Settings.Farm["Auto Farm Fruit Tween"] then
        tween:Cancel()
    end
end

-- 2. O BOTÃO DA INTERFACE
Tabs.Fruit:AddToggle("ToggleAutoFruitOriginal", {
    Title = "Auto colect Fruit",
    Description = "",
    Default = false,
    Callback = function(Value)
        _G.Settings.Farm["Auto Farm Fruit Tween"] = Value
        if not Value then
            local char = plr.Character
            local hrp = char and char:FindFirstChild("HumanoidRootPart")
            if hrp then
                TweenService:Create(hrp, TweenInfo.new(0.1), {CFrame = hrp.CFrame}):Play()
            end
        end
    end
})

-- 3. LOOP DO NOCLIP BRUTO DE ALTA PERFORMANCE
RunService.Stepped:Connect(function()
    if _G.Settings.Farm["Auto Farm Fruit Tween"] and plr.Character then
        for _, part in ipairs(plr.Character:GetDescendants()) do
            if part:IsA("BasePart") and part.CanCollide then
                part.CanCollide = false
            end
        end
    end
end)

-- 4. MOTOR PRINCIPAL - VAI ATÉ A FRUTA E LIMPA O SERVER
task.spawn(function()
    while true do
        if _G.Settings.Farm["Auto Farm Fruit Tween"] then
            local items = Workspace:GetChildren()
            local frutaEncontradaNoTurno = false
            
            -- Varre o Workspace usando o seu loop numérico indexado de alta performance
            for i = 1, #items do
                if not _G.Settings.Farm["Auto Farm Fruit Tween"] then break end
                
                local v = items[i]
                local handle = v and v:FindFirstChild("Handle")
                
                -- Se encontrar uma fruta no mapa, vai até ela (Tween) por ordem de aparição
                if handle and v.Name:find("Fruit") then
                    frutaEncontradaNoTurno = true
                    
                    -- Fica preso na fruta atual indo até ela e pegando até que ela suma do mapa
                    repeat
                        RunService.Heartbeat:Wait()
                        TweenPlayer(handle.CFrame)
                        
                        local char = plr.Character
                        local hrp = char and char:FindFirstChild("HumanoidRootPart")
                        
                        if hrp and handle then
                            -- Força a coleta por toque físico direto na fruta
                            firetouchinterest(hrp, handle, 0)
                            firetouchinterest(hrp, handle, 1)
                        end
                    -- Só passa para a próxima fruta da lista quando a atual sumir (.Parent virar nil)
                    until not _G.Settings.Farm["Auto Farm Fruit Tween"] or not v.Parent
                end
            end
            
            -- Se varreu o servidor inteiro e não achou mais nenhuma fruta, espera antes de checar de novo
            if not frutaEncontradaNoTurno then
                task.wait(1) -- Delay inteligente para economizar CPU quando o server estiver limpo
            end
        else
            task.wait(0.5) -- Delay padrão quando o botão está desligado
        end
    end
end)

_G.AutoBuyBones = false -- Começa desligado

-- TOGGLE PARA COMPRAR COM BONES
Tabs.Main:AddToggle("AutoBuyBonesToggle", {
    Title = "random bones",
    Default = false,
    Callback = function(Value)
        _G.AutoBuyBones = Value
        
        if Value then
            task.spawn(function()
                while _G.AutoBuyBones do
                    pcall(function()
                        -- Executa o remote enviado com os seus argumentos
                        local args = {
                            "Bones",
                            "Buy",
                            1,
                            1
                        }
                        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_"):InvokeServer(unpack(args))
                    end)
                    task.wait(0.5) -- Intervalo entre cada compra (ajuste se quiser mais rápido ou mais lento)
                end
            end)
        end
    end
})

_G.BringMobRadius = 300 -- Começa em 300 (Desativado)

-- 1. O SLIDER NA ABA CONFIG
Tabs.Config:AddSlider("BringMobRadiusSlider", {
    Title = "Bring Mob Distance",
    Description = "bring mob",
    Min = 300,
    Max = 500,
    Default = 300,
    Rounding = 0,
    Callback = function(Value)
        _G.BringMobRadius = Value
    end
})

-- 2. A FUNÇÃO ISOLADA COM AJUSTE DE ALTURA (-3 STUDS)
_G.BringMobFuncion = function(alvoPrincipal)
    -- Só executa se o slider estiver acima de 300
    if not _G.BringMobRadius or _G.BringMobRadius <= 300 then return end
    
    local character = game.Players.LocalPlayer.Character
    local hrp = character and character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end
    
    -- Varre a pasta de inimigos
    for _, mob in pairs(workspace.Enemies:GetChildren()) do
        local mobHrp = mob:FindFirstChild("HumanoidRootPart")
        local mobHumanoid = mob:FindFirstChildOfClass("Humanoid")
        
        -- Verifica se o mob está vivo e ignora o alvo principal do voo
        if mobHrp and mobHumanoid and mobHumanoid.Health > 0 and mob ~= alvoPrincipal then
            local distanciaMob = (hrp.Position - mobHrp.Position).Magnitude
            
            -- Se estiver dentro do raio definido no Slider, puxa exatamente 3 studs abaixo
            if distanciaMob <= _G.BringMobRadius then
                -- O CFrame pega a sua posição e subtrai 3 studs apenas na altura (eixo Y)
            mobHrp.CFrame = CFrame.new(hrp.Position + Vector3.new(0, -3, 0))
            end
        end
    end
end

repeat wait() until game:IsLoaded()

local PlaceIds = {
    [2753915549] = "World 1",
    [4442272183] = "World 2",
    [7449423635] = "World 3 ",
    [79091703265657] = "Sea 2",
    [996949360] = "Sea 2",
    [100117331123089] = "World 3",
}

local CurrentWorld = PlaceIds[game.PlaceId] or "Desconhecido"

Tabs.Status:AddParagraph({
    Title = "Mundo Atual",
    Content = CurrentWorld
})

_G.BaitAmount = 10
_G.SelectedBait = "Basic Bait"
_G.AutoCraftBait = false

local Sea2Ids = {[79091703265657] = true, [996949360] = true}
local Sea3Ids = {[7449423635] = true, [100117331123089] = true}

local function GetAnglerCFrame()
    local placeId = game.PlaceId
    if Sea2Ids[placeId] then
        return CFrame.new(-1942.65723, 6.4788909, -2605.2312, 0.596867919, -3.44649553e-09, -0.802339554, -2.59415778e-08, 1, -2.35937403e-08, 0.802339554, 3.48962992e-08, 0.596867919)
    elseif Sea3Ids[placeId] then
        return CFrame.new(-16201.0859, 9.70211792, 441.266724, 0.190850228, -3.35462218e-08, -0.981619179, 1.69887677e-08, 1, -3.08713517e-08, 0.981619179, -1.07846958e-08, 0.190850228)
    else
        return CFrame.new(26.5670776, 10.6520329, 5344.48633, -0.961029828, 1.24920945e-08, -0.276444763, 2.37652227e-08, 1, -3.74287907e-08, 0.276444763, -4.2539952e-08, -0.961029828)
    end
end

Tabs.Others:AddDropdown("BaitSelector", {
    Title = "Select Bait Type",
    Values = {"Basic Bait", "Abyssal Bait"},
    Default = "Basic Bait",
    Callback = function(Value)
        _G.SelectedBait = Value
    end
})

Tabs.Others:AddToggle("AutoCraftBait", {
    Title = "Craft Bait",
    Default = false,
    Callback = function(Value)
        _G.AutoCraftBait = Value
        if Value then
            task.spawn(function()
                while _G.AutoCraftBait do
                    pcall(function()
                        if not _G.AutoCraftBait then return end
                        local hrp = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                        if not hrp then return end

                        local destino = GetAnglerCFrame()
                        local distancia = (hrp.Position - destino.Position).Magnitude
                        local tempo = math.max(distancia / (_G.VelocidadeFarmBone or 350), 0.05)
                        game:GetService("TweenService"):Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = destino}):Play()
                        task.wait(tempo + 0.3)

                        if not _G.AutoCraftBait then return end

                        local args = {"Craft", _G.SelectedBait, _G.BaitAmount, {}}
                        game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("Net"):WaitForChild("RF/Craft"):InvokeServer(unpack(args))
                    end)
                    task.wait(1)
                end
            end)
        end
    end
})

Tabs.Others:AddDropdown("BaitAmountDropdown", {
    Title = "Quantidade de Bait",
    Values = {"10", "20", "30", "40", "50"},
    Default = "10",
    Callback = function(Value)
        _G.BaitAmount = tonumber(Value)
    end
})

local Players = game:GetService("Players")

local playerList = {}

local function getPlayerNames()
    local names = {}
    for _, p in ipairs(Players:GetPlayers()) do
        table.insert(names, p.Name)
    end
    return names
end

local selectedPlayer = nil

local PlayerDropdown = Tabs.PvpTab:AddDropdown("PlayerDropdown", {
    Title = "Jogadores",
    Values = getPlayerNames(),
    Default = nil,
    Callback = function(Value)
        selectedPlayer = Value
    end
})

Tabs.PvpTab:AddButton({
    Title = "Refresh",
    Callback = function()
        PlayerDropdown:SetValues(getPlayerNames())
    end
})

_G.Test = false

local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local CFrameEspera = CFrame.new(-9502.07227, 501.75415, 6012.94678, 0.0197974183, 0.000531507714, 0.999803841, 0.00015926265, 0.999999821, -0.000534765481, -0.99980402, 0.000169818391, 0.0197973307)

local Sea3Ids = {
    [7449423635] = true,
    [100117331123089] = true,
}

local indexList = {3, 4, 6, 7, 8, 11, 12, 14, 15, 16, 17, 18, 19, 20, 22}

local function getNPCs()
    local vivos = {}
    local enemies = workspace.Enemies

    local namedMobs = {"Reborn Skeleton", "Posessed Mummy", "Living Zombie"}
    for _, name in ipairs(namedMobs) do
        local mob = enemies:FindFirstChild(name)
        if mob then
            local mobHrp = mob:FindFirstChild("HumanoidRootPart")
            local mobHumanoid = mob:FindFirstChildOfClass("Humanoid")
            if mobHrp and mobHumanoid and mobHumanoid.Health > 0 then
                table.insert(vivos, mob)
            end
        end
    end

    for _, index in ipairs(indexList) do
        local mob = enemies:GetChildren()[index]
        if mob then
            local mobHrp = mob:FindFirstChild("HumanoidRootPart")
            local mobHumanoid = mob:FindFirstChildOfClass("Humanoid")
            if mobHrp and mobHumanoid and mobHumanoid.Health > 0 then
                table.insert(vivos, mob)
            end
        end
    end

    return vivos
end

local function voarAte(hrp, posicaoAlvo)
    local distancia = (hrp.Position - posicaoAlvo).Magnitude
    local velocidade = (_G.VelocidadeFarmBone and _G.VelocidadeFarmBone > 0) and _G.VelocidadeFarmBone or 350
    local duracao = distancia / velocidade
    local tween = TweenService:Create(hrp, TweenInfo.new(duracao, Enum.EasingStyle.Linear), {CFrame = CFrame.new(posicaoAlvo)})
    tween:Play()
    tween.Completed:Wait()
end

Tabs.Main:AddToggle("Test", {
    Title = "Farm Bone",
    Default = false,
    Callback = function(Value)
        _G.Test = Value

        if Value then
            task.spawn(function()
                if not Sea3Ids[game.PlaceId] then
                    _G.Test = false
                    return
                end

                local character = LocalPlayer.Character
                local hrp = character and character:FindFirstChild("HumanoidRootPart")
                if not hrp then return end

                voarAte(hrp, CFrameEspera.Position)

                while _G.Test do
                    pcall(function()
                        character = LocalPlayer.Character
                        hrp = character and character:FindFirstChild("HumanoidRootPart")
                        if not hrp then return end

                        local lista = getNPCs()

                        if #lista == 0 then
                            task.wait(0.5)
                            return
                        end

                        for _, inimigo in ipairs(lista) do
                            if not _G.Test then break end

                            local mobHrp = inimigo:FindFirstChild("HumanoidRootPart")
                            local mobHumanoid = inimigo:FindFirstChildOfClass("Humanoid")

                            if mobHrp and mobHumanoid and mobHumanoid.Health > 0 then
                                _G.ChooseWP2()
                                voarAte(hrp, mobHrp.Position + Vector3.new(0, 3, 0))

                                while _G.Test and inimigo.Parent and mobHumanoid and mobHumanoid.Health > 0 do
                                    character = LocalPlayer.Character
                                    hrp = character and character:FindFirstChild("HumanoidRootPart")
                                    if not hrp then break end
                                    _G.ChooseWP2()
                                    voarAte(hrp, mobHrp.Position + Vector3.new(0, 10, 0))
                                end
                            end
                        end
                    end)
                end
            end)
        end
    end
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

_G.TestVoarCakePrince = false

-- =====================================================================
-- FUNÇÃO DE VOO COMPATÍVEL COM ANTI-CHEAT (COMPLETA SEM CORTES)
-- =====================================================================
local function voarFisicoAntiCheat(hrp, posicaoAlvo, humanoid)
    -- Evita que o anti-cheat detecte o estado de "Falling" ou "Freefall"
    humanoid:ChangeState(Enum.HumanoidStateType.Physics)
    
    -- Cria uma força de velocidade linear nativa se ela não existir
    local bv = hrp:FindFirstChild("AntiCheatFlyForce")
    if not bv then
        bv = Instance.new("BodyVelocity")
        bv.Name = "AntiCheatFlyForce"
        bv.MaxForce = Vector3.new(9e9, 9e9, 9e9) -- Força total contra a gravidade
        bv.Parent = hrp
    end
    
    -- Mantém o loop de movimentação suave atualizando frame por frame
    while _G.TestVoarCakePrince and hrp and hrp.Parent and (hrp.Position - posicaoAlvo).Magnitude > 3 do
        local distanciaVector = (posicaoAlvo - hrp.Position)
        local direcao = distanciaVector.Unit
        local distancia = distanciaVector.Magnitude
        
        -- Puxa o controle do seu Slider principal de velocidade
        local velocidadeMax = (_G.VelocidadeFarmBone and _G.VelocidadeFarmBone > 0) and _G.VelocidadeFarmBone or 300
        
        -- OTIMIZAÇÃO ANTI-CHEAT: Reduz a velocidade na chegada para não dar tranco
        local velocidadeAtual = distancia < 15 and (velocidadeMax * 0.4) or velocidadeMax
        
        -- Aplica a velocidade fisicamente na direção correta
        bv.Velocity = direcao * velocidadeAtual
        
        -- Faz o personagem olhar fixamente para o alvo (evita giros doidos)
        hrp.CFrame = CFrame.lookAt(hrp.Position, Vector3.new(posicaoAlvo.X, hrp.Position.Y, posicaoAlvo.Z))
        
        RunService.Heartbeat:Wait()
    end
end

-- =====================================================================
-- TOGGLE NA ABA STACK (Ajustado e Limpo)
-- =====================================================================
Tabs.Stack:AddToggle("TestVoarCakePrince", {
    Title = "kill cake prince",
    Default = false,
    Callback = function(Value)
        _G.TestVoarCakePrince = Value

        if Value then
            task.spawn(function()
                while _G.TestVoarCakePrince do
                    pcall(function()
                        local character = LocalPlayer.Character
                        local hrp = character and character:FindFirstChild("HumanoidRootPart")
                        local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                        
                        if not hrp or not humanoid or humanoid.Health <= 0 then 
                            task.wait(0.5)
                            return 
                        end

                        -- Escaneamento de localização do Boss
                        local boss = workspace:FindFirstChild("Enemies") and workspace.Enemies:FindFirstChild("Cake Prince") or workspace:FindFirstChild("Cake Prince")

                        if not boss then
                            -- Se não achar o boss, limpa a força física para não bugar
                            local bv = hrp:FindFirstChild("AntiCheatFlyForce")
                            if bv then bv:Destroy() end
                            task.wait(0.5)
                            return
                        end

                        local bossHrp = boss:FindFirstChild("HumanoidRootPart")
                        local bossHumanoid = boss:FindFirstChildOfClass("Humanoid")

                        if bossHrp and bossHumanoid and bossHumanoid.Health > 0 then
                            -- Equip Weapon nativo (Fica forçando o clique/equip)
                            if type(_G.ChooseWP2) == "function" and not character:FindFirstChildOfClass("Tool") then
                                _G.ChooseWP2()
                            end

                            -- Calcula a posição de ataque seguro (3 studs acima do alvo)
                            local posicaoAlvo = bossHrp.Position + Vector3.new(0, 3, 0)
                            
                            -- Inicia o voo físico suave bypassando o anti-cheat
                            voarFisicoAntiCheat(hrp, posicaoAlvo, humanoid)
                        end
                    end)
                    task.wait(0.05)
                end
                
                -- Limpeza absoluta ao desligar a Toggle manual
                pcall(function()
                    local character = LocalPlayer.Character
                    local hrp = character and character:FindFirstChild("HumanoidRootPart")
                    local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                    
                    if hrp and hrp:FindFirstChild("AntiCheatFlyForce") then
                        hrp.AntiCheatFlyForce:Destroy()
                    end
                    if humanoid then
                        humanoid:ChangeState(Enum.HumanoidStateType.Standing)
                    end
                end)
            end)
        end
    end
})

_G.TeleportKitsune = false

local TweenService = game:GetService("TweenService")

local function voarAte(hrp, posicaoAlvo)
    local distancia = (hrp.Position - posicaoAlvo).Magnitude
    if distancia < 2 then return end
    local velocidade = (_G.VelocidadeFarmBone and _G.VelocidadeFarmBone > 0) and _G.VelocidadeFarmBone or 350
    local duracao = distancia / velocidade
    local tween = TweenService:Create(hrp, TweenInfo.new(duracao, Enum.EasingStyle.Linear), {CFrame = CFrame.new(posicaoAlvo)})
    tween:Play()
    local timeout = tick() + duracao + 1
    repeat
        task.wait(math.random(1, 3) / 10)
    until (hrp.Position - posicaoAlvo).Magnitude < 3 or tick() > timeout or not _G.TeleportKitsune
end

Tabs.Stack:AddToggle("TeleportKitsune", {
    Title = "Teleport Kitsune Island",
    Default = false,
    Callback = function(Value)
        _G.TeleportKitsune = Value

        if Value then
            task.spawn(function()
                local character = game.Players.LocalPlayer.Character
                local hrp = character and character:FindFirstChild("HumanoidRootPart")
                if not hrp then return end

                local destino = workspace._WorldOrigin.Locations["Kitsune Island"]
                if not destino then return end

                -- cooldown aleatório antes de começar o voo
                task.wait(math.random(5, 15) / 10)

                _G.ChooseWP2()
                voarAte(hrp, destino.Position)

                _G.TeleportKitsune = false
            end)
        end
    end
})

_G.AutoKillPlayer = false -- Controle da Toggle

Tabs.PvpTab:AddToggle("FollowPlayerToggle", {
    Title = "Voar ate Jogador",
    Default = false,
    Callback = function(Value)
        _G.AutoKillPlayer = Value
        
        if Value then
            task.spawn(function()
                -- O loop principal SÓ PARA se a toggle for desligada
                while _G.AutoKillPlayer do
                    local targetPlayer = selectedPlayer and game.Players:FindFirstChild(selectedPlayer)
                    
                    -- VERIFICAÇÃO: Se o jogador saiu do server, desliga a toggle e para o farm
                    if not targetPlayer then
                        warn("O jogador selecionado saiu do servidor!")
                        _G.AutoKillPlayer = false
                        -- Aqui você pode atualizar a sua UI para falso se a sua biblioteca permitir (ex: FollowPlayerToggle:Set(false))
                        break
                    end
                    
                    pcall(function()
                        local targetChar = targetPlayer.Character
                        local targetHrp = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
                        local targetHumanoid = targetChar and targetChar:FindFirstChildOfClass("Humanoid")
                        
                        local myChar = game.Players.LocalPlayer.Character
                        local myHrp = myChar and myChar:FindFirstChild("HumanoidRootPart")
                        local myHumanoid = myChar and myChar:FindFirstChildOfClass("Humanoid")
                        
                        -- Verifica se ambos estão vivos e com os corpos carregados
                        if targetHrp and targetHumanoid and myHrp and myHumanoid and myHumanoid.Health > 0 then
                            
                            -- Executa a SUA função de equipar arma
                            if type(_G.ChooseWP2) == "function" then
                                _G.ChooseWP2()
                            end
                            
                            -- Calcula o trajeto com base no seu Slider de velocidade
                            local distancia = (myHrp.Position - targetHrp.Position).Magnitude
                            local velocidade = (_G.VelocidadeFarmBone and _G.VelocidadeFarmBone > 0) and _G.VelocidadeFarmBone or 350
                            local duracao = distancia / velocidade
                            
                            local tweenInfo = TweenInfo.new(duracao, Enum.EasingStyle.Linear)
                            local tween = game:GetService("TweenService"):Create(myHrp, tweenInfo, {CFrame = targetHrp.CFrame})
                            
                            tween:Play()
                            
                            -- ESPERA CHEGAR: Espera o voo terminar ou o alvo morrer antes de recalcular
                            -- Isso evita travamentos e faz o voo ser 100% fluido
                            while tween.PlaybackState == Enum.PlaybackState.Playing and _G.AutoKillPlayer and targetHumanoid.Health > 0 and targetPlayer.Parent == game.Players do
                                task.wait(0.1)
                            end
                            
                            tween:Cancel() -- Cancela o tween antigo para iniciar o próximo limpo
                        end
                    end)
                    
                    task.wait(0.2) -- Pequena pausa de segurança antes de checar o alvo novamente
                end
            end)
        end
    end
})

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local EnemiesFolder = Workspace:WaitForChild("Enemies")

-- Função padrão de voo implacável com controle de velocidade (_G.VelocidadeFarmBone)
local function voarAteSoulReaper(hrp, posicaoAlvo)
    local distancia = (hrp.Position - posicaoAlvo).Magnitude
    if distancia < 2 then return end
    
    local velocidade = (_G.VelocidadeFarmBone and _G.VelocidadeFarmBone > 0) and _G.VelocidadeFarmBone or 350
    local duracao = distancia / velocidade
    
    local tween = TweenService:Create(hrp, TweenInfo.new(duracao, Enum.EasingStyle.Linear), {CFrame = CFrame.new(posicaoAlvo)})
    tween:Play()
    
    local timeout = tick() + duracao + 0.5
    repeat
        task.wait(0.05)
    until (hrp.Position - posicaoAlvo).Magnitude < 3 or tick() > timeout or not _G.KillSoulReaper
    
    if not _G.KillSoulReaper then tween:Cancel() end
end

-- TOGGLE PARA MATAR O SOUL REAPER NA TAB STACK
_G.KillSoulReaper = false

Tabs.Stack:AddToggle("KillSoulReaperToggle", {
    Title = "Kill Soul Reaper",
    Default = false,
    Callback = function(Value)
        _G.KillSoulReaper = Value
        
        if Value then
            task.spawn(function()
                -- O loop só para se o boss morrer/sumir ou se você desligar a toggle
                while _G.KillSoulReaper do
                    pcall(function()
                        local character = LocalPlayer.Character
                        local hrp = character and character:FindFirstChild("HumanoidRootPart")
                        local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                        
                        -- Se você morrer ou resetar, espera o seu corpo spawnar de novo
                        if not hrp or not humanoid or humanoid.Health <= 0 then
                            task.wait(0.5)
                            return
                        end
                        
                        -- Verifica se o Soul Reaper está na pasta e vivo
                        local boss = EnemiesFolder:FindFirstChild("Soul Reaper")
                        if not boss then
                            warn("Soul Reaper nao encontrado ou ja foi derrotado!")
                            _G.KillSoulReaper = false -- Desliga automaticamente se ele sumir
                            return
                        end
                        
                        local bossHrp = boss:FindFirstChild("HumanoidRootPart")
                        local bossHumanoid = boss:FindFirstChildOfClass("Humanoid")
                        
                        if bossHrp and bossHumanoid and bossHumanoid.Health > 0 then
                            -- EQUIP WEAPON (Garante a arma se a mão estiver vazia)
                            if not character:FindFirstChildOfClass("Tool") and type(_G.ChooseWP2) == "function" then 
                                _G.ChooseWP2() 
                            end
                            
                            -- Voa exatamente 3 studs acima dele para descer a lenha com segurança
                            voarAteSoulReaper(hrp, bossHrp.Position + Vector3.new(0, 3, 0))
                            
                            -- Se o Bring Mob estiver ativo, ajuda a puxar se tiver mais bicho perto
                            if type(_G.BringMobFuncion) == "function" then
                                _G.BringMobFuncion(boss)
                            end
                        else
                            -- Se a vida dele zerou, o farm acabou!
                            warn("Soul Reaper foi de base! Desligando farm.")
                            _G.KillSoulReaper = false
                        end
                    end)
                    task.wait(0.1)
                end
            end)
        end
    end
})

_G.KillRipIndra = false

-- TOGGLE PARA CAÇAR O RIP INDRA TRUE FORM
Tabs.Stack:AddToggle("KillRipIndraToggle", {
    Title = "Kill Rip Indra",
    Default = false,
    Callback = function(Value)
        _G.KillRipIndra = Value

        if Value then
            task.spawn(function()
                while _G.KillRipIndra do
                    pcall(function()
                        local character = LocalPlayer.Character
                        local hrp = character and character:FindFirstChild("HumanoidRootPart")
                        local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                        
                        if not hrp or not humanoid or humanoid.Health <= 0 then 
                            task.wait(0.5)
                            return 
                        end

                        -- Escaneia a localização exata na pasta workspace.Enemies
                        local boss = workspace:FindFirstChild("Enemies") and workspace.Enemies:FindFirstChild("rip_indra True Form") or workspace:FindFirstChild("rip_indra True Form")

                        -- Se o Boss não estiver spawnado no servidor, limpa as forças e espera
                        if not boss then
                            local bv = hrp:FindFirstChild("AntiCheatFlyForce")
                            if bv then bv:Destroy() end
                            task.wait(0.5)
                            return
                        end

                        local bossHrp = boss:FindFirstChild("HumanoidRootPart")
                        local bossHumanoid = boss:FindFirstChildOfClass("Humanoid")

                        -- Se o rip_indra estiver vivo, desce o cacete nele
                        if bossHrp and bossHumanoid and bossHumanoid.Health > 0 then
                            
                            -- Executa a sua função nativa de equipar a arma para atacar
                            if type(_G.ChooseWP2) == "function" and not character:FindFirstChildOfClass("Tool") then
                                _G.ChooseWP2()
                            end

                            -- Calcula o ponto cego (3 studs acima da cabeça dele para atacar com total segurança)
                            local posicaoAlvo = bossHrp.Position + Vector3.new(0, 3, 0)
                            
                            -- Inicia o voo físico suave travando no Boss
                            voarFisicoAntiCheat(hrp, posicaoAlvo, humanoid)
                        end
                    end)
                    task.wait(0.05)
                end
                
                -- Limpeza absoluta ao desligar a Toggle manualmente para você não ficar flutuando
                pcall(function()
                    local character = LocalPlayer.Character
                    local hrp = character and character:FindFirstChild("HumanoidRootPart")
                    if hrp and hrp:FindFirstChild("AntiCheatFlyForce") then
                        hrp.AntiCheatFlyForce:Destroy()
                    end
                end)
            end)
        end
    end
})


local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer

-- VARIÁVEIS DE CONTROLE
_G.BarcoSelecionado = "Guardian" -- Valor padrão inicial
_G.AutoSpawnBoat = false

-- Conexão para manter o Noclip ativo
local NoclipConnection = nil

-- FUNÇÃO DE VOO FÍSICO ANTI-CHEAT
local function voarFisicoAntiCheat(hrp, posicaoAlvo, humanoid)
    humanoid:ChangeState(Enum.HumanoidStateType.Physics)
    
    local bv = hrp:FindFirstChild("AntiCheatFlyForce")
    if not bv then
        bv = Instance.new("BodyVelocity")
        bv.Name = "AntiCheatFlyForce"
        bv.MaxForce = Vector3.new(9e9, 9e9, 9e9)
        bv.Parent = hrp
    end
    
    while _G.AutoSpawnBoat and hrp and hrp.Parent and (hrp.Position - posicaoAlvo).Magnitude > 4 do
        local distanciaVector = (posicaoAlvo - hrp.Position)
        local direcao = distanciaVector.Unit
        
        bv.Velocity = direcao * 85 -- Velocidade segura de voo
        hrp.CFrame = CFrame.lookAt(hrp.Position, Vector3.new(posicaoAlvo.X, hrp.Position.Y, posicaoAlvo.Z))
        
        RunService.Heartbeat:Wait()
    end
    
    if bv then bv:Destroy() end
    humanoid:ChangeState(Enum.HumanoidStateType.Standing)
end

-- FUNÇÃO PARA ATIVAR/DESATIVAR NOCLIP
local function setNoclip(state)
    if state then
        if not NoclipConnection then
            NoclipConnection = RunService.Stepped:Connect(function()
                local character = LocalPlayer.Character
                if character then
                    for _, part in ipairs(character:GetDescendants()) do
                        if part:IsA("BasePart") and part.CanCollide then
                            part.CanCollide = false
                        end
                    end
                end
            end)
        end
    else
        if NoclipConnection then
            NoclipConnection:Disconnect()
            NoclipConnection = nil
        end
    end
end

-- 1. DROPDOWN DE SELEÇÃO DO BARCO
Tabs.seaevent:AddDropdown("DropdownBarcos", {
    Title = "Selecionar Barco",
    Values = {"Guardian"},
    CurrentOption = "Guardian",
    Callback = function(Value)
        _G.BarcoSelecionado = Value
    end
})

-- 2. TOGGLE PARA EXECUTAR TODO O PROCESSO
Tabs.seaevent:AddToggle("AutoSpawnBoatToggle", {
    Title = "Spawn boat",
    Default = false,
    Callback = function(Value)
        _G.AutoSpawnBoat = Value
        
        if Value then
            task.spawn(function()
                pcall(function()
                    local character = LocalPlayer.Character
                    local hrp = character and character:FindFirstChild("HumanoidRootPart")
                    local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                    
                    if not hrp or not humanoid or humanoid.Health <= 0 then return end
                    
                    -- ETAPA 1: Voar até o Luxury Boat Dealer
                    local dealerPart = Workspace:FindFirstChild("NPCs") and Workspace.NPCs:FindFirstChild("Luxury Boat Dealer") and Workspace.NPCs["Luxury Boat Dealer"]:FindFirstChild("UpperTorso")
                    
                    if dealerPart then
                        voarFisicoAntiCheat(hrp, dealerPart.Position, humanoid)
                        if not _G.AutoSpawnBoat then return end
                        task.wait(0.3)
                        
                        -- ETAPA 2: Executa os args e o remote EXATAMENTE juntos (uma única vez)
                        local args = {
                            "BuyBoat",
                            _G.BarcoSelecionado
                        }
                        game:GetService("ReplicatedStorage"):WaitForChild("Remotes"):WaitForChild("CommF_"):InvokeServer(unpack(args))
                        
                        -- Espera rápida para o jogo carregar e criar o barco na pasta
                        task.wait(1.5) 
                    else
                        warn("UpperTorso do Luxury Boat Dealer nao encontrado!")
                    end
                    
                    -- ETAPA 3: Voar até o VehicleSeat do barco selecionado que foi criado
                    if _G.AutoSpawnBoat then
                        local pastaBoats = Workspace:FindFirstChild("Boats")
                        local meuBarco = pastaBoats and pastaBoats:FindFirstChild(_G.BarcoSelecionado)
                        local assento = meuBarco and meuBarco:FindFirstChild("VehicleSeat")
                        
                        if assento then
                            -- 🔥 ATIVA O NOCLIP AQUI ANTES DE IR PRO ASSEENTO DO BARCO
                            setNoclip(true)
                            
                            voarFisicoAntiCheat(hrp, assento.Position, humanoid)
                            
                            -- Senta no banco quando chega perto
                            if _G.AutoSpawnBoat and (hrp.Position - assento.Position).Magnitude <= 5 then
                                if humanoid.SeatPart ~= assento then
                                    assento:Sit(humanoid)
                                end
                            end
                        else
                            warn("VehicleSeat do barco " .. tostring(_G.BarcoSelecionado) .. " nao encontrado!")
                        end
                    end
                    
                    -- Desliga o noclip após sentar com sucesso ou encerrar a sequência
                    setNoclip(false)
                end)
            end)
        else
            -- Limpeza de forças e desativar noclip caso desligue a toggle no meio do caminho
            setNoclip(false)
            pcall(function()
                local character = LocalPlayer.Character
                local hrp = character and character:FindFirstChild("HumanoidRootPart")
                if hrp and hrp:FindFirstChild("AntiCheatFlyForce") then
                    hrp.AntiCheatFlyForce:Destroy()
                end
            end)
        end
    end
})

            -- Limpeza de forças caso desligue a toggle no meio do caminho
            pcall(function()
                local character = LocalPlayer.Character
                local hrp = character and character:FindFirstChild("HumanoidRootPart")
                if hrp and hrp:FindFirstChild("AntiCheatFlyForce") then
                    hrp.AntiCheatFlyForce:Destroy()
                end
            end)
        end
    end
})
