local UserInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local speed = 50 
local normalSpeed = humanoid.WalkSpeed
local isSpeedActive = false

local function applySpeed()
    while isSpeedActive do
        humanoid.WalkSpeed = speed
        task.wait(0.1) -- Mantém a velocidade sem sobrecarregar
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.V then -- Tecla para ativar/desativar
        isSpeedActive = not isSpeedActive
        if isSpeedActive then
            applySpeed()
        else
            humanoid.WalkSpeed = normalSpeed
        end
    end
end)
