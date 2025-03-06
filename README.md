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

    if isSpeedActive then
        task.wait(1) -- Pequeno delay para garantir que o jogo não sobrescreva
        applySpeed(humanoid)
    end

    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.V then
            isSpeedActive = not isSpeedActive
            if isSpeedActive then
                applySpeed(humanoid)
            else
                humanoid.WalkSpeed = normalSpeed
            end
        end
    end)
end

player.CharacterAdded:Connect(onCharacterAdded)
if player.Character then
    onCharacterAdded(player.Character)
end
