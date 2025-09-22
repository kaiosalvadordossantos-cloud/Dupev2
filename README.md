local ADMIN_USERIDS = {
    [7255074977] = true,
}

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local SpawnItem = ReplicatedStorage:WaitForChild("SpawnItem")

SpawnItem.OnServerEvent:Connect(function(player, itemName, tipo)
    if not ADMIN_USERIDS[player.UserId] then
        warn("[ANTICHEAT] Tentativa de SpawnItem por "..player.Name)
        player:Kick("Tentativa de trapaça detectada.")
        return
    end

    local modelo = ReplicatedStorage:FindFirstChild(itemName)
    if not modelo then return end

    if tipo == "Arma" then
        modelo:Clone().Parent = player.Backpack
    elseif tipo == "Carro" then
        local novoCarro = modelo:Clone()
        novoCarro.Parent = workspace
        if player.Character and player.Character.PrimaryPart then
            novoCarro:SetPrimaryPartCFrame(player.Character.PrimaryPart.CFrame * CFrame.new(5,0,0))
        end
    end
end)
