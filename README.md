local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

local speed = 50
local normalSpeed = 16
local isSpeedActive = false
local button

local function applySpeed(humanoid)
    while true do
        if isSpeedActive then
            humanoid.WalkSpeed = speed
        else
            humanoid.WalkSpeed = normalSpeed
        end
        task.wait(0.1) -- Loop contínuo para evitar stun
    end
end

local function toggleSpeed()
    isSpeedActive = not isSpeedActive
    if button then
        button.Text = isSpeedActive and "Speed ON" or "Speed OFF"
        button.BackgroundColor3 = isSpeedActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(255, 0, 0)
    end
end

local function createButton()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = game:GetService("CoreGui")

    button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 150, 0, 50)
    button.Position = UDim2.new(0.05, 0, 0.85, 0)
    button.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    button.TextScaled = true
    button.Text = "Speed OFF"
    button.Parent = screenGui

    button.MouseButton1Click:Connect(toggleSpeed)
end

local function onCharacterAdded(character)
    local humanoid = character:WaitForChild("Humanoid")

    task.spawn(function()
        applySpeed(humanoid) -- Loop infinito para manter a velocidade
    end)
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.V then
        toggleSpeed()
    end
end)

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end

createButton()
