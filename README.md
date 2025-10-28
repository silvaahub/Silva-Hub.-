local redzlib = loadstring(game:HttpGet("https://raw.githubusercontent.com/tbao143/Library-ui/refs/heads/main/Redzhubui"))()

local Window = redzlib:MakeWindow({
    Title = "forsaken Hub v1.0.1",
    SubTitle = "by silva4M",
    SaveFolder = "teste"
})

Window:AddMinimizeButton({
    Button = { Image = "rbxassetid://0", BackgroundTransparency = 0 },
    Corner = { CornerRadius = UDim.new(35, 1) },
})

-- ================= Tabs =================
local Tab1 = Window:MakeTab({"Credits", "info"})
local Tab2 = Window:MakeTab({"Fun", "fun"})
local Tab3 = Window:MakeTab({"Avatar", "shirt"})
local Tab4 = Window:MakeTab({"House", "Home"})
local Tab5 = Window:MakeTab({"Car", "Car"})
local Tab6 = Window:MakeTab({"RGB", "brush"})
local Tab7 = Window:MakeTab({"Music All", "radio"})    
local Tab8 = Window:MakeTab({"Music", "music"}) 
local Tab9 = Window:MakeTab({"Troll", "skull"}) 
local Tab10 = Window:MakeTab({"Coming soon", "bomb"})
local Tab11 = Window:MakeTab({"Coming soon", "scroll"})
local Tab12 = Window:MakeTab({"Coming soon", "map-pin"})

-- ================= Tab 1: Credits =================
local function detectExecutor()
    if identifyexecutor then
        return identifyexecutor()
    elseif syn then
        return "Synapse X"
    elseif KRNL_LOADED then
        return "KRNL"
    elseif is_sirhurt_closure then
        return "SirHurt"
    elseif pebc_execute then
        return "ProtoSmasher"
    elseif getexecutorname then
        return getexecutorname()
    else
        return "Executor Desconhecido"
    end
end

local executorName = detectExecutor()
Tab1:AddParagraph({"Executor", executorName})
Tab1:AddSection({"[ 🌟 ] forsaken Hub : Script sendo executado."})
Tab1:AddParagraph({"Criadores", "silva4M\\by silva"})

-- ================= Tab 2: Fun =================
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local localPlayer = Players.LocalPlayer

-- ==== Headsit ====
local selectedPlayerName = nil
local headsitActive = false

local function headsitOnPlayer(targetPlayer)
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then return false end
    local targetHead = targetPlayer.Character.Head
    local localRoot = character:FindFirstChild("HumanoidRootPart")
    if not localRoot then return false end
    localRoot.CFrame = targetHead.CFrame * CFrame.new(0, 2.2, 0)
    for _, v in pairs(localRoot:GetChildren()) do if v:IsA("WeldConstraint") then v:Destroy() end end
    local weld = Instance.new("WeldConstraint")
    weld.Part0 = localRoot
    weld.Part1 = targetHead
    weld.Parent = localRoot
    if humanoid then humanoid.Sit = true end
    return true
end

local function removeHeadsit()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    local localRoot = character:FindFirstChild("HumanoidRootPart")
    if localRoot then
        for _, v in pairs(localRoot:GetChildren()) do if v:IsA("WeldConstraint") then v:Destroy() end end
    end
    if humanoid then humanoid.Sit = false end
end

local function findPlayerByPartialName(partial)
    partial = partial:lower()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Name:lower():sub(1, #partial) == partial then
            return player
        end
    end
    return nil
end

local function notifyPlayerSelected(player)
    local StarterGui = game:GetService("StarterGui")
    local content, _ = Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size100x100)
    StarterGui:SetCore("SendNotification", {
        Title = "Player Selecionado",
        Text = player.Name .. " foi selecionado!",
        Icon = content,
        Duration = 5
    })
end

Tab2:AddTextBox({
    Name = "Nome do Jogador",
    Description = "Digite parte do nome",
    PlaceholderText = "ex: lo → Lolyta",
    Callback = function(Value)
        local foundPlayer = findPlayerByPartialName(Value)
        if foundPlayer then
            selectedPlayerName = foundPlayer.Name
            notifyPlayerSelected(foundPlayer)
        else
            warn("Nenhum jogador encontrado com esse nome.")
        end
    end
})

