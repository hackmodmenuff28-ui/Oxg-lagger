-- ============================================================
-- RED HUB | AUTO GRAB SYSTEM
-- DISCORD: discord.gg/CXvmJBUJn
-- ============================================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Stats = game:GetService("Stats")
local lp = Players.LocalPlayer

local STEAL_RADIUS = 60
local STEAL_DURATION = 1.4
local isStealing = false
local StealData = {}

-- Alterado o nome do ScreenGui para Red Hub
local Red_Hub = Instance.new("ScreenGui")
Red_Hub.Name = "Red Hub"
Red_Hub.ResetOnSpawn = false
Red_Hub.IgnoreGuiInset = true
Red_Hub.Parent = lp:WaitForChild("PlayerGui")

local Frame = Instance.new("Frame")
Frame.Name = "Frame"
Frame.Position = UDim2.new(0.5, -130, 0, 35)
Frame.Size = UDim2.new(0, 260, 0, 90)
Frame.BackgroundTransparency = 1
Frame.Parent = Red_Hub

-- Top Bar (Ajustado para cores do Red Hub)
local Frame2 = Instance.new("Frame")
Frame2.Size = UDim2.new(1, 0, 0, 30)
Frame2.BackgroundColor3 = Color3.fromRGB(180, 0, 0) -- Cor alterada para Vermelho
Frame2.BackgroundTransparency = 0.85
Frame2.BorderSizePixel = 0
Frame2.Parent = Frame

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 6)
UICorner.Parent = Frame2

local UIStroke = Instance.new("UIStroke")
UIStroke.Color = Color3.fromRGB(255, 0, 0) -- Borda alterada para Vermelho Vivo
UIStroke.Thickness = 1.5
UIStroke.Parent = Frame2

local infoLabel = Instance.new("TextLabel")
infoLabel.Size = UDim2.new(1, 0, 1, 0)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = "Red Hub | Auto Grab  |  Ping: 0ms  |  FPS: 60" -- Nome atualizado
infoLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
infoLabel.TextSize = 14
infoLabel.Font = Enum.Font.GothamBold
infoLabel.TextXAlignment = Enum.TextXAlignment.Center
infoLabel.Parent = Frame2

-- Progress Bar
local Frame3 = Instance.new("Frame")
Frame3.Position = UDim2.new(0, 0, 0, 38)
Frame3.Size = UDim2.new(1, 0, 0, 32)
Frame3.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Frame3.BackgroundTransparency = 0.25
Frame3.Parent = Frame

local UICorner2 = Instance.new("UICorner")
UICorner2.CornerRadius = UDim.new(0, 12)
UICorner2.Parent = Frame3

local progressFill = Instance.new("Frame")
progressFill.Size = UDim2.new(0, 0, 1, 0)
progressFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Barra de carregamento vermelha
progressFill.Parent = Frame3

local UICorner3 = Instance.new("UICorner")
UICorner3.CornerRadius = UDim.new(0, 12)
UICorner3.Parent = progressFill

local percentLabel = Instance.new("TextLabel")
percentLabel.Size = UDim2.new(1, 0, 1, 0)
percentLabel.BackgroundTransparency = 1
percentLabel.Text = "0%"
percentLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
percentLabel.TextSize = 14
percentLabel.Font = Enum.Font.GothamBold
percentLabel.Parent = Frame3

local statusLabel = Instance.new("TextLabel")
statusLabel.Size = UDim2.new(1, 0, 0, 20)
statusLabel.Position = UDim2.new(0, 0, 1, 5)
statusLabel.BackgroundTransparency = 1
statusLabel.Text = "READY"
statusLabel.TextColor3 = Color3.fromRGB(255, 0, 0) -- Texto de status vermelho
statusLabel.TextSize = 12
statusLabel.Font = Enum.Font.GothamBold
statusLabel.Parent = Frame

