local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local localPlayer = Players.LocalPlayer
local isActive = true  -- Variável para ativar/desativar o script

-- Função para criar uma marca de FOV
local function createFOVMarker(player, color)
    local marker = Instance.new("BillboardGui")
    marker.Size = UDim2.new(0, 200, 0, 50)
    marker.AlwaysOnTop = true

    local frame = Instance.new("Frame", marker)
    frame.BackgroundColor3 = color
    frame.Size = UDim2.new(1, 0, 1, 0)

    marker.Adornee = player.Character.Head
    marker.Parent = player.Character.Head
end

-- Função para adicionar setas na direção dos inimigos
local function createArrowMarker(player, color)
    local arrow = Instance.new("Part")
    arrow.Size = Vector3.new(1, 1, 1)
    arrow.Color = color
    arrow.Anchored = true
    arrow.CFrame = CFrame.new(player.Character.Head.Position + Vector3.new(0, 5, 0))
    arrow.Parent = player.Character
end

-- Função para atualizar a identificação das equipes
local function updatePlayerMarkers()
    if not isActive then return end
    
    for _, player in pairs(Players:GetPlayers()) do
        if player.Team == localPlayer.Team then
            createFOVMarker(player, Color3.fromRGB(0, 0, 255))  -- Cor azul para equipe amiga
        else
            createFOVMarker(player, Color3.fromRGB(255, 0, 0))  -- Cor vermelha para equipe inimiga
            createArrowMarker(player, Color3.fromRGB(255, 0, 0)) -- Setas vermelhas para inimigos
        end
    end
end

-- Função para alternar a ativação do script
local function toggleScript()
    isActive = not isActive
    if isActive then
        print("Script ativado")
        updatePlayerMarkers()
    else
        print("Script desativado")
    end
end

-- Atualizar os marcadores quando os jogadores entram no jogo
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        updatePlayerMarkers()
    end)
end)

-- Atualizar os marcadores continuamente
RunService.RenderStepped:Connect(updatePlayerMarkers)

-- Detectar toque no botão para ativar/desativar o script
local button = script.Parent:WaitForChild("ScreenGui"):WaitForChild("TextButton")
button.MouseButton1Click:Connect(toggleScript)
