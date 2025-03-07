local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local StarterGui = game:GetService("StarterGui")

local speed = 50
local normalSpeed = 16
local isSpeedActive = false
local button

local function updateSpeed()
    while true do
        local character = player.Character
        if character then
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.WalkSpeed = isSpeedActive and speed or normalSpeed
            end
        end
        task.wait(0.1)
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

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.V then
        toggleSpeed()
    end
end)

task.spawn(updateSpeed) -- Mantém o speed ativo o tempo todo
createButton()
