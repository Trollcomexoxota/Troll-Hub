if game.PlaceId == 76598287484083 then
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
    local Window = OrionLib:MakeWindow({
        Name = "Troll Hub - Auto Tower",
        HidePremium = false,
        IntroEnabled = false,
        IntroText = "Troll Hub",
        SaveConfig = true,
        ConfigFolder = "TrollHub"
    })

    local Players = game:GetService("Players")
    local LocalPlayer = Players.LocalPlayer
    local TweenService = game:GetService("TweenService")
    local UserInputService = game:GetService("UserInputService")

    -- CONFIG
    local towerEntrance = Vector3.new(-2038.8095703125, 57.406160076660156, -14844.740234375)
    local teleportSpeed = 1
    local waitInTower = 60 * 10 -- 10 minutos
    local savedPosition = nil
    local autoTowerActive = false

    -- Funções
    local function teleportTo(position)
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            local tween = TweenService:Create(char.HumanoidRootPart, TweenInfo.new(teleportSpeed), {CFrame = CFrame.new(position)})
            tween:Play()
            tween.Completed:Wait()
        end
    end

    local function savePosition()
        local char = LocalPlayer.Character
        if char and char:FindFirstChild("HumanoidRootPart") then
            savedPosition = char.HumanoidRootPart.Position
            OrionLib:MakeNotification({Name = "Troll Hub", Content = "Posição salva com sucesso!", Time = 3})
        end
    end

    local function removeSavedPosition()
        savedPosition = nil
        OrionLib:MakeNotification({Name = "Troll Hub", Content = "Posição salva removida!", Time = 3})
    end

    local function returnToSavedPosition()
        if savedPosition then
            teleportTo(savedPosition + Vector3.new(0, 5, 0))
            OrionLib:MakeNotification({Name = "Troll Hub", Content = "Voltando para a posição salva...", Time = 3})
        else
            OrionLib:MakeNotification({Name = "Troll Hub", Content = "Nenhuma posição salva.", Time = 3})
        end
    end

    local function waitUntilTowerOpens()
        while autoTowerActive do
            local currentTime = os.date("*t")
            local minute = currentTime.min
            local second = currentTime.sec

            if (minute == 0 or minute == 30) and second < 5 then
                break
            end
            wait(1)
        end
    end

    local function autoTowerLoop()
        while autoTowerActive do
            waitUntilTowerOpens()
            if not autoTowerActive then break end
            OrionLib:MakeNotification({Name = "Troll Hub", Content = "Entrando na torre!", Time = 3})
            teleportTo(towerEntrance + Vector3.new(0, 5, 0))
            wait(waitInTower)
            returnToSavedPosition()
            wait(10)
        end
    end

    -- GUI
    local tab = Window:MakeTab({Name = "Tower", Icon = "rbxassetid://4483345998", PremiumOnly = false})

    tab:AddToggle({
        Name = "Auto Tower",
        Default = false,
        Callback = function(state)
            autoTowerActive = state
            if autoTowerActive then
                OrionLib:MakeNotification({Name = "Troll Hub", Content = "Auto Tower Ativado", Time = 3})
                coroutine.wrap(autoTowerLoop)()
            else
                OrionLib:MakeNotification({Name = "Troll Hub", Content = "Auto Tower Desativado", Time = 3})
            end
        end
    })

    tab:AddButton({
        Name = "Salvar posição atual",
        Callback = function()
            savePosition()
        end
    })

    tab:AddButton({
        Name = "Remover posição salva",
        Callback = function()
            removeSavedPosition()
        end
    })

    -- Tecla "P" como atalho extra para salvar posição
    UserInputService.InputBegan:Connect(function(input, gameProcessed)
        if gameProcessed then return end
        if input.KeyCode == Enum.KeyCode.P then
            savePosition()
        end
    end)

end
