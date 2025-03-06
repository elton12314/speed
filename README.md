local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

local speed = 50 -- Velocidade alterada
local normalSpeed = 16
local isSpeedActive = false

local function applySpeed(humanoid)
    while isSpeedActive do
        humanoid.WalkSpeed = speed
        task.wait(0.1)
    end
end

local function onCharacterAdded(character)
    local humanoid = character:WaitForChild("Humanoid")

    task.spawn(function()
        while true do
            if isSpeedActive then
                humanoid.WalkSpeed = speed
            else
                humanoid.WalkSpeed = normalSpeed
            end
            task.wait(0.1)
        end
    end)

    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.V then
            isSpeedActive = not isSpeedActive
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end
