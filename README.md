-- Função para desenhar as linhas
local function drawLine(startPos, endPos, color)
    local line = Instance.new("Part")
    line.Anchored = true
    line.CanCollide = false
    line.Size = Vector3.new(0.1, 0.1, (startPos - endPos).Magnitude)
    line.Color = color
    line.Position = (startPos + endPos) / 2
    line.CFrame = CFrame.new(startPos, endPos) -- Alinha o eixo da linha
    line.Parent = workspace

    -- Destrói a linha após 1 segundo
    game.Debris:AddItem(line, 1)
end

-- Função principal para conectar o jogador a outros jogadores próximos
local function connectToPlayers()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local hrp = character:WaitForChild("HumanoidRootPart")

    -- Loop para verificar os outros jogadores no jogo
    while true do
        wait(0.1) -- Atualiza a cada 0.1 segundos

        for _, otherPlayer in ipairs(game.Players:GetPlayers()) do
            if otherPlayer ~= player and otherPlayer.Character then
                local otherHrp = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
                if otherHrp then
                    local distance = (hrp.Position - otherHrp.Position).Magnitude
                    -- Desenha a linha se a distância entre os jogadores for menor que 50 studs
                    if distance < 50 then
                        drawLine(hrp.Position, otherHrp.Position, Color3.fromRGB(255, 255, 255)) -- Cor branca
                    end
                end
            end
        end
    end
end

-- Chama a função de conexão ao iniciar
connectToPlayers()
