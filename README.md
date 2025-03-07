local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

local speed = 50
local normalSpeed = 16
local isSpeedActive = false

local function createButton()
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = game:GetService("CoreGui")

    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 150, 0, 50)
    button.Position = UDim2.new(0.05, 0, 0.85, 0) -- Ajuste a posição do botão
    button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    button.TextScaled = true
    button.Text = "Speed OFF"
    button.Parent = screenGui

    button.MouseButton1Click:Connect(function()
        isSpeedActive = not isSpeedActive
        button.Text = isSpeedActive and "Speed ON" or "Speed OFF"
    end)
end

local function onCharacterAdded(character)
    local humanoid = character:WaitForChild("Humanoid")

    task.spawn(function()
        while true do
            humanoid.WalkSpeed = isSpeedActive and speed or normalSpeed
            task.wait(0.1)
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end

createButton() -- Cria o botão na tela