Tab2:AddButton({
    Name = "Headsit Toggle",
    Callback = function()
        if not selectedPlayerName then return end
        if not headsitActive then
            local target = Players:FindFirstChild(selectedPlayerName)
            if target and headsitOnPlayer(target) then headsitActive = true end
        else
            removeHeadsit()
            headsitActive = false
        end
    end
})

-- ==== Speed / Jump / Gravity / Infinite Jump ====
Tab2:AddSlider({
    Name = "Speed Player",
    Increase = 1,
    MinValue = 16,
    MaxValue = 888,
    Default = 16,
    Callback = function(Value)
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then humanoid.WalkSpeed = Value end
    end
})

Tab2:AddSlider({
    Name = "Jumppower",
    Increase = 1,
    MinValue = 50,
    MaxValue = 500,
    Default = 50,
    Callback = function(Value)
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then humanoid.JumpPower = Value end
    end
})

Tab2:AddSlider({
    Name = "Gravity",
    Increase = 1,
    MinValue = 0,
    MaxValue = 10000,
    Default = 196.2,
    Callback = function(Value)
        game.Workspace.Gravity = Value
    end
})

local InfiniteJumpEnabled = false
game:GetService("UserInputService").JumpRequest:Connect(function()
    if InfiniteJumpEnabled then
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then humanoid:ChangeState(Enum.HumanoidStateType.Jumping) end
    end
end)

Tab2:AddToggle({
    Name = "Infinite Jump",
    Default = false,
    Callback = function(Value)
        InfiniteJumpEnabled = Value
    end
})

Tab2:AddButton({
    Name = "Reset Speed/Gravity/Jumppower.✅",
    Callback = function()
        local humanoid = localPlayer.Character and localPlayer.Character:FindFirstChildOfClass("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = 16
            humanoid.JumpPower = 50
        end
        game.Workspace.Gravity = 196.2
        InfiniteJumpEnabled = false
    end
})

-- ==== Ultimate Noclip ====
local UltimateNoclip = { Enabled = false, Connections = {}, SoccerBalls = {} }
local function managePlayerCollisions(character)
    if not character then return end
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not UltimateNoclip.Enabled
            part.Anchored = false
        end
    end
end

local function voidProtection(rootPart)
    if rootPart.Position.Y < -500 then
        rootPart.CFrame = CFrame.new(0, 100, 0)
    end
end

local function manageSoccerBalls()
    local soccerFolder = Workspace:FindFirstChild("Com", true) and Workspace.Com:FindFirstChild("001_SoccerBalls")
    if soccerFolder then
        for _, ball in ipairs(soccerFolder:GetChildren()) do
            if ball.Name:match("^Soccer") then
                pcall(function()
                    ball.CanCollide = not UltimateNoclip.Enabled
                    ball.Anchored = UltimateNoclip.Enabled
                end)
                UltimateNoclip.SoccerBalls[ball] = true
            end
        end
    end
end

local function mainLoop()
    UltimateNoclip.Connections.Heartbeat = RunService.Heartbeat:Connect(function()
        local character = localPlayer.Character
        if character then
            managePlayerCollisions(character)
            local rootPart = character:FindFirstChild("HumanoidRootPart")
            if rootPart then voidProtection(rootPart) end
        end
    end)
end

Tab2:AddToggle({
    Name = "Ultimate Noclip",
    Description = "Noclip + Controle de bolas integrado",
    Default = false,
    Callback = function(state)
        UltimateNoclip.Enabled = state
        if state then
            mainLoop()
            manageSoccerBalls()
        else
            for _, conn in pairs(UltimateNoclip.Connections) do if conn.Connected then conn:Disconnect() end end
            if localPlayer.Character then managePlayerCollisions(localPlayer.Character) end
        end
    end
})

-- ==== Anti-Sit ====
local antiSitConnection = nil
local antiSitEnabled = false
Tab2:AddToggle({
    Name = "Anti-Sit",
    Default = false,
    Callback = function(state)
        antiSitEnabled = state
        local function applyAntiSit(character)
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid.Sit = false
                humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, not state)
                if antiSitConnection then antiSitConnection:Disconnect() end
                antiSitConnection = humanoid.Seated:Connect(function(isSeated)
                    if isSeated then
                        humanoid.Sit = false
                        humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
                    end
                end)
            end
        end
        if localPlayer.Character then applyAntiSit(localPlayer.Character) end
        localPlayer.CharacterAdded:Connect(function(character) if antiSitEnabled then applyAntiSit(character) end end)
    end
})