-- Core Functions
local function updateTopBar()
    local fps = 60
    local ping = 0
    local framesCount = 0
    local last = tick()
    RunService.RenderStepped:Connect(function()
        framesCount += 1
        if tick() - last >= 1 then
            fps = framesCount
            framesCount = 0
            last = tick()
        end
        local network = Stats:FindFirstChild("Network")
        if network and network:FindFirstChild("ServerStatsItem") then
            local dataPing = network.ServerStatsItem:FindFirstChild("Data Ping")
            if dataPing then ping = math.floor(dataPing:GetValue()) end
        end
        infoLabel.Text = "Red Hub | Auto Grab  |  Ping: " .. ping .. "ms  |  FPS: " .. fps -- Nome atualizado no Loop
    end)
end

local function getHRP()
    local c = lp.Character
    if c then return c:FindFirstChild("HumanoidRootPart") or c:FindFirstChild("Torso") or c:FindFirstChild("UpperTorso") end
    return nil
end

local function isMyPlotByName(pn)
    local plots = workspace:FindFirstChild("Plots")
    if not plots then return false end
    local plot = plots:FindFirstChild(pn)
    if not plot then return false end
    local sign = plot:FindFirstChild("PlotSign")
    if sign then
        local yb = sign:FindFirstChild("YourBase")
        if yb and yb:IsA("BillboardGui") then return yb.Enabled == true end
    end
    return false
end

local function findNearestPrompt()
    local hrp = getHRP()
    if not hrp then return nil end
    local plots = workspace:FindFirstChild("Plots")
    if not plots then return nil end
    local nearest, dist = nil, math.huge
    for _, plot in ipairs(plots:GetChildren()) do
        if isMyPlotByName(plot.Name) then continue end
        local pods = plot:FindFirstChild("AnimalPodiums")
        if not pods then continue end
        for _, pod in ipairs(pods:GetChildren()) do
            local base = pod:FindFirstChild("Base")
            if not base then continue end
            local spawn = base:FindFirstChild("Spawn")
            if not spawn then continue end
            local d = (spawn.Position - hrp.Position).Magnitude
            if d <= STEAL_RADIUS and d < dist then
                local att = spawn:FindFirstChild("PromptAttachment")
                if att then
                    for _, p in ipairs(att:GetChildren()) do
                        if p:IsA("ProximityPrompt") and p.ActionText and p.ActionText:find("Steal") then
                            nearest, dist = p, d
                        end
                    end
                end
            end
        end
    end
    return nearest
end

local function updateProgressBar(p)
    progressFill.Size = UDim2.new(p, 0, 1, 0)
    percentLabel.Text = math.floor(p * 100) .. "%"
end

local function executeSteal(prompt)
    if isStealing then return end
    if not StealData[prompt] then
        StealData[prompt] = {hold = {}, trigger = {}, ready = true}
        if getconnections then
            for _, c in ipairs(getconnections(prompt.PromptButtonHoldBegan)) do
                if c.Function then table.insert(StealData[prompt].hold, c.Function) end
            end
            for _, c in ipairs(getconnections(prompt.Triggered)) do
                if c.Function then table.insert(StealData[prompt].trigger, c.Function) end
            end
        end
    end
    local data = StealData[prompt]
    if not data.ready then return end
    data.ready = false
    isStealing = true
    statusLabel.Text = "STEALING..."
    local startTime = tick()
    task.spawn(function()
        for _, f in ipairs(data.hold) do pcall(f) end
        while tick() - startTime < STEAL_DURATION do
            local elapsed = tick() - startTime
            local p = math.clamp(elapsed / STEAL_DURATION, 0, 1)
            updateProgressBar(p)
            task.wait()
        end
        updateProgressBar(1)
        for _, f in ipairs(data.trigger) do pcall(f) end
        task.wait(0.05)
        updateProgressBar(0)
        statusLabel.Text = "READY"
        data.ready = true
        isStealing = false
    end)
end

local heartbeatConn
local function startAutoSteal()
    updateTopBar()
    if heartbeatConn then return end
    heartbeatConn = RunService.Heartbeat:Connect(function()
        if isStealing then return end
        local success, prompt = pcall(findNearestPrompt)
        if success and prompt then pcall(executeSteal, prompt) end
    end)
end

startAutoSteal()
print("Red Hub Auto Grab loaded successfully!") -- Mensagem de console atualizada
