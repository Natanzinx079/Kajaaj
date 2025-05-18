-- NATAN DEAD REAILS 1.0 | Script Avançado para Dead Rails
-- Desenvolvido para Natan com hub único, funções úteis e teleportes atualizados

-- Biblioteca de UI (Orion)
local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))()
local Window = OrionLib:MakeWindow({
    Name = "NATAN DEAD REAILS 1.0",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "NatanDeadRails",
    IntroText = "NATAN DEAD REAILS 1.0",
    IntroIcon = ""
})

-- Variáveis principais
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

-- Função de teleporte
function teleportTo(pos)
    if character and character:FindFirstChild("HumanoidRootPart") then
        character:MoveTo(pos)
    end
end

-- Criação das abas
local tpTab = Window:MakeTab({
    Name = "Teleporte",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

local farmTab = Window:MakeTab({
    Name = "Auto Farm / Noclip",
    Icon = "rbxassetid://6035047409",
    PremiumOnly = false
})

-- TELEPORTES PRINCIPAIS
tpTab:AddButton({ Name = "Banco do Trem", Callback = function() teleportTo(Vector3.new(10, 5, -80)) end })
tpTab:AddButton({ Name = "Torre Final", Callback = function() teleportTo(Vector3.new(105, 110, 340)) end })
tpTab:AddButton({ Name = "Base Principal", Callback = function() teleportTo(Vector3.new(-200, 30, 50)) end })

-- TELEPORTES EXTRAS
tpTab:AddButton({ Name = "Castelo", Callback = function() teleportTo(Vector3.new(-500, 40, 250)) end })
tpTab:AddButton({ Name = "Tesla Lab", Callback = function() teleportTo(Vector3.new(340, 25, -120)) end })
tpTab:AddButton({ Name = "Estação Inicial", Callback = function() teleportTo(Vector3.new(75, 15, 10)) end })
tpTab:AddButton({ Name = "Área do Zumbi Gigante", Callback = function() teleportTo(Vector3.new(910, 28, 120)) end })
tpTab:AddButton({ Name = "Fortaleza de Ferro", Callback = function() teleportTo(Vector3.new(-820, 70, -280)) end })

-- AUTO FARM
_G.AutoFarm = false
farmTab:AddToggle({
    Name = "Ativar Auto Farm",
    Default = false,
    Callback = function(value)
        _G.AutoFarm = value
        while _G.AutoFarm do
            for _, item in pairs(workspace:GetDescendants()) do
                if item:IsA("TouchTransmitter") and item.Parent then
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 0)
                    firetouchinterest(character.HumanoidRootPart, item.Parent, 1)
                end
            end
            task.wait(1)
        end
    end
})

-- NOCLIP
farmTab:AddToggle({
    Name = "Ativar Noclip",
    Default = false,
    Callback = function(state)
        getgenv().noclip = state
        game:GetService("RunService").Stepped:Connect(function()
            if getgenv().noclip and character then
                for _, p in pairs(character:GetDescendants()) do
                    if p:IsA("BasePart") then p.CanCollide = false end
                end
            end
        end)
    end
})

-- Inicializa o menu
OrionLib:Init()