-- ==== Fly GUI ====
Tab2:AddButton({
    Name = "Ativar Fly GUI",
    Callback = function()
        local success, _ = pcall(function()
            loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Fly-gui-v3-30439"))()
        end)
        game.StarterGui:SetCore("SendNotification", {
            Title = success and "Sucesso" or "Erro",
            Text = success and "Fly GUI carregado!" or "Falha ao carregar Fly GUI.",
            Duration = 5
        })
    end
})

-- ==== ESP ====
local espEnabled = false
local billboardGuis = {}
local connections = {}
local selectedColor = "RGB"

Tab2:AddDropdown({
    Name = "Cor do ESP",
    Default = "RGB",
    Options = { "RGB", "Branco", "Preto", "Vermelho", "Verde", "Azul", "Amarelo", "Rosa", "Roxo" },
    Callback = function(value) selectedColor = value end
})

local function getESPColor()
    if selectedColor == "RGB" then return Color3.fromHSV((tick() % 5)/5,1,1) end
    local colors = { Preto=Color3.new(0,0,0), Branco=Color3.new(1,1,1), Vermelho=Color3.fromRGB(255,0,0),
        Verde=Color3.fromRGB(0,255,0), Azul=Color3.fromRGB(0,170,255), Amarelo=Color3.fromRGB(255,255,0),
        Rosa=Color3.fromRGB(255,105,180), Roxo=Color3.fromRGB(128,0,128) }
    return colors[selectedColor] or Color3.new(1,1,1)
end

local function updateESP(player)
    if player == localPlayer or not espEnabled then return end
    local character = player.Character
    if character then
        local head = character:FindFirstChild("Head")
        if head then
            if billboardGuis[player] then billboardGuis[player]:Destroy() end
            local billboard = Instance.new("BillboardGui")
            billboard.Name = "ESP_Billboard"
            billboard.Parent = head
            billboard.Adornee = head
            billboard.Size = UDim2.new(0,200,0,50)
            billboard.StudsOffset = Vector3.new(0,3,0)
            billboard.AlwaysOnTop = true
            local textLabel = Instance.new("TextLabel")
            textLabel.Parent = billboard
            textLabel.Size = UDim2.new(1,0,1,0)
            textLabel.BackgroundTransparency = 1
            textLabel.TextStrokeTransparency = 0.5
            textLabel.Font = Enum.Font.SourceSansBold
            textLabel.TextSize = 14
            textLabel.Text = player.Name.." | "..player.AccountAge.." dias"
            textLabel.TextColor3 = getESPColor()
            billboardGuis[player] = billboard
        end
    end
end

local function removeESP(player)
    if billboardGuis[player] then
        billboardGuis[player]:Destroy()
        billboardGuis[player] = nil
    end
end

local function disconnectESPConnections()
    for _, conn in pairs(connections) do if conn.Connected then conn:Disconnect() end end
    connections = {}
    for player, gui in pairs(billboardGuis) do removeESP(player) end
end

local Toggle1 = Tab2:AddToggle({
    Name = "ESP Ativado",
    Default = false,
    Callback = function(value)
        espEnabled = value
        if espEnabled then
            for _, player in pairs(Players:GetPlayers()) do updateESP(player) end
            local updateConnection = RunService.Heartbeat:Connect(function()
                for _, player in pairs(Players:GetPlayers()) do updateESP(player) end
                if selectedColor == "RGB" then
                    for _, player in pairs(Players:GetPlayers()) do
                        local gui = billboardGuis[player]
                        if gui and gui:FindFirstChild("TextLabel") then gui.TextLabel.TextColor3 = getESPColor() end
                    end
                end
            end)
            table.insert(connections, updateConnection)
            table.insert(connections, Players.PlayerAdded:Connect(function(player)
                updateESP(player)
                table.insert(connections, player.CharacterAdded:Connect(function() updateESP(player) end))
            end))
            table.insert(connections, Players.PlayerRemoving:Connect(removeESP))
        else
            disconnectESPConnections()
        end
    end
})
