local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

-- ==========================================================
--                   CONFIGURAÇÃO DA JANELA
-- ==========================================================
local Window = Fluent:CreateWindow({
    Title = "Coelho Hub",
    SubTitle = "by Coelh0",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

-- ==========================================================
--                         ABAS (TABS)
-- ==========================================================
local Tabs = {
    Main = Window:AddTab({ Title = "Principal", Icon = "home" }),
    Settings = Window:AddTab({ Title = "Configurações", Icon = "settings" })
}

-- Começa direto na aba principal
Window:SelectTab(Tabs.Main)

-- ==========================================================
--                   GERENCIADORES DE CONFIGS
-- ==========================================================
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({})

InterfaceManager:SetFolder("CoelhoHub_Configs")
SaveManager:SetFolder("CoelhoHub_Configs/main")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)

SaveManager:LoadAutoloadConfig()
