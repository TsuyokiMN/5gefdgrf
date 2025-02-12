-- GOLDEN HUB - SCRIPT COMPLETO
-- Script otimizado para desempenho, funções de ESP, Anti AFK, Horário e Região do servidor, entre outras.

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = game.Players.LocalPlayer
local Camera = game:GetService("Workspace").CurrentCamera

-- Configurações do servidor e exibição de informações
local serverInfo = {
    showTime = true,  -- Mostrar horário do servidor
    showRegion = true, -- Mostrar região do servidor
}

-- Função para exibir horário do servidor
function displayServerTime()
    local timeLabel = Instance.new("TextLabel")
    timeLabel.Size = UDim2.new(0, 200, 0, 50)
    timeLabel.Position = UDim2.new(0, 10, 0, 10)
    timeLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    timeLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    timeLabel.TextSize = 18
    timeLabel.Text = "Server Time: " .. os.date("%X", tick())
    timeLabel.Parent = LocalPlayer.PlayerGui.ScreenGui

    while serverInfo.showTime do
        timeLabel.Text = "Server Time: " .. os.date("%X", tick())
        wait(1)
    end
end

-- Função para exibir a região do servidor
function displayServerRegion()
    local regionLabel = Instance.new("TextLabel")
    regionLabel.Size = UDim2.new(0, 200, 0, 50)
    regionLabel.Position = UDim2.new(0, 10, 0, 70)
    regionLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    regionLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    regionLabel.TextSize = 18
    regionLabel.Text = "Server Region: " .. game:GetService("NetworkClient").Region
    regionLabel.Parent = LocalPlayer.PlayerGui.ScreenGui
end

-- Função para criar o ESP de jogadores
function createESP(player)
    local char = player.Character
    if not char or not char:FindFirstChild("Head") then return end

    local espPart = Instance.new("Part")
    espPart.Anchored = true
    espPart.CanCollide = false
    espPart.Size = Vector3.new(4, 6, 4)
    espPart.Transparency = 0.5
    espPart.Color = Color3.fromRGB(255, 0, 0)
    espPart.Parent = char

    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = char:FindFirstChild("Head")
    billboard.Size = UDim2.new(0, 100, 0, 100)
    billboard.AlwaysOnTop = true
    billboard.Parent = char:FindFirstChild("Head")

    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)  -- Cor da borda
    frame.BackgroundTransparency = 0.5
    frame.Parent = billboard
end

-- Anti AFK (Prevenir desconexão)
function antiAFK()
    local mouse = LocalPlayer:GetMouse()
    local lastMove = tick()

    game:GetService("RunService").Heartbeat:Connect(function()
        if tick() - lastMove > 30 then
            -- Simula atividade para evitar o AFK
            mouse.Move:Fire()
            lastMove = tick()
        end
    end)
end

-- Função para otimizar o desempenho (Anti Lag Super)
local function optimizePerformance()
    -- Desativa partículas
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Trail") then
            v:Destroy()
        end
    end

    -- Configuração de sombra e gráficos
    Lighting.ShadowQuality = Enum.ShadowQuality.NoShadows
    game:GetService("GraphicsQuality").QualityLevel = Enum.GraphicsQuality.Level1

    -- Reduz a qualidade dos sons
    for _, sound in pairs(Workspace:GetDescendants()) do
        if sound:IsA("Sound") then
            sound:Stop()
        end
    end

    -- Otimização de física
    Players.PlayerAdded:Connect(function(player)
        player.CharacterAdded:Connect(function(character)
            for _, part in pairs(character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                    part.Massless = true
                end
            end
        end)
    end)

    -- Otimização de rede e instâncias
    Players.SetNetworkOwnershipAuto(true)
end

-- Função principal para executar todas as otimizações e funcionalidades
function main()
    -- Ativa funcionalidades de otimização
    optimizePerformance()

    -- Exibe informações do servidor
    if serverInfo.showTime then
        displayServerTime()
    end
    if serverInfo.showRegion then
        displayServerRegion()
    end

    -- Configura o Anti AFK
    antiAFK()

    -- Cria ESP para jogadores
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character then
            createESP(player)
        end

        player.CharacterAdded:Connect(function()
            createESP(player)
        end)
    end
end

-- Inicia o script
main()

-- Exibição do link do Discord
print("Junte-se ao meu servidor no Discord: https://discord.gg/XvdyTu9GZS")

