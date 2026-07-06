local P = game:GetService("Players")
local Lp = P.LocalPlayer
local Rs = game:GetService("RunService")
local Uis = game:GetService("UserInputService")
local Ts = game:GetService("TweenService")

local antibatactive = false
local antibatconn = nil

local function startantibat()
	local char = Lp.Character
	if not char then return end
	local root = char:FindFirstChild("HumanoidRootPart")
	if not root then return end
	if antibatconn then antibatconn:Disconnect() end
	antibatconn = Rs.Heartbeat:Connect(function()
		if not root or not root.Parent then return end
		local orig = root.Velocity
		root.Velocity = Vector3.new(1000, root.Velocity.Y, 1000)
		Rs.RenderStepped:Wait()
		root.Velocity = orig
	end)
end

local function stopantibat()
	if antibatconn then antibatconn:Disconnect() antibatconn = nil end
end

Lp.CharacterAdded:Connect(function()
	if antibatactive then stopantibat() task.wait(0.2) startantibat() end
end)

local sg = Instance.new("ScreenGui")
sg.Name = "HanamiAntiBat"
sg.ResetOnSpawn = false
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
sg.Parent = Lp:WaitForChild("PlayerGui")

local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 220, 0, 100)
main.Position = UDim2.new(0.5, -110, 0.2, 0)
main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
main.BorderSizePixel = 0
main.Parent = sg

local maincorner = Instance.new("UICorner")
maincorner.CornerRadius = UDim.new(0, 12)
maincorner.Parent = main

local mainstroke = Instance.new("UIStroke")
mainstroke.Color = Color3.fromRGB(255, 105, 180)
mainstroke.Thickness = 1
mainstroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
mainstroke.Parent = main

local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 32)
header.Position = UDim2.new(0, 0, 0, 0)
header.BackgroundTransparency = 1
header.Parent = main

local headertitle = Instance.new("TextLabel")
headertitle.Size = UDim2.new(1, 0, 1, 0)
headertitle.Position = UDim2.new(0, 0, 0, 0)
headertitle.BackgroundTransparency = 1
headertitle.Text = "Hanami Anti Bat"
headertitle.TextColor3 = Color3.fromRGB(255, 105, 180)
headertitle.Font = Enum.Font.GothamBold
headertitle.TextSize = 18
headertitle.Parent = header

local card = Instance.new("Frame")
card.Size = UDim2.new(1, -20, 0, 52)
card.Position = UDim2.new(0, 10, 0, 38)
card.BackgroundColor3 = Color3.fromRGB(10, 0, 15)
card.BorderSizePixel = 0
card.Parent = main

local cardcorner = Instance.new("UICorner")
cardcorner.CornerRadius = UDim.new(0, 10)
cardcorner.Parent = card

local cardstroke = Instance.new("UIStroke")
cardstroke.Color = Color3.fromRGB(255, 105, 180)
cardstroke.Thickness = 1
cardstroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
cardstroke.Parent = card

local label = Instance.new("TextLabel")
label.Size = UDim2.new(0.6, 0, 0, 22)
label.Position = UDim2.new(0, 14, 0, 8)
label.BackgroundTransparency = 1
label.Text = "PROTECTION"
label.TextColor3 = Color3.fromRGB(255, 105, 180)
label.Font = Enum.Font.GothamBold
label.TextSize = 14
label.TextXAlignment = Enum.TextXAlignment.Left
label.Parent = card

local status = Instance.new("TextLabel")
status.Size = UDim2.new(0.6, 0, 0, 14)
status.Position = UDim2.new(0, 14, 0, 28)
status.BackgroundTransparency = 1
status.Text = "DISABLED"
status.TextColor3 = Color3.fromRGB(220, 90, 160)
status.Font = Enum.Font.GothamBold
status.TextSize = 10
status.TextXAlignment = Enum.TextXAlignment.Left
status.Parent = card

local pillbg = Instance.new("Frame")
pillbg.Size = UDim2.new(0, 40, 0, 22)
pillbg.Position = UDim2.new(1, -54, 0.5, -11)
pillbg.BackgroundColor3 = Color3.fromRGB(30, 0, 30)
pillbg.BorderSizePixel = 0
pillbg.Parent = card

local pillcorner = Instance.new("UICorner")
pillcorner.CornerRadius = UDim.new(1, 0)
pillcorner.Parent = pillbg

local knob = Instance.new("Frame")
knob.Size = UDim2.new(0, 14, 0, 14)
knob.Position = UDim2.new(0, 4, 0.5, -7)
knob.BackgroundColor3 = Color3.fromRGB(180, 60, 130)
knob.BorderSizePixel = 0
knob.Parent = pillbg

local knobcorner = Instance.new("UICorner")
knobcorner.CornerRadius = UDim.new(1, 0)
knobcorner.Parent = knob

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, 0, 1, 0)
btn.BackgroundTransparency = 1
btn.Text = ""
btn.Parent = card

local tweeninfo = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

local function setstate(active)
	if active then
		Ts:Create(cardstroke, tweeninfo, {Color = Color3.fromRGB(255, 105, 180)}):Play()
		status.Text = "ENABLED"
		Ts:Create(status, tweeninfo, {TextColor3 = Color3.fromRGB(255, 105, 180)}):Play()
		Ts:Create(pillbg, tweeninfo, {BackgroundColor3 = Color3.fromRGB(30, 0, 30)}):Play()
		Ts:Create(knob, tweeninfo, {BackgroundColor3 = Color3.fromRGB(255, 105, 180)}):Play()
		Ts:Create(knob, tweeninfo, {Position = UDim2.new(0, 22, 0.5, -7)}):Play()
	else
		Ts:Create(cardstroke, tweeninfo, {Color = Color3.fromRGB(255, 20, 147)}):Play()
		status.Text = "DISABLED"
		Ts:Create(status, tweeninfo, {TextColor3 = Color3.fromRGB(220, 90, 160)}):Play()
		Ts:Create(pillbg, tweeninfo, {BackgroundColor3 = Color3.fromRGB(40, 0, 40)}):Play()
		Ts:Create(knob, tweeninfo, {BackgroundColor3 = Color3.fromRGB(180, 60, 130)}):Play()
		Ts:Create(knob, tweeninfo, {Position = UDim2.new(0, 4, 0.5, -7)}):Play()
	end
end

setstate(false)

btn.MouseButton1Click:Connect(function()
	antibatactive = not antibatactive
	setstate(antibatactive)
	if antibatactive then startantibat() else stopantibat() end
end)

local dragging, draginput, dragstart, startpos = false, nil, nil, nil

header.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragstart = input.Position
		startpos = main.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

header.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		draginput = input
	end
end)

Uis.InputChanged:Connect(function(input)
	if input == draginput and dragging then
		local delta = input.Position - dragstart
		main.Position = UDim2.new(
			startpos.X.Scale, startpos.X.Offset + delta.X,
			startpos.Y.Scale, startpos.Y.Offset + delta.Y
		)
	end
end)local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local HttpService = game:GetService("HttpService")
local ContentProvider = game:GetService("ContentProvider")
local Lighting = game:GetService("Lighting")

local _vibranceCC = nil
local function enableVibrance()
	if not _vibranceCC then
		_vibranceCC = Lighting:FindFirstChildOfClass("ColorCorrectionEffect")
		if not _vibranceCC then
			_vibranceCC = Instance.new("ColorCorrectionEffect", Lighting)
		end
	end
	_vibranceCC.Saturation = 0.6
end
local function disableVibrance()
	if _vibranceCC then
		_vibranceCC.Saturation = 0
	end
end
local LP = Players.LocalPlayer

local State = {
        normalSpeed = 60, carrySpeed = 30, laggerSpeed = 13, laggerCarrySpeed = 13,
        speedType = "normal",
        laggerActive = false,
        laggerCarryActive = false,
        autoBatToggled = false,
        hittingCooldown = false, infJumpEnabled = false,
        antiRagdollEnabled = false, fpsBoostEnabled = false, stretchRezEnabled = false,
        countdownActive = false,
        guiVisible = true,
        isStealing = false, stealStartTime = nil, lastStealTick = 0,
        medusaLastUsed = 0, medusaDebounce = false, medusaCounterEnabled = false,
        dropBrainrotActive = false,
        autoTpDownEnabled = false, autoTpDownY = 20,
        autoLeftEnabled = false, autoRightEnabled = false,
        autoLeftPhase = 1, autoRightPhase = 1,
        _tpInProgress = false,
        lastMoveDir = Vector3.new(0,0,0),
        animEnabled = false, unwalkEnabled = false,
        infJumpMode = "manual",
        dropBrainrotToggled = false,
        tpDownToggled = false,
        -- Keybinds
        keyAutoLeft = Enum.KeyCode.Unknown,
        keyAutoRight = Enum.KeyCode.Unknown,
        keyDropBR = Enum.KeyCode.Unknown,
        keyTpDown = Enum.KeyCode.Unknown,
        keyAutoBat = Enum.KeyCode.Unknown,
}


-- ========== AUTOâ€‘STEAL ==========
local AutoSteal = {
        Enabled = false,
        Radius = 20,
        Duration = 1.3,
        IsStealing = false,
        Data = {},
        ProgressFill = nil,
        ProgressText = nil,
}

local function isMyPlotByName(plotName)
        local plots = workspace:FindFirstChild("Plots")
        if not plots then return false end
        local plot = plots:FindFirstChild(plotName)
        if not plot then return false end
        local sign = plot:FindFirstChild("PlotSign")
        if sign then
                local yb = sign:FindFirstChild("YourBase")
                if yb and yb:IsA("BillboardGui") then return yb.Enabled == true end
        end
        return false
end

local function findNearestPrompt()
        local char = LP.Character
        local root = char and char:FindFirstChild("HumanoidRootPart")
        if not root then return nil end
        local plots = workspace:FindFirstChild("Plots")
        if not plots then return nil end
        local bestPrompt, bestDist, bestName = nil, math.huge, nil
        for _, plot in ipairs(plots:GetChildren()) do
                if isMyPlotByName(plot.Name) then continue end
                local podiums = plot:FindFirstChild("AnimalPodiums")
                if not podiums then continue end
                for _, pod in ipairs(podiums:GetChildren()) do
                        pcall(function()
                                local base = pod:FindFirstChild("Base")
                                local spawn = base and base:FindFirstChild("Spawn")
                                if spawn then
                                        local dist = (spawn.Position - root.Position).Magnitude
                                        if dist < bestDist and dist <= AutoSteal.Radius then
                                                local att = spawn:FindFirstChild("PromptAttachment")
                                                if att then
                                                        for _, child in ipairs(att:GetChildren()) do
                                                                if child:IsA("ProximityPrompt") then
                                                                        bestPrompt, bestDist, bestName = child, dist, pod.Name
                                                                        break
                                                                end
                                                        end
                                                end
                                        end
                                end
                        end)
                end
        end
        return bestPrompt, bestDist, bestName
end

local function executeSteal(prompt, animalName)
        if AutoSteal.IsStealing then return end
        if not AutoSteal.Data[prompt] then
                AutoSteal.Data[prompt] = {hold = {}, trigger = {}, ready = true}
                pcall(function()
                        if getconnections then
                                for _, c in ipairs(getconnections(prompt.PromptButtonHoldBegan)) do
                                        if c.Function then table.insert(AutoSteal.Data[prompt].hold, c.Function) end
                                end
                                for _, c in ipairs(getconnections(prompt.Triggered)) do
                                        if c.Function then table.insert(AutoSteal.Data[prompt].trigger, c.Function) end
                                end
                        end
                end)
        end
        local data = AutoSteal.Data[prompt]
        if not data.ready then return end
        data.ready = false
        AutoSteal.IsStealing = true
        local startTime = tick()

        local conn
        conn = RunService.Heartbeat:Connect(function()
                if not AutoSteal.IsStealing then
                        conn:Disconnect()
                        return
                end
                local prog = math.clamp((tick() - startTime) / AutoSteal.Duration, 0, 1)
                if AutoSteal.ProgressFill then
                        AutoSteal.ProgressFill.Size = UDim2.new(prog, 0, 1, 0)
                end
                if AutoSteal.ProgressText then
                        AutoSteal.ProgressText.Text = math.floor(prog * 100).."%"
                end
        end)

        task.spawn(function()
                for _, f in ipairs(data.hold) do task.spawn(f) end
                task.wait(AutoSteal.Duration)
                for _, f in ipairs(data.trigger) do task.spawn(f) end
                AutoSteal.IsStealing = false
                data.ready = true
                task.wait(0.6)
                if not AutoSteal.IsStealing and AutoSteal.ProgressFill then
                        TweenService:Create(AutoSteal.ProgressFill, TweenInfo.new(0.4), {Size = UDim2.new(0,0,1,0)}):Play()
                end
                if AutoSteal.ProgressText then
                        AutoSteal.ProgressText.Text = "0%"
                end
        end)
end

local autoStealConnection = nil
local function startAutoSteal()
        if autoStealConnection then return end
        autoStealConnection = RunService.Heartbeat:Connect(function()
                if AutoSteal.Enabled and not AutoSteal.IsStealing then
                        if State.countdownActive then return end  -- ANTI-DETECT: Stop during countdown
                        local p, _, name = findNearestPrompt()
                        if p then executeSteal(p, name) end
                end
        end)
end
local function stopAutoSteal()
        if autoStealConnection then
                autoStealConnection:Disconnect()
                autoStealConnection = nil
        end
        AutoSteal.IsStealing = false
        for k, v in pairs(AutoSteal.Data) do
                if v.ready ~= nil then v.ready = true end
        end
end
-- ========== FIN AUTOâ€‘STEAL ==========

local MOVE_KEYS = {
        [Enum.KeyCode.W]=true,[Enum.KeyCode.A]=true,
        [Enum.KeyCode.S]=true,[Enum.KeyCode.D]=true,
        [Enum.KeyCode.Up]=true,[Enum.KeyCode.Left]=true,
        [Enum.KeyCode.Down]=true,[Enum.KeyCode.Right]=true,
}

local DROP_ASCEND_DURATION = 0.2
local DROP_ASCEND_SPEED = 150

local POS = {
        L1 = Vector3.new(-476.48,-6.28,92.73), L2 = Vector3.new(-483.12,-4.95,94.80),
        R1 = Vector3.new(-476.16,-6.52,25.62), R2 = Vector3.new(-483.04,-5.09,23.14),
}

local Conns = {
        autoSteal = nil, antiRag = nil,
        autoLeft = nil, autoRight = nil,
        anchor = {}, progress = nil,
}

local h, hrp, speedLbl
local setAutoLeft, setAutoRight
local setInstaGrab, setAutoBat, setInfJump, setAntiRag, setFps, setMedusaCounter, setAutoTpDown
local setAnimToggle, setUnwalkToggle
local setupMedusaCounter, stopMedusaCounter, startAntiRagdoll, stopAntiRagdoll
local enableAntiLag, disableAntiLag
local antiLagEnabled = false
local removeAccessoriesEnabled = false
local antiLagDescConn = nil
local defLightBrightness, defLightClock, defLightAmbient, defLightExposure
local nightModeEnabled = false
local stretchRezConn = nil

local modeValLbl, normalBox, carryBox, laggerBox, carryLaggerBox
local setSpeedToggleUI, setLaggerToggleUI
local progressRadLbl

local setMenuDropBR, setMenuTpDown

-- ================= AUTOâ€‘SAVE =================
local saveDebounce = false
local function autoSaveConfig()
        if saveDebounce then return end
        saveDebounce = true
        task.delay(0.5, function()
                local cfg = {
                        normalSpeed=State.normalSpeed, carrySpeed=State.carrySpeed,
                        laggerSpeed=State.laggerSpeed, laggerCarrySpeed=State.laggerCarrySpeed,
                        speedType=State.speedType, laggerActive=State.laggerActive,
                        laggerCarryActive=State.laggerCarryActive,
                        autoBatToggled=State.autoBatToggled,
                        autoLeftEnabled=State.autoLeftEnabled, autoRightEnabled=State.autoRightEnabled,
                        autoStealEnabled=AutoSteal.Enabled, grabRadius=AutoSteal.Radius,
                        infJump=State.infJumpEnabled, antiRagdoll=State.antiRagdollEnabled, fpsBoost=State.fpsBoostEnabled,
                        medusaCounter=State.medusaCounterEnabled,
                        animEnabled=State.animEnabled, unwalkEnabled=State.unwalkEnabled,
                        mobileVisible=MobileButtons.Visible,
                        mobileLocked=MobileButtons.Locked,
                        autoTpDown=State.autoTpDownEnabled, autoTpDownY=State.autoTpDownY,
                        infJumpMode=State.infJumpMode,
                        -- Keybinds
                        keyAutoLeft=State.keyAutoLeft.Name,
                        keyAutoRight=State.keyAutoRight.Name,
                        keyDropBR=State.keyDropBR.Name,
                        keyTpDown=State.keyTpDown.Name,
                        keyAutoBat=State.keyAutoBat.Name,
                }
                pcall(function() writefile("HanamiHubConfig.json", HttpService:JSONEncode(cfg)) end)
                saveDebounce = false
        end)
end

-- ================= INSTANT RESET =================

local function doInstantReset()
    local char = LP.Character
    local hum = char and char:FindFirstChildOfClass("Humanoid")
    local hrp = char and char:FindFirstChild("HumanoidRootPart")

    if hum then
        hum.WalkSpeed = 16
    end
    if hrp then
        hrp.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
    end

    local carpet = char and char:FindFirstChild("Flying Carpet")
    if not carpet then
        carpet = LP.Backpack:FindFirstChild("Flying Carpet")
    end
    if carpet then
        carpet:Destroy()
    end

    local camera = workspace.CurrentCamera
    camera.CameraType = Enum.CameraType.Scriptable
    camera.CFrame = camera.CFrame

    if char then
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end

    if hum then
        hum.Health = 0
    end
    if hrp then
        hrp.Velocity = Vector3.new(0, 0, 0)
        hrp.CFrame = CFrame.new(0, 999999, 0)
    end
    if char then
        char:BreakJoints()
    end

    LP.CharacterAdded:Once(function(newChar)
        task.wait(0.1)
        local newHumanoid = newChar:WaitForChild("Humanoid")
        workspace.CurrentCamera.CameraSubject = newHumanoid
        workspace.CurrentCamera.CameraType = Enum.CameraType.Custom
    end)
end

-- ================= BOUTONS MOBILES =================
local MobileButtons = {
        Visible = true,
        Locked = true,
        Frame = nil,
        Containers = {},
        Buttons = {}
}

local function refreshUIToggles()
        if setSpeedToggleUI then setSpeedToggleUI(State.speedType == "carry") end
        if setLaggerToggleUI then setLaggerToggleUI(State.laggerActive) end
        if State.laggerCarryActive then
                modeValLbl.Text = "Lagger Carry"
        elseif State.laggerActive then
                modeValLbl.Text = "Lagger Normal"
        else
                modeValLbl.Text = (State.speedType == "normal") and "Normal" or "Carry"
        end
end

local function deactivateAllSpeedModes()
        if State.speedType == "carry" then
                State.speedType = "normal"
                if MobileButtons.Buttons.carrySpeed then
                        MobileButtons.Buttons.carrySpeed(false)
                end
        end
        if State.laggerActive then
                State.laggerActive = false
                AutoSteal.Enabled = false
                if setInstaGrab then setInstaGrab(false) end
                stopAutoSteal()
                if MobileButtons.Buttons.lagger then
                        MobileButtons.Buttons.lagger(false)
                end
        end
        if State.laggerCarryActive then
                State.laggerCarryActive = false
                AutoSteal.Enabled = false
                if setInstaGrab then setInstaGrab(false) end
                stopAutoSteal()
                if MobileButtons.Buttons.laggerCarry then
                        MobileButtons.Buttons.laggerCarry(false)
                end
        end
end

local function toggleSpeedType()
        if State.speedType == "carry" then
                State.speedType = "normal"
                refreshUIToggles()
                autoSaveConfig()
                if MobileButtons.Buttons.carrySpeed then
                        MobileButtons.Buttons.carrySpeed(false)
                end
                return
        end
        deactivateAllSpeedModes()
        State.speedType = "carry"
        refreshUIToggles()
        autoSaveConfig()
        if MobileButtons.Buttons.carrySpeed then
                MobileButtons.Buttons.carrySpeed(true)
        end
end

local function toggleLagger()
        if State.laggerActive then
                State.laggerActive = false
                AutoSteal.Enabled = false
                if setInstaGrab then setInstaGrab(false) end
                stopAutoSteal()
                refreshUIToggles()
                autoSaveConfig()
                if MobileButtons.Buttons.lagger then
                        MobileButtons.Buttons.lagger(false)
                end
                return
        end
        deactivateAllSpeedModes()
        State.laggerActive = true
        AutoSteal.Enabled = true
        if setInstaGrab then setInstaGrab(true) end
        startAutoSteal()
        refreshUIToggles()
        autoSaveConfig()
        if MobileButtons.Buttons.lagger then
                MobileButtons.Buttons.lagger(true)
        end
end

local function toggleLaggerCarry()
        if State.laggerCarryActive then
                State.laggerCarryActive = false
                AutoSteal.Enabled = false
                if setInstaGrab then setInstaGrab(false) end
                stopAutoSteal()
                refreshUIToggles()
                autoSaveConfig()
                if MobileButtons.Buttons.laggerCarry then
                        MobileButtons.Buttons.laggerCarry(false)
                end
                return
        end
        deactivateAllSpeedModes()
        State.laggerCarryActive = true
        AutoSteal.Enabled = true
        if setInstaGrab then setInstaGrab(true) end
        startAutoSteal()
        refreshUIToggles()
        autoSaveConfig()
        if MobileButtons.Buttons.laggerCarry then
                MobileButtons.Buttons.laggerCarry(true)
        end
end

local function getCurrentSpeed()
        if State.laggerCarryActive then
                return State.laggerCarrySpeed
        elseif State.laggerActive then
                return State.laggerCarrySpeed
        else
                return State.speedType == "normal" and State.normalSpeed or State.carrySpeed
        end
end

local function getAutoMoveSpeed()
        if State.laggerCarryActive then
                return State.normalSpeed
        elseif State.laggerActive then
                return State.laggerSpeed
        else
                return State.normalSpeed
        end
end

local function tpToGround()
        local char = LP.Character
        if not char then return end
        local root = char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        local rot = root.CFrame.Rotation
        local raycastParams = RaycastParams.new()
        raycastParams.FilterType = Enum.RaycastFilterType.Exclude
        raycastParams.FilterDescendantsInstances = {char}
        local rayResult = workspace:Raycast(root.Position, Vector3.new(0, -500, 0), raycastParams)
        if rayResult then
                root.CFrame = CFrame.new(rayResult.Position + Vector3.new(0, 3, 0)) * rot
        else
                root.CFrame = CFrame.new(root.Position + Vector3.new(0, -20, 0)) * rot
        end
end

local function runDropBrainrot()
        if State.dropBrainrotActive then return end
        local char=LP.Character; if not char then return end
        local root=char:FindFirstChild("HumanoidRootPart"); if not root then return end
        State.dropBrainrotActive=true; local t0=tick(); local dc
        dc=RunService.Heartbeat:Connect(function()
                local r=char and char:FindFirstChild("HumanoidRootPart")
                if not r then dc:Disconnect(); State.dropBrainrotActive=false; return end
                if tick()-t0>=DROP_ASCEND_DURATION then
                        dc:Disconnect()
                        local rp=RaycastParams.new(); rp.FilterDescendantsInstances={char}; rp.FilterType=Enum.RaycastFilterType.Exclude
                        local rr=workspace:Raycast(r.Position,Vector3.new(0,-2000,0),rp)
                        if rr then
                                local hum2=char:FindFirstChildOfClass("Humanoid")
                                local off=(hum2 and hum2.HipHeight or 2)+(r.Size.Y/2)
                                r.CFrame=CFrame.new(r.Position.X,rr.Position.Y+off,r.Position.Z); r.AssemblyLinearVelocity=Vector3.new(0,0,0)
                        end
                        State.dropBrainrotActive=false; return
                end
                r.AssemblyLinearVelocity=Vector3.new(r.AssemblyLinearVelocity.X,DROP_ASCEND_SPEED,r.AssemblyLinearVelocity.Z)
        end)
end

-- ================= PANNEAU MOBILE =================
local function createMobilePanel()
	-- ============================================================
	-- CANDY HUB MOBILE BUTTONS (visual shell + Hanami logic wiring)
	-- ============================================================
	local CoreGui = game:GetService("CoreGui")

	local ScreenGui_1 = Instance.new("ScreenGui")
	ScreenGui_1.Name = "CandyHubMobileButtons"
	ScreenGui_1.DisplayOrder = 8
	ScreenGui_1.IgnoreGuiInset = true
	ScreenGui_1.ResetOnSpawn = false
	ScreenGui_1.Parent = CoreGui

	local Frame_1 = Instance.new("Frame")
	Frame_1.Name = "ButtonContainer"
	Frame_1.AnchorPoint = Vector2.new(1, 0)
	Frame_1.BackgroundTransparency = 1
	Frame_1.Position = UDim2.new(1, -20, 0.11999999731779099, 0)
	Frame_1.Size = UDim2.new(0, 144, 0, 200)
	Frame_1.Parent = ScreenGui_1

	local Scale_1 = Instance.new("UIScale")
	Scale_1.Name = "UIScale"
	Scale_1.Scale = 1.149999976158142
	Scale_1.Parent = Frame_1

	-- EditBanner (shown in lock/edit mode)
	local Frame_2 = Instance.new("Frame")
	Frame_2.Name = "EditBanner"
	Frame_2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	Frame_2.BorderSizePixel = 0
	Frame_2.Position = UDim2.new(0, 0, 0, -32)
	Frame_2.Size = UDim2.new(1, 0, 0, 26)
	Frame_2.Visible = false
	Frame_2.ZIndex = 10
	Frame_2.Parent = Frame_1
	do
		local c = Instance.new("UICorner"); c.CornerRadius = UDim.new(1,0); c.Parent = Frame_2
		local Gradient_1 = Instance.new("UIGradient")
		Gradient_1.Color = ColorSequence.new({
			ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 92, 181)),
			ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 180, 255))
		})
		Gradient_1.Rotation = 154
		Gradient_1.Parent = Frame_2
		local sk = Instance.new("UIStroke"); sk.Color = Color3.fromRGB(255,255,255); sk.Transparency = 0.4; sk.Parent = Frame_2
		local tl = Instance.new("TextLabel"); tl.BackgroundTransparency=1; tl.Size=UDim2.new(1,0,1,0)
		tl.ZIndex=11; tl.Font=Enum.Font.GothamBlack; tl.Text="EDIT MODE"
		tl.TextColor3=Color3.fromRGB(15,5,25); tl.TextSize=12; tl.Parent=Frame_2
		-- Spin the gradient
		RunService.RenderStepped:Connect(function()
			Gradient_1.Rotation = (Gradient_1.Rotation + 0.8) % 360
		end)
	end

	-- Helper: build a single Candy-styled button frame
	local function makeCandyBtn(name, labelText, posX, posY, sizeX, sizeY)
		local frame = Instance.new("Frame")
		frame.Name = name
		frame.BackgroundColor3 = Color3.fromRGB(9, 9, 12)
		frame.BorderSizePixel = 0
		frame.Position = UDim2.new(0, posX, 0, posY)
		frame.Size = UDim2.new(0, sizeX, 0, sizeY)
		frame.Parent = Frame_1

		local corner = Instance.new("UICorner"); corner.CornerRadius = UDim.new(0,9); corner.Parent = frame

		local gradLayer = Instance.new("Frame"); gradLayer.Name="GradLayer"
		gradLayer.BackgroundColor3=Color3.fromRGB(255,255,255); gradLayer.BackgroundTransparency=1
		gradLayer.BorderSizePixel=0; gradLayer.Size=UDim2.new(1,0,1,0); gradLayer.Parent=frame
		local gc=Instance.new("UICorner"); gc.CornerRadius=UDim.new(0,9); gc.Parent=gradLayer
		local gl=Instance.new("UIGradient")
		gl.Color=ColorSequence.new({ColorSequenceKeypoint.new(0,Color3.fromRGB(255,92,181)),ColorSequenceKeypoint.new(1,Color3.fromRGB(0,180,255))})
		gl.Parent=gradLayer

		local stroke=Instance.new("UIStroke"); stroke.Color=Color3.fromRGB(24,28,36); stroke.Transparency=0.34; stroke.Parent=frame
		local sg=Instance.new("UIGradient")
		sg.Color=ColorSequence.new({ColorSequenceKeypoint.new(0,Color3.fromRGB(255,92,181)),ColorSequenceKeypoint.new(1,Color3.fromRGB(0,180,255))})
		sg.Parent=stroke

		local flash=Instance.new("Frame"); flash.Name="FlashLayer"; flash.BackgroundColor3=Color3.fromRGB(255,255,255)
		flash.BackgroundTransparency=1; flash.BorderSizePixel=0; flash.Size=UDim2.new(1,0,1,0); flash.ZIndex=2; flash.Parent=frame
		local fc=Instance.new("UICorner"); fc.CornerRadius=UDim.new(0,9); fc.Parent=flash

		local lbl=Instance.new("TextLabel"); lbl.Name="TextLabel"; lbl.BackgroundTransparency=1
		lbl.Position=UDim2.new(0,2,0,2); lbl.Size=UDim2.new(1,-4,1,-4); lbl.ZIndex=3
		lbl.Font=Enum.Font.GothamBlack; lbl.Text=labelText; lbl.TextColor3=Color3.fromRGB(255,182,213)
		lbl.TextSize=9; lbl.TextWrapped=true; lbl.Parent=frame

		local btn=Instance.new("TextButton"); btn.Name="TextButton"; btn.BackgroundTransparency=1
		btn.Size=UDim2.new(1,0,1,0); btn.ZIndex=4; btn.Text=""; btn.Parent=frame

		frame.Visible = MobileButtons.Visible
		table.insert(MobileButtons.Containers, frame)
		return frame, btn, lbl
	end

	-- Build all 9 buttons (Candy Hub layout)
	local fAutoLeft,    bAutoLeft,    lAutoLeft    = makeCandyBtn("AutoLeft",    "AUTO LEFT",     0,   0, 64, 42)
	local fAutoRight,   bAutoRight,   lAutoRight   = makeCandyBtn("AutoRight",   "AUTO RIGHT",   72,   0, 64, 42)
	local fAutoBat,     bAutoBat,     lAutoBat     = makeCandyBtn("AutoBat",     "AIMBOT",        0,  50, 64, 42)
	local fCarrySpd,    bCarrySpd,    lCarrySpd    = makeCandyBtn("CarrySpeed",  "CARRY",        72,  50, 64, 42)
	local fDropBR,      bDropBR,      lDropBR      = makeCandyBtn("DropBrainrot","DROP BR",       0, 100, 64, 42)
	local fTpDown,      bTpDown,      lTpDown      = makeCandyBtn("TPDown",      "TP DOWN",      72, 100, 64, 42)
	local fLaggerCarry, bLaggerCarry, lLaggerCarry = makeCandyBtn("LaggerCarry", "LAGGER CARRY",  0, 150, 64, 42)
	local fLagger,      bLagger,      lLagger      = makeCandyBtn("LaggerSpeed", "LAGGER",       72, 150, 64, 42)
	local fInstReset,   bInstReset,   lInstReset   = makeCandyBtn("InstantReset","INSTANT RESET", 0, 200,136, 42)

	-- Drag the whole container
	do
		local dragging, dragStart, startPos
		Frame_1.InputBegan:Connect(function(input)
			if MobileButtons.Locked then return end
			if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
				dragging = true; dragStart = input.Position; startPos = Frame_1.Position
				input.Changed:Connect(function()
					if input.UserInputState == Enum.UserInputState.End then dragging = false end
				end)
			end
		end)
		UIS.InputChanged:Connect(function(input)
			if not dragging then return end
			if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
				local delta = input.Position - dragStart
				Frame_1.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset+delta.X, startPos.Y.Scale, startPos.Y.Offset+delta.Y)
			end
		end)
	end

	-- Candy Hub toggle/clash visual system
	local activeLightGrey = Color3.fromRGB(255, 182, 213)
	local activeDarkGrey  = Color3.fromRGB(180, 50, 110)
	local inactiveColor   = Color3.fromRGB(9, 9, 12)
	local activeTextClr   = Color3.fromRGB(10, 10, 10)
	local inactiveTextClr = Color3.fromRGB(255, 182, 213)
	local tweenInfo       = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
	local clashLoops      = {}

	local function startClash(frame, label, speed)
		local s = speed or 0.45
		local tw = TweenInfo.new(s, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut)
		local running = true
		label.TextColor3 = activeTextClr
		local function cycle()
			if not running then return end
			TweenService:Create(frame, tw, {BackgroundColor3 = activeLightGrey}):Play()
			task.wait(s)
			if not running then return end
			TweenService:Create(frame, tw, {BackgroundColor3 = activeDarkGrey}):Play()
			task.wait(s)
			if running then cycle() end
		end
		task.spawn(cycle)
		return function() running = false end
	end

	local function setActiveVisual(key, frame, label, state)
		if clashLoops[key] then clashLoops[key](); clashLoops[key] = nil end
		if state then
			clashLoops[key] = startClash(frame, label, nil)
		else
			TweenService:Create(frame, tweenInfo, {BackgroundColor3 = inactiveColor}):Play()
			label.TextColor3 = inactiveTextClr
		end
	end

	local function fireTimed(key, frame, label, duration)
		if clashLoops[key] then clashLoops[key](); clashLoops[key] = nil end
		clashLoops[key] = startClash(frame, label, 0.18)
		task.spawn(function()
			task.wait(duration)
			if clashLoops[key] then clashLoops[key](); clashLoops[key] = nil end
			TweenService:Create(frame, tweenInfo, {BackgroundColor3 = inactiveColor}):Play()
			label.TextColor3 = inactiveTextClr
		end)
	end

	-- Click-scale physics
	local function setupPhysics(btn)
		local origSize = btn.Size
		local clickSize = UDim2.new(origSize.X.Scale*0.95, origSize.X.Offset*0.95, origSize.Y.Scale*0.95, origSize.Y.Offset*0.95)
		local info = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
		btn.MouseButton1Down:Connect(function() TweenService:Create(btn, info, {Size=clickSize}):Play() end)
		btn.MouseButton1Up:Connect(function()   TweenService:Create(btn, info, {Size=origSize}):Play()  end)
	end
	for _, b in ipairs({bAutoLeft,bAutoRight,bAutoBat,bCarrySpd,bDropBR,bTpDown,bLaggerCarry,bLagger,bInstReset}) do
		setupPhysics(b)
	end

	-- Wire buttons to Hanami logic
	bAutoLeft.MouseButton1Click:Connect(function()
		State.autoLeftEnabled = not State.autoLeftEnabled
		if setAutoLeft then setAutoLeft(State.autoLeftEnabled) end
		if State.autoLeftEnabled then startAutoLeft() else stopAutoLeft() end
		setActiveVisual("autoLeft", fAutoLeft, lAutoLeft, State.autoLeftEnabled)
		autoSaveConfig()
	end)

	bAutoRight.MouseButton1Click:Connect(function()
		State.autoRightEnabled = not State.autoRightEnabled
		if setAutoRight then setAutoRight(State.autoRightEnabled) end
		if State.autoRightEnabled then startAutoRight() else stopAutoRight() end
		setActiveVisual("autoRight", fAutoRight, lAutoRight, State.autoRightEnabled)
		autoSaveConfig()
	end)

	bAutoBat.MouseButton1Click:Connect(function()
		State.autoBatToggled = not State.autoBatToggled
		setAutoBat(State.autoBatToggled)
		setActiveVisual("autoBat", fAutoBat, lAutoBat, State.autoBatToggled)
		autoSaveConfig()
	end)

	bCarrySpd.MouseButton1Click:Connect(function()
		toggleSpeedType()
		setActiveVisual("carrySpd", fCarrySpd, lCarrySpd, State.speedType == "carry")
	end)

	bDropBR.MouseButton1Click:Connect(function()
		runDropBrainrot()
		if setMenuDropBR then setMenuDropBR(true) end
		fireTimed("dropBR", fDropBR, lDropBR, 0.5)
		task.delay(0.5, function() if setMenuDropBR then setMenuDropBR(false) end end)
	end)

	bTpDown.MouseButton1Click:Connect(function()
		tpToGround()
		if setMenuTpDown then setMenuTpDown(true) end
		fireTimed("tpDown", fTpDown, lTpDown, 0.5)
		task.delay(0.5, function() if setMenuTpDown then setMenuTpDown(false) end end)
	end)

	bLaggerCarry.MouseButton1Click:Connect(function()
		toggleLaggerCarry()
		setActiveVisual("laggerCarry", fLaggerCarry, lLaggerCarry, State.laggerCarryActive)
		setActiveVisual("lagger",      fLagger,      lLagger,      State.laggerActive)
	end)

	bLagger.MouseButton1Click:Connect(function()
		toggleLagger()
		setActiveVisual("lagger",      fLagger,      lLagger,      State.laggerActive)
		setActiveVisual("laggerCarry", fLaggerCarry, lLaggerCarry, State.laggerCarryActive)
	end)

	bInstReset.MouseButton1Click:Connect(function()
		doInstantReset()
		fireTimed("instReset", fInstReset, lInstReset, 0.5)
	end)

	-- MobileButtons.Buttons API (used by menu toggles)
	MobileButtons.Buttons = {
		autoLeft    = function(state) setActiveVisual("autoLeft",    fAutoLeft,    lAutoLeft,    state) end,
		autoRight   = function(state) setActiveVisual("autoRight",   fAutoRight,   lAutoRight,   state) end,
		autoBat     = function(state) setActiveVisual("autoBat",     fAutoBat,     lAutoBat,     state) end,
		carrySpeed  = function(state) setActiveVisual("carrySpd",    fCarrySpd,    lCarrySpd,    state) end,
		lagger      = function(state) setActiveVisual("lagger",      fLagger,      lLagger,      state) end,
		laggerCarry = function(state) setActiveVisual("laggerCarry", fLaggerCarry, lLaggerCarry, state) end,
		dropBR      = function(state)
			if state then fireTimed("dropBR", fDropBR, lDropBR, 0.5)
			else setActiveVisual("dropBR", fDropBR, lDropBR, false) end
		end,
		tpDown      = function(state)
			if state then fireTimed("tpDown", fTpDown, lTpDown, 0.5)
			else setActiveVisual("tpDown", fTpDown, lTpDown, false) end
		end,
	}

	-- Sync visuals to current State on load
	task.spawn(function()
		task.wait(0.1)
		MobileButtons.Buttons.autoLeft(State.autoLeftEnabled)
		MobileButtons.Buttons.autoRight(State.autoRightEnabled)
		MobileButtons.Buttons.autoBat(State.autoBatToggled)
		MobileButtons.Buttons.carrySpeed(State.speedType == "carry")
		MobileButtons.Buttons.lagger(State.laggerActive)
		MobileButtons.Buttons.laggerCarry(State.laggerCarryActive)
	end)

	return ScreenGui_1
end
-- ================= INTERFACE GRAPHIQUE =================
local C_BG      = Color3.fromRGB(0,0,0)
local C_PANEL   = Color3.fromRGB(10,0,8)
local C_ROW     = Color3.fromRGB(60,10,40)
local C_ROW_HOV = Color3.fromRGB(80,15,55)
local C_BORDER  = Color3.fromRGB(180,60,120)
local C_BORDER2 = Color3.fromRGB(220,80,150)
local C_HEADER  = Color3.fromRGB(5,0,4)
local C_ACCENT  = Color3.fromRGB(255,182,215)
local C_ACCENT2 = Color3.fromRGB(255,140,190)
local C_DIM     = Color3.fromRGB(200,100,160)
local C_WHITE   = Color3.fromRGB(255,200,230)
local C_ON_BG   = Color3.fromRGB(120,30,80)
local C_OFF_BG  = Color3.fromRGB(40,5,25)
local C_KEY_BG  = Color3.fromRGB(0,0,0)

local Anims = {
        idle1    = "rbxassetid://133806214992291",
        idle2    = "rbxassetid://94970088341563",
        walk     = "rbxassetid://707897309",
        run      = "rbxassetid://707861613",
        jump     = "rbxassetid://116936326516985",
        fall     = "rbxassetid://116936326516985",
        climb    = "rbxassetid://116936326516985",
        swim     = "rbxassetid://116936326516985",
        swimidle = "rbxassetid://116936326516985",
}
task.spawn(function()
        pcall(function()
                ContentProvider:PreloadAsync({
                        Anims.idle1, Anims.idle2, Anims.walk, Anims.run,
                        Anims.jump, Anims.fall, Anims.climb, Anims.swim, Anims.swimidle,
                })
        end)
end)

local animHeartbeatConn = nil
local savedAnimate = nil
local originalAnims = nil

local function isPackAnim(id)
        if not id then return false end
        for _, v in pairs(Anims) do if v == id then return true end end
        return false
end

local function saveOriginalAnims(char)
        local animate = char:FindFirstChild("Animate")
        if not animate then return end
        local function g(obj) return obj and obj.AnimationId or nil end
        local ids = {
                idle1    = g(animate.idle     and animate.idle.Animation1),
                idle2    = g(animate.idle     and animate.idle.Animation2),
                walk     = g(animate.walk     and animate.walk.WalkAnim),
                run      = g(animate.run      and animate.run.RunAnim),
                jump     = g(animate.jump     and animate.jump.JumpAnim),
                fall     = g(animate.fall     and animate.fall.FallAnim),
                climb    = g(animate.climb    and animate.climb.ClimbAnim),
                swim     = g(animate.swim     and animate.swim.Swim),
                swimidle = g(animate.swimidle and animate.swimidle.SwimIdle),
        }
        if not isPackAnim(ids.walk) then originalAnims = ids end
end

local function applyAnimPack(char)
        local animate = char:FindFirstChild("Animate")
        if not animate then return end
        local function s(obj, id) if obj then obj.AnimationId = id end end
        s(animate.idle     and animate.idle.Animation1,     Anims.idle1)
        s(animate.idle     and animate.idle.Animation2,     Anims.idle2)
        s(animate.walk     and animate.walk.WalkAnim,       Anims.walk)
        s(animate.run      and animate.run.RunAnim,         Anims.run)
        s(animate.jump     and animate.jump.JumpAnim,       Anims.jump)
        s(animate.fall     and animate.fall.FallAnim,       Anims.fall)
        s(animate.climb    and animate.climb.ClimbAnim,     Anims.climb)
        s(animate.swim     and animate.swim.Swim,           Anims.swim)
        s(animate.swimidle and animate.swimidle.SwimIdle,   Anims.swimidle)
end

local function restoreOriginalAnims(char)
        if not originalAnims then return end
        local animate = char:FindFirstChild("Animate")
        if not animate then return end
        local function s(obj, id) if obj and id then obj.AnimationId = id end end
        s(animate.idle     and animate.idle.Animation1,     originalAnims.idle1)
        s(animate.idle     and animate.idle.Animation2,     originalAnims.idle2)
        s(animate.walk     and animate.walk.WalkAnim,       originalAnims.walk)
        s(animate.run      and animate.run.RunAnim,         originalAnims.run)
        s(animate.jump     and animate.jump.JumpAnim,       originalAnims.jump)
        s(animate.fall     and animate.fall.FallAnim,       originalAnims.fall)
        s(animate.climb    and animate.climb.ClimbAnim,     originalAnims.climb)
        s(animate.swim     and animate.swim.Swim,           originalAnims.swim)
        s(animate.swimidle and animate.swimidle.SwimIdle,   originalAnims.swimidle)
        local hum2 = char:FindFirstChildOfClass("Humanoid")
        if hum2 then
                for _, track in ipairs(hum2:GetPlayingAnimationTracks()) do track:Stop(0) end
                hum2:ChangeState(Enum.HumanoidStateType.Running)
        end
end

local function startAnimToggle()
        if animHeartbeatConn then animHeartbeatConn:Disconnect(); animHeartbeatConn = nil end
        local char = LP.Character
        if char then
                saveOriginalAnims(char)
                applyAnimPack(char)
                local hum2 = char:FindFirstChildOfClass("Humanoid")
                if hum2 then
                        for _, track in ipairs(hum2:GetPlayingAnimationTracks()) do track:Stop(0) end
                        hum2:ChangeState(Enum.HumanoidStateType.Running)
                end
        end
        animHeartbeatConn = RunService.Heartbeat:Connect(function()
                if not State.animEnabled then return end
                local c = LP.Character
                if c then applyAnimPack(c) end
        end)
end

local function stopAnimToggle()
        if animHeartbeatConn then animHeartbeatConn:Disconnect(); animHeartbeatConn = nil end
        local char = LP.Character
        if char then restoreOriginalAnims(char) end
end

local function startUnwalk()
        if State.unwalkEnabled then return end
        State.unwalkEnabled = true
        local c = LP.Character
        if not c then return end
        local hum = c:FindFirstChildOfClass("Humanoid")
        if hum then for _, t in ipairs(hum:GetPlayingAnimationTracks()) do t:Stop() end end
        local anim = c:FindFirstChild("Animate")
        if anim then savedAnimate = anim:Clone(); anim:Destroy() end
end

local function stopUnwalk()
        if not State.unwalkEnabled then return end
        State.unwalkEnabled = false
        local c = LP.Character
        if c and savedAnimate then
                savedAnimate.Parent = c; savedAnimate.Disabled = false; savedAnimate = nil
        end
        task.spawn(function()
                task.wait(0.15)
                local char = LP.Character
                if not char then return end
                if State.animEnabled then saveOriginalAnims(char); applyAnimPack(char)
                else restoreOriginalAnims(char) end
        end)
end

for _, name in pairs({"JispiHubGUI","HanamiHubGUI"}) do
        local old = game:GetService("CoreGui"):FindFirstChild(name)
        if old then old:Destroy() end
        local old2 = LP:FindFirstChild("PlayerGui") and LP.PlayerGui:FindFirstChild(name)
        if old2 then old2:Destroy() end
end

local closeBtnRef = nil
local miniButtonRef = nil
local miniBtn = nil

local function makeMainDraggable(frame)
        local dragging, dragInput, dragStart, startPos = false, nil, nil, nil
        local startCloseBtnPos
        
        frame.InputBegan:Connect(function(inp)
                if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
                        dragging = true
                        dragStart = inp.Position
                        startPos = frame.Position
                        if closeBtnRef then startCloseBtnPos = closeBtnRef.Position end
                        inp.Changed:Connect(function()
                                if inp.UserInputState == Enum.UserInputState.End or inp.UserInputState == Enum.UserInputState.Cancelled then
                                        dragging = false
                                end
                        end)
                end
        end)
        
        frame.InputChanged:Connect(function(inp)
                if inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch then
                        dragInput = inp
                end
        end)
        
        UIS.InputChanged:Connect(function(inp)
                if inp == dragInput and dragging then
                        local dx = inp.Position.X - dragStart.X
                        local dy = inp.Position.Y - dragStart.Y
                        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + dx, startPos.Y.Scale, startPos.Y.Offset + dy)
                        if closeBtnRef and startCloseBtnPos then
                                closeBtnRef.Position = UDim2.new(startCloseBtnPos.X.Scale, startCloseBtnPos.X.Offset + dx, startCloseBtnPos.Y.Scale, startCloseBtnPos.Y.Offset + dy)
                        end
                end
        end)
end

local function makeMiniDraggable(frame)
        local dragging, dragInput, dragStart, startPos = false, nil, nil, nil
        
        frame.InputBegan:Connect(function(inp)
                if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
                        dragging = true
                        dragStart = inp.Position
                        startPos = frame.Position
                        inp.Changed:Connect(function()
                                if inp.UserInputState == Enum.UserInputState.End or inp.UserInputState == Enum.UserInputState.Cancelled then
                                        dragging = false
                                end
                        end)
                end
        end)
        
        frame.InputChanged:Connect(function(inp)
                if inp.UserInputType == Enum.UserInputType.MouseMovement or inp.UserInputType == Enum.UserInputType.Touch then
                        dragInput = inp
                end
        end)
        
        UIS.InputChanged:Connect(function(inp)
                if inp == dragInput and dragging then
                        local dx = inp.Position.X - dragStart.X
                        local dy = inp.Position.Y - dragStart.Y
                        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + dx, startPos.Y.Scale, startPos.Y.Offset + dy)
                end
        end)
end

local gui = Instance.new("ScreenGui")
gui.Name="HanamiHubGUI"; gui.ResetOnSpawn=false; gui.DisplayOrder=10
gui.IgnoreGuiInset=true; gui.Parent=LP:WaitForChild("PlayerGui")

-- ========================================================================
-- AUTO STEAL INDICATOR - Square / slightly rounded corners, like the image
-- Left: %, Right: Steal Radius, white fill bar
-- ========================================================================
local stealProgressBar = Instance.new("Frame", gui)
stealProgressBar.Size = UDim2.new(0, 280, 0, 52)
stealProgressBar.Position = UDim2.new(0.5, -140, 1, -80)
stealProgressBar.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
stealProgressBar.BackgroundTransparency = 0
stealProgressBar.BorderSizePixel = 0
stealProgressBar.ZIndex = 100
-- Slightly rounded corners only on left side (square look, small corner radius)
local spCorner = Instance.new("UICorner", stealProgressBar)
spCorner.CornerRadius = UDim.new(0, 16)

-- Left: big % label
local pctLbl = Instance.new("TextLabel", stealProgressBar)
pctLbl.Size = UDim2.new(0.42, 0, 0, 28)
pctLbl.Position = UDim2.new(0, 14, 0, 7)
pctLbl.BackgroundTransparency = 1
pctLbl.Text = "0%"
pctLbl.TextColor3 = Color3.fromRGB(255,150,200)
pctLbl.Font = Enum.Font.GothamBlack
pctLbl.TextSize = 24
pctLbl.TextXAlignment = Enum.TextXAlignment.Left
pctLbl.ZIndex = 101

-- Right: "Steal Radius" label
progressRadLbl = Instance.new("TextLabel", stealProgressBar)
progressRadLbl.Size = UDim2.new(0.58, -14, 0, 28)
progressRadLbl.Position = UDim2.new(0.42, 0, 0, 7)
progressRadLbl.BackgroundTransparency = 1
progressRadLbl.Text = "Steal Radius: "..AutoSteal.Radius
progressRadLbl.TextColor3 = Color3.fromRGB(255,150,200)
progressRadLbl.Font = Enum.Font.GothamBlack
progressRadLbl.TextSize = 18
progressRadLbl.TextXAlignment = Enum.TextXAlignment.Right
progressRadLbl.ZIndex = 101

-- Track (dark pill behind bar)
local barTrack = Instance.new("Frame", stealProgressBar)
barTrack.Size = UDim2.new(1, -28, 0, 11)
barTrack.Position = UDim2.new(0, 14, 0, 33)
barTrack.BackgroundColor3 = Color3.fromRGB(28, 28, 28)
barTrack.BorderSizePixel = 0
barTrack.ZIndex = 101
Instance.new("UICorner", barTrack).CornerRadius = UDim.new(1, 0)

-- Fill: WHITE like the image
local barFill = Instance.new("Frame", barTrack)
barFill.Size = UDim2.new(0, 0, 1, 0)
barFill.BackgroundColor3 = Color3.fromRGB(255,100,180)
barFill.BorderSizePixel = 0
barFill.ZIndex = 102
Instance.new("UICorner", barFill).CornerRadius = UDim.new(1, 0)

AutoSteal.ProgressFill = barFill
AutoSteal.ProgressText = pctLbl

local function makeStealBarDraggable(frame)
    local dragging = false
    local dragInput = nil
    local dragStart = nil
    local startPos = nil
    local activeDragConn = nil

    local function stopDrag()
        dragging = false
        dragInput = nil
        dragStart = nil
        startPos = nil
        if activeDragConn then
            activeDragConn:Disconnect()
            activeDragConn = nil
        end
    end

    local function startDragWatch()
        if activeDragConn then activeDragConn:Disconnect() end
        activeDragConn = RunService.Heartbeat:Connect(function()
            if dragging and MobileButtons.Locked then
                stopDrag()
            end
        end)
    end

    frame.InputBegan:Connect(function(input)
        if MobileButtons.Locked then return end
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position
            startDragWatch()
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End or input.UserInputState == Enum.UserInputState.Cancelled then
                    stopDrag()
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if not dragging then return end
        if MobileButtons.Locked then
            stopDrag()
            return
        end
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    UIS.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            if MobileButtons.Locked then
                stopDrag()
                return
            end
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

makeStealBarDraggable(stealProgressBar)

local function saveStealBarPosition()
    local pos = stealProgressBar.Position
    local str = string.format("%.3f,%.1f,%.3f,%.1f", pos.X.Scale, pos.X.Offset, pos.Y.Scale, pos.Y.Offset)
    pcall(function() writefile("HanamiStealBarPos.txt", str) end)
end

local function loadStealBarPosition()
    local savedPos = nil
    pcall(function() savedPos = readfile("HanamiStealBarPos.txt") end)
    if savedPos and savedPos ~= "" then
        local parts = {}
        for part in string.gmatch(savedPos, "[^,]+") do table.insert(parts, part) end
        if #parts >= 4 then
            stealProgressBar.Position = UDim2.new(tonumber(parts[1]), tonumber(parts[2]), tonumber(parts[3]), tonumber(parts[4]))
        end
    end
end

local lastStealBarSave = 0
stealProgressBar:GetPropertyChangedSignal("Position"):Connect(function()
    if tick() - lastStealBarSave > 0.5 then
        lastStealBarSave = tick()
        saveStealBarPosition()
    end
end)
-- ========================================================================

local main = Instance.new("Frame",gui)
main.Name="Main"; main.Size=UDim2.new(0,260,0,450)
main.Position=UDim2.new(0,20,0,20)
main.BackgroundColor3=C_BG; main.BorderSizePixel=0; main.Active=true; main.ClipsDescendants=true
Instance.new("UICorner",main).CornerRadius = UDim.new(0, 14)
local mainStroke = Instance.new("UIStroke",main); mainStroke.Color=Color3.fromRGB(255,100,180); mainStroke.Thickness=2


local function saveMainPosition()
        local pos = main.Position
        local str = string.format("%.3f,%.1f,%.3f,%.1f", pos.X.Scale, pos.X.Offset, pos.Y.Scale, pos.Y.Offset)
        pcall(function() writefile("HanamiHubGUIPos.txt", str) end)
end

local function saveMiniPosition()
        if miniBtn then
                local pos = miniBtn.Position
                local str = string.format("%.3f,%.1f,%.3f,%.1f", pos.X.Scale, pos.X.Offset, pos.Y.Scale, pos.Y.Offset)
                pcall(function() writefile("HanamiMiniPos.txt", str) end)
        end
end

local function loadMainPosition()
        local savedPos = nil
        pcall(function() savedPos = readfile("HanamiHubGUIPos.txt") end)
        if savedPos and savedPos ~= "" then
                local parts = {}
                for part in string.gmatch(savedPos, "[^,]+") do table.insert(parts, part) end
                if #parts >= 4 then
                        main.Position = UDim2.new(tonumber(parts[1]), tonumber(parts[2]), tonumber(parts[3]), tonumber(parts[4]))
                end
        end
end

local function loadMiniPosition()
        local savedPos = nil
        pcall(function() savedPos = readfile("HanamiMiniPos.txt") end)
        if savedPos and savedPos ~= "" and miniBtn then
                local parts = {}
                for part in string.gmatch(savedPos, "[^,]+") do table.insert(parts, part) end
                if #parts >= 4 then
                        miniBtn.Position = UDim2.new(tonumber(parts[1]), tonumber(parts[2]), tonumber(parts[3]), tonumber(parts[4]))
                end
        end
end

local header = Instance.new("Frame",main)
header.Size=UDim2.new(1,0,0,52); header.BackgroundColor3=C_HEADER; header.BorderSizePixel=0; header.ZIndex=5
Instance.new("UICorner",header).CornerRadius = UDim.new(0, 14)
local headerDiv = Instance.new("Frame",header)
headerDiv.Size=UDim2.new(1,0,0,1); headerDiv.Position=UDim2.new(0,0,1,-1)
headerDiv.BackgroundColor3=C_BORDER; headerDiv.BorderSizePixel=0; headerDiv.ZIndex=6

local titleLbl = Instance.new("TextLabel",header)
titleLbl.Size=UDim2.new(1,-50,0,22); titleLbl.Position=UDim2.new(0,14,0.5,-11)
titleLbl.BackgroundTransparency=1; titleLbl.Text="HANAMI HUB"
titleLbl.TextColor3=Color3.fromRGB(255,150,200); titleLbl.Font=Enum.Font.GothamBlack; titleLbl.TextSize=16
titleLbl.TextXAlignment=Enum.TextXAlignment.Left; titleLbl.ZIndex=6

local closeBtn = Instance.new("TextButton",gui)
closeBtn.Size=UDim2.new(0,26,0,26)
closeBtn.BackgroundColor3=Color3.fromRGB(35,35,35); closeBtn.BorderSizePixel=0
closeBtn.Text="X"; closeBtn.TextColor3=Color3.fromRGB(255,150,200)
closeBtn.Font=Enum.Font.GothamBlack; closeBtn.TextSize=12; closeBtn.ZIndex=50
Instance.new("UICorner",closeBtn).CornerRadius=UDim.new(1,0)
local closeBtnStroke = Instance.new("UIStroke",closeBtn)
closeBtnStroke.Color=C_BORDER; closeBtnStroke.Thickness=1
closeBtnRef = closeBtn

closeBtn.MouseEnter:Connect(function()
        TweenService:Create(closeBtn,TweenInfo.new(0.12),{BackgroundColor3=Color3.fromRGB(180,40,40),TextColor3=C_WHITE}):Play()
        TweenService:Create(closeBtnStroke,TweenInfo.new(0.12),{Color=Color3.fromRGB(220,60,60)}):Play()
end)
closeBtn.MouseLeave:Connect(function()
        TweenService:Create(closeBtn,TweenInfo.new(0.12),{BackgroundColor3=Color3.fromRGB(40,40,40),TextColor3=Color3.fromRGB(255,150,200)}):Play()
        TweenService:Create(closeBtnStroke,TweenInfo.new(0.12),{Color=C_BORDER2}):Play()
end)
closeBtn.MouseButton1Click:Connect(function()
        main.Visible = false
        closeBtn.Visible = false
        if miniBtn then miniBtn.Visible = true end
end)

-- CHANGE: mini button moved slightly lower (was Y offset 56, now 66)
miniBtn = Instance.new("TextButton",gui)
miniBtn.Name = "HanamiMiniButton"
miniBtn.Size = UDim2.new(0,150,0,30)
miniBtn.Position = UDim2.new(0,8,0,66)
miniBtn.BackgroundColor3 = Color3.fromRGB(0,0,0)
miniBtn.BackgroundTransparency = 0
miniBtn.BorderSizePixel = 0
miniBtn.Text = "HANAMI HUB"
miniBtn.TextColor3 = Color3.fromRGB(255,150,200)
miniBtn.Font = Enum.Font.GothamBlack
miniBtn.TextSize = 11
miniBtn.AutoButtonColor = false
miniBtn.Visible = false
miniBtn.ZIndex = 50
Instance.new("UICorner", miniBtn).CornerRadius = UDim.new(0, 6)

miniBtn.MouseButton1Click:Connect(function()
        main.Visible = true
        closeBtn.Visible = true
        miniBtn.Visible = false
end)

miniBtn.MouseEnter:Connect(function()
        TweenService:Create(miniBtn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(20,20,20)}):Play()
end)
miniBtn.MouseLeave:Connect(function()
        TweenService:Create(miniBtn, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(0,0,0)}):Play()
end)

miniButtonRef = miniBtn

makeMainDraggable(main)
makeMiniDraggable(miniBtn)

local lastSaveTime = 0
main:GetPropertyChangedSignal("Position"):Connect(function()
        if tick() - lastSaveTime > 0.5 then
                lastSaveTime = tick()
                saveMainPosition()
        end
end)

local lastMiniSaveTime = 0
miniBtn:GetPropertyChangedSignal("Position"):Connect(function()
        if tick() - lastMiniSaveTime > 0.5 then
                lastMiniSaveTime = tick()
                saveMiniPosition()
        end
end)

local function updateCloseButtonPosition()
        if main.Visible then
                local mainPos = main.Position
                closeBtn.Position = UDim2.new(mainPos.X.Scale, mainPos.X.Offset + 260 - 34, mainPos.Y.Scale, mainPos.Y.Offset + 13)
        end
end

main:GetPropertyChangedSignal("Position"):Connect(updateCloseButtonPosition)
main:GetPropertyChangedSignal("Visible"):Connect(updateCloseButtonPosition)

local scroll = Instance.new("ScrollingFrame",main)
scroll.Size=UDim2.new(1,0,1,-52); scroll.Position=UDim2.new(0,0,0,52)
scroll.BackgroundTransparency=1; scroll.BorderSizePixel=0; scroll.ScrollBarThickness=2
scroll.ScrollBarImageColor3=C_BORDER2; scroll.AutomaticCanvasSize=Enum.AutomaticSize.Y
scroll.CanvasSize=UDim2.new(0,0,0,0); scroll.ZIndex=2
local listLayout = Instance.new("UIListLayout",scroll)
listLayout.SortOrder=Enum.SortOrder.LayoutOrder; listLayout.Padding=UDim.new(0,2)
local pad = Instance.new("UIPadding",scroll)
pad.PaddingLeft=UDim.new(0,8); pad.PaddingRight=UDim.new(0,8)
pad.PaddingTop=UDim.new(0,8); pad.PaddingBottom=UDim.new(0,10)

local lo = 0
local function LO() lo+=1; return lo end

local function makeGap(px)
        local f=Instance.new("Frame",scroll); f.Size=UDim2.new(1,0,0,px or 4)
        f.BackgroundTransparency=1; f.BorderSizePixel=0; f.LayoutOrder=LO()
end
local function makeDivider()
        local f=Instance.new("Frame",scroll); f.Size=UDim2.new(1,0,0,1)
        f.BackgroundColor3=C_BORDER; f.BorderSizePixel=0; f.LayoutOrder=LO()
end
local function makeSectionLabel(text)
        local row=Instance.new("Frame",scroll); row.Size=UDim2.new(1,0,0,22)
        row.BackgroundTransparency=1; row.BorderSizePixel=0; row.LayoutOrder=LO()
        local lbl=Instance.new("TextLabel",row); lbl.Size=UDim2.new(1,0,1,0)
        lbl.BackgroundTransparency=1; lbl.Text=text:upper(); lbl.TextColor3=C_DIM
        lbl.Font=Enum.Font.GothamBold; lbl.TextSize=10; lbl.TextXAlignment=Enum.TextXAlignment.Left
end

local function makeInputRow(label, default, onChange)
        local row = Instance.new("Frame", scroll)
        row.Size = UDim2.new(1,0,0,34)
        row.BackgroundColor3 = C_ROW
        row.BorderSizePixel = 0
        row.LayoutOrder = LO()
        Instance.new("UICorner",row).CornerRadius = UDim.new(0,8)
        
        local lbl = Instance.new("TextLabel",row)
        lbl.Size = UDim2.new(0.55,0,1,0)
        lbl.Position = UDim2.new(0,12,0,0)
        lbl.BackgroundTransparency = 1
        lbl.Text = label
        lbl.TextColor3 = C_ACCENT
        lbl.Font = Enum.Font.GothamBold
        lbl.TextSize = 12
        lbl.TextXAlignment = Enum.TextXAlignment.Left
        lbl.ZIndex = 2
        
        local box = Instance.new("TextBox",row)
        box.Size = UDim2.new(0,80,0,26)
        box.Position = UDim2.new(1,-86,0.5,-13)
        box.BackgroundColor3 = Color3.fromRGB(0,0,0)
        box.BorderSizePixel = 0
        box.Text = tostring(default)
        box.TextColor3 = C_ACCENT
        box.Font = Enum.Font.GothamBold
        box.TextSize = 14
        box.ClearTextOnFocus = true
        box.PlaceholderText = "0"
        box.ZIndex = 3
        Instance.new("UICorner",box).CornerRadius = UDim.new(0,6)
        local bs = Instance.new("UIStroke",box)
        bs.Color = C_BORDER2
        bs.Thickness = 1
        
        box.InputBegan:Connect(function(input)
                input:StopPropagation()
        end)
        
        box.Focused:Connect(function()
                TweenService:Create(bs,TweenInfo.new(0.15),{Color=C_ACCENT2}):Play()
                box.BackgroundColor3 = Color3.fromRGB(8,8,8)
        end)
        
        box.FocusLost:Connect(function(enterPressed)
                TweenService:Create(bs,TweenInfo.new(0.15),{Color=C_BORDER2}):Play()
                box.BackgroundColor3 = Color3.fromRGB(0,0,0)
                local num = tonumber(box.Text)
                if num ~= nil then
                        local finalVal = math.floor(math.clamp(num, 0, 500))
                        box.Text = tostring(finalVal)
                        if onChange then onChange(tostring(finalVal)) end
                        autoSaveConfig()
                else
                        box.Text = tostring(default)
                end
        end)
        
        box.Active = true
        box.Selectable = true
        
        row.MouseEnter:Connect(function() 
                TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=C_ROW_HOV}):Play() 
        end)
        row.MouseLeave:Connect(function() 
                TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=C_ROW}):Play() 
        end)
        
        return box
end

local function makeStatusRow(label, valTxt)
        local row=Instance.new("Frame",scroll); row.Size=UDim2.new(1,0,0,28); row.BackgroundColor3=C_ROW
        row.BorderSizePixel=0; row.LayoutOrder=LO()
        Instance.new("UICorner",row).CornerRadius=UDim.new(0,8)
        local lbl=Instance.new("TextLabel",row); lbl.Size=UDim2.new(0.5,0,1,0); lbl.Position=UDim2.new(0,12,0,0)
        lbl.BackgroundTransparency=1; lbl.Text=label; lbl.TextColor3=C_ACCENT; lbl.Font=Enum.Font.GothamBold
        lbl.TextSize=12; lbl.TextXAlignment=Enum.TextXAlignment.Left
        local val=Instance.new("TextLabel",row); val.Size=UDim2.new(0.45,-10,1,0); val.Position=UDim2.new(0.52,0,0,0)
        val.BackgroundTransparency=1; val.Text=valTxt; val.TextColor3=C_ACCENT2
        val.Font=Enum.Font.GothamBlack; val.TextSize=12; val.TextXAlignment=Enum.TextXAlignment.Right
        return val
end

local function makeActionBtn(label, onClick)
        local btn=Instance.new("TextButton",scroll)
        btn.Size=UDim2.new(1,0,0,30); btn.BackgroundColor3=C_PANEL; btn.BorderSizePixel=0
        btn.LayoutOrder=LO(); btn.Text=label; btn.TextColor3=C_WHITE; btn.Font=Enum.Font.GothamBold; btn.TextSize=12
        Instance.new("UICorner",btn).CornerRadius=UDim.new(0,6); Instance.new("UIStroke",btn).Color=C_BORDER2
        btn.MouseButton1Click:Connect(function()
                TweenService:Create(btn,TweenInfo.new(0.08),{BackgroundColor3=C_BORDER2}):Play()
                task.delay(0.16,function() TweenService:Create(btn,TweenInfo.new(0.12),{BackgroundColor3=C_PANEL}):Play() end)
                if onClick then pcall(onClick) end
        end)
        btn.MouseEnter:Connect(function() TweenService:Create(btn,TweenInfo.new(0.1),{BackgroundColor3=C_ROW_HOV}):Play() end)
        btn.MouseLeave:Connect(function() TweenService:Create(btn,TweenInfo.new(0.1),{BackgroundColor3=C_PANEL}):Play() end)
        return btn
end

-- =========================================================
-- makeToggleRow WITH optional keybind button
-- keybindStateKey: key in State table (e.g. "keyAutoLeft")
-- =========================================================
local function makeToggleRow(label, defaultKey, defaultOn, onToggle, onKeyChanged, keybindStateKey)
        local row=Instance.new("Frame",scroll); row.Size=UDim2.new(1,0,0,30); row.BackgroundColor3=C_ROW; row.LayoutOrder=LO()
        Instance.new("UICorner",row).CornerRadius=UDim.new(0,8)

        -- Label: width depends on whether we have both keybind + toggle
        local lblWidth = keybindStateKey and 0.38 or (defaultKey and 0.5 or 0.65)
        local lbl=Instance.new("TextLabel",row)
        lbl.Size=UDim2.new(lblWidth, -4, 1, 0)
        lbl.Position=UDim2.new(0,10,0,0)
        lbl.BackgroundTransparency=1; lbl.Text=label; lbl.TextColor3=C_ACCENT
        lbl.Font=Enum.Font.GothamBold; lbl.TextSize=11; lbl.TextXAlignment=Enum.TextXAlignment.Left

        -- Old keybind (original system, kept for non-keybind rows)
        local keyBtn=nil
        if defaultKey and not keybindStateKey then
                keyBtn=Instance.new("TextButton",row); keyBtn.Size=UDim2.new(0,64,0,20); keyBtn.Position=UDim2.new(1,-120,0.5,-10)
                keyBtn.BackgroundColor3=C_KEY_BG; keyBtn.BorderSizePixel=0; keyBtn.Text=defaultKey.Name
                keyBtn.TextColor3=C_ACCENT2; keyBtn.Font=Enum.Font.GothamBold; keyBtn.TextSize=10; keyBtn.ZIndex=5
                Instance.new("UICorner",keyBtn).CornerRadius=UDim.new(0,4)
                local ks=Instance.new("UIStroke",keyBtn); ks.Color=C_BORDER2; ks.Thickness=1
                local kListening=false; local kConn
                local function kStop(key)
                        kListening=false; if kConn then kConn:Disconnect(); kConn=nil end
                        TweenService:Create(ks,TweenInfo.new(0.12),{Color=C_BORDER2}):Play(); keyBtn.TextColor3=C_ACCENT2
                        if key then keyBtn.Text=key.Name; if onKeyChanged then onKeyChanged(key) end; autoSaveConfig() end
                end
                keyBtn.MouseButton1Click:Connect(function()
                        if kListening then kStop(nil); return end
                        kListening=true; keyBtn.Text="..."; keyBtn.TextColor3=C_WHITE
                        TweenService:Create(ks,TweenInfo.new(0.12),{Color=C_ACCENT}):Play()
                        kConn=UIS.InputBegan:Connect(function(inp, gp)
                                if not kListening then return end
                                if inp.UserInputType == Enum.UserInputType.Keyboard then
                                        if inp.KeyCode==Enum.KeyCode.Escape then kStop(nil); return end
                                        kStop(inp.KeyCode)
                                elseif inp.UserInputType == Enum.UserInputType.Gamepad1 then
                                        if inp.KeyCode==Enum.KeyCode.Escape then kStop(nil); return end
                                        kStop(inp.KeyCode)
                                end
                        end)
                end)
        end

        -- NEW: Keybind button (black bg, no emoji, keyboard+controller)
        local newKeyBtn = nil
        if keybindStateKey then
                local currentKey = State[keybindStateKey]
                local keyDisplayName = (currentKey and currentKey ~= Enum.KeyCode.Unknown) and currentKey.Name or "-"
                
                newKeyBtn = Instance.new("TextButton", row)
                newKeyBtn.Size = UDim2.new(0, 56, 0, 20)
                newKeyBtn.Position = UDim2.new(1, -106, 0.5, -10)
                newKeyBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
                newKeyBtn.BorderSizePixel = 0
                newKeyBtn.Text = keyDisplayName
                newKeyBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
                newKeyBtn.Font = Enum.Font.GothamBold
                newKeyBtn.TextSize = 10
                newKeyBtn.AutoButtonColor = false
                newKeyBtn.ZIndex = 6
                Instance.new("UICorner", newKeyBtn).CornerRadius = UDim.new(0, 4)
                local nks = Instance.new("UIStroke", newKeyBtn)
                nks.Color = Color3.fromRGB(50, 50, 50)
                nks.Thickness = 1

                local nkListening = false
                local nkConn = nil
                local function nkStop(key)
                        nkListening = false
                        if nkConn then nkConn:Disconnect(); nkConn = nil end
                        TweenService:Create(nks, TweenInfo.new(0.12), {Color=Color3.fromRGB(50,50,50)}):Play()
                        newKeyBtn.TextColor3 = Color3.fromRGB(200,200,200)
                        if key then
                                State[keybindStateKey] = key
                                newKeyBtn.Text = key.Name
                                autoSaveConfig()
                        end
                end
                newKeyBtn.MouseButton1Click:Connect(function()
                        if nkListening then nkStop(nil); return end
                        nkListening = true
                        newKeyBtn.Text = "..."
                        newKeyBtn.TextColor3 = C_WHITE
                        TweenService:Create(nks, TweenInfo.new(0.12), {Color=C_ACCENT}):Play()
                        nkConn = UIS.InputBegan:Connect(function(inp, gp)
                                if not nkListening then return end
                                if inp.UserInputType == Enum.UserInputType.Keyboard then
                                        if inp.KeyCode == Enum.KeyCode.Escape then nkStop(nil); return end
                                        nkStop(inp.KeyCode)
                                elseif inp.UserInputType == Enum.UserInputType.Gamepad1 then
                                        nkStop(inp.KeyCode)
                                end
                        end)
                end)
        end

        -- Toggle pill
        local pillBg=Instance.new("Frame",row); pillBg.Size=UDim2.new(0,40,0,20); pillBg.Position=UDim2.new(1,-46,0.5,-10)
        pillBg.BackgroundColor3=defaultOn and C_ON_BG or C_OFF_BG; pillBg.BorderSizePixel=0; pillBg.ZIndex=5
        Instance.new("UICorner",pillBg).CornerRadius=UDim.new(1,0)
        local pStroke=Instance.new("UIStroke",pillBg); pStroke.Color=defaultOn and C_ACCENT2 or C_BORDER; pStroke.Thickness=1
        local dot=Instance.new("Frame",pillBg); dot.Size=UDim2.new(0,14,0,14); dot.Position=defaultOn and UDim2.new(1,-17,0.5,-7) or UDim2.new(0,3,0.5,-7)
        dot.BackgroundColor3=defaultOn and C_WHITE or C_DIM; dot.BorderSizePixel=0; dot.ZIndex=6
        Instance.new("UICorner",dot).CornerRadius=UDim.new(1,0)
        local isOn=defaultOn or false
        local function setV(on)
                isOn=on
                TweenService:Create(pillBg,TweenInfo.new(0.2,Enum.EasingStyle.Quad),{BackgroundColor3=on and C_ON_BG or C_OFF_BG}):Play()
                TweenService:Create(pStroke,TweenInfo.new(0.2),{Color=on and C_ACCENT2 or C_BORDER}):Play()
                TweenService:Create(dot,TweenInfo.new(0.2,Enum.EasingStyle.Back),{
                        Position=on and UDim2.new(1,-17,0.5,-7) or UDim2.new(0,3,0.5,-7),
                        BackgroundColor3=on and C_WHITE or C_DIM
                }):Play()
        end
        local clk=Instance.new("TextButton",row); clk.Size=UDim2.new(1,0,1,0); clk.BackgroundTransparency=1; clk.Text=""; clk.ZIndex=3
        clk.MouseButton1Click:Connect(function()
                isOn=not isOn; setV(isOn); if onToggle then pcall(onToggle,isOn) end
                autoSaveConfig()
        end)
        if keyBtn then keyBtn.ZIndex=6 end
        if newKeyBtn then newKeyBtn.ZIndex=7; clk.ZIndex=3 end
        pillBg.ZIndex=5; dot.ZIndex=6
        clk.MouseEnter:Connect(function() TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=C_ROW_HOV}):Play() end)
        clk.MouseLeave:Connect(function() TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=C_ROW}):Play() end)
        return setV, keyBtn or newKeyBtn
end

-- ================= CONSTRUCTION DE L'INTERFACE =================
makeSectionLabel("Speed")
normalBox = makeInputRow("Normal Speed", State.normalSpeed, function(v) local n=tonumber(v); if n and n>0 and n<=500 then State.normalSpeed=n end end)
carryBox  = makeInputRow("Carry Speed",  State.carrySpeed,  function(v) local n=tonumber(v); if n and n>0 and n<=500 then State.carrySpeed=n  end end)
laggerBox = makeInputRow("Lagger Normal", State.laggerSpeed, function(v) local n=tonumber(v); if n and n>0 and n<=500 then State.laggerSpeed=n end end)
carryLaggerBox = makeInputRow("Lagger Steal", State.laggerCarrySpeed, function(v) local n=tonumber(v); if n and n>0 and n<=500 then State.laggerCarrySpeed=n end end)
modeValLbl = makeStatusRow("Mode","Normal")
makeGap(10)

makeSectionLabel("Combat")
-- BAT AIMBOT with keybind
setAutoBat = makeToggleRow("Auto Bat", nil, false,
        function(on) State.autoBatToggled=on; if MobileButtons.Buttons.autoBat then MobileButtons.Buttons.autoBat(on) end end,
        nil, "keyAutoBat")
makeGap(10)

makeSectionLabel("Mechanics")
local radiusBox = makeInputRow("Grab Radius", AutoSteal.Radius, function(v)
        local n = tonumber(v)
        if n and n >= 5 and n <= 300 then
                AutoSteal.Radius = math.floor(n)
                if progressRadLbl then progressRadLbl.Text = "Steal Radius: "..AutoSteal.Radius end
        end
end)
setInstaGrab = makeToggleRow("Auto Grab (Progression)", nil, false, function(on)
        AutoSteal.Enabled = on
        if on then startAutoSteal() else stopAutoSteal() end
end)
setInfJump = makeToggleRow("Infinite Jump", nil, false, function(on) State.infJumpEnabled=on end)

local jumpModeContainer = Instance.new("Frame", scroll)
jumpModeContainer.Size = UDim2.new(1,0,0,34)
jumpModeContainer.BackgroundTransparency = 1
jumpModeContainer.BorderSizePixel = 0
jumpModeContainer.LayoutOrder = LO()

local manuelBtn = Instance.new("TextButton", jumpModeContainer)
manuelBtn.Size = UDim2.new(0.5,-3,1,0)
manuelBtn.Position = UDim2.new(0,0,0,0)
manuelBtn.BackgroundColor3 = C_ON_BG
manuelBtn.BorderSizePixel = 0
manuelBtn.Text = "Manual"
manuelBtn.TextColor3 = C_WHITE
manuelBtn.Font = Enum.Font.GothamBold
manuelBtn.TextSize = 12
manuelBtn.AutoButtonColor = false
manuelBtn.ZIndex = 5
Instance.new("UICorner", manuelBtn).CornerRadius = UDim.new(0,8)
Instance.new("UIStroke", manuelBtn).Color = C_BORDER2

local holdBtn = Instance.new("TextButton", jumpModeContainer)
holdBtn.Size = UDim2.new(0.5,-3,1,0)
holdBtn.Position = UDim2.new(0.5,3,0,0)
holdBtn.BackgroundColor3 = C_OFF_BG
holdBtn.BorderSizePixel = 0
holdBtn.Text = "Hold"
holdBtn.TextColor3 = C_DIM
holdBtn.Font = Enum.Font.GothamBold
holdBtn.TextSize = 12
holdBtn.AutoButtonColor = false
holdBtn.ZIndex = 5
Instance.new("UICorner", holdBtn).CornerRadius = UDim.new(0,8)
Instance.new("UIStroke", holdBtn).Color = C_BORDER2

local function updateInfJumpModeUI()
        if State.infJumpMode == "manual" then
                manuelBtn.BackgroundColor3 = C_ON_BG; manuelBtn.TextColor3 = C_WHITE
                holdBtn.BackgroundColor3 = C_OFF_BG; holdBtn.TextColor3 = C_DIM
        else
                manuelBtn.BackgroundColor3 = C_OFF_BG; manuelBtn.TextColor3 = C_DIM
                holdBtn.BackgroundColor3 = C_ON_BG; holdBtn.TextColor3 = C_WHITE
        end
end

manuelBtn.MouseButton1Click:Connect(function()
        State.infJumpMode = "manual"
        updateInfJumpModeUI()
        autoSaveConfig()
end)

holdBtn.MouseButton1Click:Connect(function()
        State.infJumpMode = "hold"
        updateInfJumpModeUI()
        autoSaveConfig()
end)

setAutoTpDown = makeToggleRow("Auto Tp Down", nil, false, function(on)
        State.autoTpDownEnabled = on
        autoSaveConfig()
end)
makeInputRow("Y Trigger", State.autoTpDownY, function(v)
        local n = tonumber(v)
        if n then State.autoTpDownY = n; autoSaveConfig() end
end)
setAntiRag = makeToggleRow("Anti Ragdoll", nil, false, function(on)
        State.antiRagdollEnabled=on; if on then startAntiRagdoll() else stopAntiRagdoll() end
end)
setMedusaCounter = makeToggleRow("Medusa Counter", nil, false, function(on)
        State.medusaCounterEnabled=on
        if on then setupMedusaCounter(LP.Character) else stopMedusaCounter() end
end)
setUnwalkToggle = makeToggleRow("Unwalk", nil, false, function(on)
        if on then startUnwalk() else stopUnwalk() end
end)

makeGap(10)

makeSectionLabel("Visuals")
setFps = makeToggleRow("Anti Lag", nil, false, function(on)
        if on then pcall(enableAntiLag) else pcall(disableAntiLag) end
end)
makeToggleRow("Stretch Rez", nil, false, function(on)
        if on then enableStretchRez() else disableStretchRez() end
end)
makeToggleRow("Vibrance", nil, false, function(on)
        if on then enableVibrance() else disableVibrance() end
end)
makeToggleRow("Dark Mode", nil, false, function(on)
        nightModeEnabled = on
        defLightBrightness = defLightBrightness or Lighting.Brightness
        defLightClock      = defLightClock      or Lighting.ClockTime
        defLightAmbient    = defLightAmbient    or Lighting.OutdoorAmbient
        defLightExposure   = defLightExposure   or Lighting.ExposureCompensation
        if on then
                local sky = Lighting:FindFirstChild("hanamiDarkSky") or Instance.new("Sky")
                sky.Name = "hanamiDarkSky"
                sky.SkyboxBk = "rbxassetid://159454299"; sky.SkyboxDn = "rbxassetid://159454296"
                sky.SkyboxFt = "rbxassetid://159454293"; sky.SkyboxLf = "rbxassetid://159454286"
                sky.SkyboxRt = "rbxassetid://159454289"; sky.SkyboxUp = "rbxassetid://159454291"
                sky.Parent = Lighting
                Lighting.Brightness = 0; Lighting.ClockTime = 0
                Lighting.ExposureCompensation = -2
                Lighting.OutdoorAmbient = Color3.fromRGB(0, 0, 0)
        else
                local s = Lighting:FindFirstChild("hanamiDarkSky"); if s then s:Destroy() end
                if defLightBrightness then Lighting.Brightness          = defLightBrightness end
                if defLightClock      then Lighting.ClockTime           = defLightClock      end
                if defLightAmbient    then Lighting.OutdoorAmbient      = defLightAmbient    end
                if defLightExposure   then Lighting.ExposureCompensation = defLightExposure  end
        end
end)

makeGap(10)

makeSectionLabel("Teleport / Movement")

-- DROP BR with keybind
setMenuDropBR = makeToggleRow("Drop Brainrot", nil, false, function(on)
        if on then
                runDropBrainrot()
                task.delay(0.5, function()
                        if setMenuDropBR then setMenuDropBR(false) end
                        if MobileButtons.Buttons and MobileButtons.Buttons.dropBR then
                                MobileButtons.Buttons.dropBR(false)
                        end
                end)
        end
end, nil, "keyDropBR")

-- TP DOWN (changed from "Tp To Ground")
setMenuTpDown = makeToggleRow("Tp Down", nil, false, function(on)
        if on then
                tpToGround()
                task.delay(0.5, function()
                        if setMenuTpDown then setMenuTpDown(false) end
                        if MobileButtons.Buttons and MobileButtons.Buttons.tpDown then
                                MobileButtons.Buttons.tpDown(false)
                        end
                end)
        end
end, nil, "keyTpDown")

-- AUTO LEFT with keybind
setAutoLeft = makeToggleRow("Auto Left", nil, false,
        function(on) 
                State.autoLeftEnabled=on
                if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(on) end
                if on then startAutoLeft() else stopAutoLeft() end
        end, nil, "keyAutoLeft")

-- AUTO RIGHT with keybind
setAutoRight = makeToggleRow("Auto Right", nil, false,
        function(on) 
                State.autoRightEnabled=on
                if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(on) end
                if on then startAutoRight() else stopAutoRight() end
        end, nil, "keyAutoRight")

makeGap(10)

-- ========== MOBILE BUTTONS SECTION IN MENU ==========
makeSectionLabel("Mobile Buttons")

local updateLockBtnUI = nil

local setLockToggle = makeToggleRow("Lock Cubes", nil, true, function(on)
        MobileButtons.Locked = on
        autoSaveConfig()
end)

updateLockBtnUI = function(locked)
        if setLockToggle then setLockToggle(locked) end
end

makeGap(6)

local autoSaveLbl = Instance.new("TextLabel", scroll)
autoSaveLbl.Size = UDim2.new(1,0,0,18)
autoSaveLbl.BackgroundTransparency = 1
autoSaveLbl.Text = "â— Config Auto Save: ON"
autoSaveLbl.TextColor3 = Color3.fromRGB(100,200,100)
autoSaveLbl.Font = Enum.Font.GothamBold
autoSaveLbl.TextSize = 10
autoSaveLbl.TextXAlignment = Enum.TextXAlignment.Center
autoSaveLbl.LayoutOrder = LO()

makeGap(2)

local footerLbl = Instance.new("TextLabel",scroll)
footerLbl.Size=UDim2.new(1,0,0,18); footerLbl.BackgroundTransparency=1; footerLbl.LayoutOrder=LO()
footerLbl.Text="hanami hub  Â·  v1.0"; footerLbl.TextColor3=Color3.fromRGB(180,80,130)
footerLbl.Font=Enum.Font.Gotham; footerLbl.TextSize=10; footerLbl.TextXAlignment=Enum.TextXAlignment.Center

local function resetProgressBar() end
local function toggleGuiVis()
        State.guiVisible=not State.guiVisible
        main.Visible=State.guiVisible
        closeBtn.Visible=State.guiVisible
        if not State.guiVisible then
                miniBtn.Visible = true
        else
                miniBtn.Visible = false
        end
end

-- ========== KEYBIND LISTENER (global) ==========
UIS.InputBegan:Connect(function(inp, gp)
        if gp then return end
        local kc = inp.KeyCode
        -- Auto Left
        if State.keyAutoLeft ~= Enum.KeyCode.Unknown and kc == State.keyAutoLeft then
                State.autoLeftEnabled = not State.autoLeftEnabled
                if setAutoLeft then setAutoLeft(State.autoLeftEnabled) end
                if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(State.autoLeftEnabled) end
                if State.autoLeftEnabled then startAutoLeft() else stopAutoLeft() end
                autoSaveConfig()
        end
        -- Auto Right
        if State.keyAutoRight ~= Enum.KeyCode.Unknown and kc == State.keyAutoRight then
                State.autoRightEnabled = not State.autoRightEnabled
                if setAutoRight then setAutoRight(State.autoRightEnabled) end
                if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(State.autoRightEnabled) end
                if State.autoRightEnabled then startAutoRight() else stopAutoRight() end
                autoSaveConfig()
        end
        -- Drop BR
        if State.keyDropBR ~= Enum.KeyCode.Unknown and kc == State.keyDropBR then
                runDropBrainrot()
                if setMenuDropBR then setMenuDropBR(true) end
                if MobileButtons.Buttons.dropBR then MobileButtons.Buttons.dropBR(true) end
                task.delay(0.5, function()
                        if setMenuDropBR then setMenuDropBR(false) end
                        if MobileButtons.Buttons.dropBR then MobileButtons.Buttons.dropBR(false) end
                end)
        end
        -- Tp Down
        if State.keyTpDown ~= Enum.KeyCode.Unknown and kc == State.keyTpDown then
                tpToGround()
                if setMenuTpDown then setMenuTpDown(true) end
                if MobileButtons.Buttons.tpDown then MobileButtons.Buttons.tpDown(true) end
                task.delay(0.5, function()
                        if setMenuTpDown then setMenuTpDown(false) end
                        if MobileButtons.Buttons.tpDown then MobileButtons.Buttons.tpDown(false) end
                end)
        end
        -- Auto Bat
        if State.keyAutoBat ~= Enum.KeyCode.Unknown and kc == State.keyAutoBat then
                State.autoBatToggled = not State.autoBatToggled
                if setAutoBat then setAutoBat(State.autoBatToggled) end
                if MobileButtons.Buttons.autoBat then MobileButtons.Buttons.autoBat(State.autoBatToggled) end
                autoSaveConfig()
        end
end)

-- Controller (Gamepad) keybind listener
UIS.InputBegan:Connect(function(inp, gp)
        if inp.UserInputType ~= Enum.UserInputType.Gamepad1 then return end
        local kc = inp.KeyCode
        if State.keyAutoLeft ~= Enum.KeyCode.Unknown and kc == State.keyAutoLeft then
                State.autoLeftEnabled = not State.autoLeftEnabled
                if setAutoLeft then setAutoLeft(State.autoLeftEnabled) end
                if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(State.autoLeftEnabled) end
                if State.autoLeftEnabled then startAutoLeft() else stopAutoLeft() end
        end
        if State.keyAutoRight ~= Enum.KeyCode.Unknown and kc == State.keyAutoRight then
                State.autoRightEnabled = not State.autoRightEnabled
                if setAutoRight then setAutoRight(State.autoRightEnabled) end
                if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(State.autoRightEnabled) end
                if State.autoRightEnabled then startAutoRight() else stopAutoRight() end
        end
        if State.keyDropBR ~= Enum.KeyCode.Unknown and kc == State.keyDropBR then
                runDropBrainrot()
        end
        if State.keyTpDown ~= Enum.KeyCode.Unknown and kc == State.keyTpDown then
                tpToGround()
        end
        if State.keyAutoBat ~= Enum.KeyCode.Unknown and kc == State.keyAutoBat then
                State.autoBatToggled = not State.autoBatToggled
                if setAutoBat then setAutoBat(State.autoBatToggled) end
                if MobileButtons.Buttons.autoBat then MobileButtons.Buttons.autoBat(State.autoBatToggled) end
        end
end)

-- ========== MÃ‰DUSA, ETC. ==========
local function findMedusa()
        local char=LP.Character; if not char then return nil end
        for _,tool in ipairs(char:GetChildren()) do
                if tool:IsA("Tool") then local tn=tool.Name:lower()
                        if tn:find("medusa") or tn:find("head") or tn:find("stone") then return tool end end
        end
        local bp2=LP:FindFirstChild("Backpack")
        if bp2 then for _,tool in ipairs(bp2:GetChildren()) do
                if tool:IsA("Tool") then local tn=tool.Name:lower()
                        if tn:find("medusa") or tn:find("head") or tn:find("stone") then return tool end end
        end end
        return nil
end

local function useMedusaCounter()
        if State.medusaDebounce then return end
        if tick()-State.medusaLastUsed<25 then return end
        local char=LP.Character; if not char then return end
        State.medusaDebounce=true
        local med=findMedusa(); if not med then State.medusaDebounce=false; return end
        if med.Parent~=char then local hum2=char:FindFirstChildOfClass("Humanoid"); if hum2 then hum2:EquipTool(med) end end
        pcall(function() med:Activate() end)
        State.medusaLastUsed=tick(); State.medusaDebounce=false
end

local function onAnchorChanged(part)
        return part:GetPropertyChangedSignal("Anchored"):Connect(function()
                if part.Anchored and part.Transparency==1 then useMedusaCounter() end
        end)
end
setupMedusaCounter = function(char)
        stopMedusaCounter(); if not char then return end
        for _,part in ipairs(char:GetDescendants()) do
                if part:IsA("BasePart") then table.insert(Conns.anchor,onAnchorChanged(part)) end
        end
        table.insert(Conns.anchor, char.DescendantAdded:Connect(function(part)
                if part:IsA("BasePart") then table.insert(Conns.anchor,onAnchorChanged(part)) end
        end))
end
stopMedusaCounter = function()
        for _,c in pairs(Conns.anchor) do pcall(function() c:Disconnect() end) end; Conns.anchor={}
end

startAutoLeft = function()
        if Conns.autoLeft then Conns.autoLeft:Disconnect() end
        State.autoLeftPhase = 1
        Conns.autoLeft = RunService.Heartbeat:Connect(function()
                if not State.autoLeftEnabled then return end
                if State.countdownActive then return end  -- ANTI-DETECT: Stop during countdown
                local char = LP.Character
                if not char then return end
                local root = char:FindFirstChild("HumanoidRootPart")
                local hum2 = char:FindFirstChildOfClass("Humanoid")
                if not root or not hum2 then return end
                local spd = getAutoMoveSpeed()
                if State.autoLeftPhase == 1 then
                        local tgt = Vector3.new(POS.L1.X, root.Position.Y, POS.L1.Z)
                        if (tgt - root.Position).Magnitude < 1 then
                                State.autoLeftPhase = 2
                                return
                        end
                        local d = (POS.L1 - root.Position)
                        local mv = Vector3.new(d.X, 0, d.Z).Unit
                        hum2:Move(mv, false)
                        root.AssemblyLinearVelocity = Vector3.new(mv.X * spd, root.AssemblyLinearVelocity.Y, mv.Z * spd)
                elseif State.autoLeftPhase == 2 then
                        local tgt = Vector3.new(POS.L2.X, root.Position.Y, POS.L2.Z)
                        if (tgt - root.Position).Magnitude < 1 then
                                hum2:Move(Vector3.zero, false)
                                root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y, 0)
                                State.autoLeftEnabled = false
                                if Conns.autoLeft then Conns.autoLeft:Disconnect(); Conns.autoLeft = nil end
                                State.autoLeftPhase = 1
                                if setAutoLeft then setAutoLeft(false) end
                                if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(false) end
                                return
                        end
                        local d = (POS.L2 - root.Position)
                        local mv = Vector3.new(d.X, 0, d.Z).Unit
                        hum2:Move(mv, false)
                        root.AssemblyLinearVelocity = Vector3.new(mv.X * spd, root.AssemblyLinearVelocity.Y, mv.Z * spd)
                end
        end)
end

stopAutoLeft = function()
        if Conns.autoLeft then Conns.autoLeft:Disconnect(); Conns.autoLeft = nil end
        State.autoLeftPhase = 1
        local char = LP.Character
        if char then
                local hum2 = char:FindFirstChildOfClass("Humanoid")
                if hum2 then hum2:Move(Vector3.zero, false) end
                local root = char:FindFirstChild("HumanoidRootPart")
                if root then root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y, 0) end
        end
        if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(false) end
end

startAutoRight = function()
        if Conns.autoRight then Conns.autoRight:Disconnect() end
        State.autoRightPhase = 1
        Conns.autoRight = RunService.Heartbeat:Connect(function()
                if not State.autoRightEnabled then return end
                if State.countdownActive then return end  -- ANTI-DETECT: Stop during countdown
                local char = LP.Character
                if not char then return end
                local root = char:FindFirstChild("HumanoidRootPart")
                local hum2 = char:FindFirstChildOfClass("Humanoid")
                if not root or not hum2 then return end
                local spd = getAutoMoveSpeed()
                if State.autoRightPhase == 1 then
                        local tgt = Vector3.new(POS.R1.X, root.Position.Y, POS.R1.Z)
                        if (tgt - root.Position).Magnitude < 1 then
                                State.autoRightPhase = 2
                                return
                        end
                        local d = (POS.R1 - root.Position)
                        local mv = Vector3.new(d.X, 0, d.Z).Unit
                        hum2:Move(mv, false)
                        root.AssemblyLinearVelocity = Vector3.new(mv.X * spd, root.AssemblyLinearVelocity.Y, mv.Z * spd)
                elseif State.autoRightPhase == 2 then
                        local tgt = Vector3.new(POS.R2.X, root.Position.Y, POS.R2.Z)
                        if (tgt - root.Position).Magnitude < 1 then
                                hum2:Move(Vector3.zero, false)
                                root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y, 0)
                                State.autoRightEnabled = false
                                if Conns.autoRight then Conns.autoRight:Disconnect(); Conns.autoRight = nil end
                                State.autoRightPhase = 1
                                if setAutoRight then setAutoRight(false) end
                                if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(false) end
                                return
                        end
                        local d = (POS.R2 - root.Position)
                        local mv = Vector3.new(d.X, 0, d.Z).Unit
                        hum2:Move(mv, false)
                        root.AssemblyLinearVelocity = Vector3.new(mv.X * spd, root.AssemblyLinearVelocity.Y, mv.Z * spd)
                end
        end)
end

stopAutoRight = function()
        if Conns.autoRight then Conns.autoRight:Disconnect(); Conns.autoRight = nil end
        State.autoRightPhase = 1
        local char = LP.Character
        if char then
                local hum2 = char:FindFirstChildOfClass("Humanoid")
                if hum2 then hum2:Move(Vector3.zero, false) end
                local root = char:FindFirstChild("HumanoidRootPart")
                if root then root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y, 0) end
        end
        if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(false) end
end

startAntiRagdoll = function()
        if Conns.antiRag then return end
        Conns.antiRag=RunService.Heartbeat:Connect(function()
                local char=LP.Character; if not char then return end
                local hum2=char:FindFirstChildOfClass("Humanoid"); local root=char:FindFirstChild("HumanoidRootPart")
                if hum2 then
                        local st=hum2:GetState()
                        if st==Enum.HumanoidStateType.Physics or st==Enum.HumanoidStateType.Ragdoll or st==Enum.HumanoidStateType.FallingDown then
                                hum2:ChangeState(Enum.HumanoidStateType.Running); workspace.CurrentCamera.CameraSubject=hum2
                                pcall(function() local pm=LP.PlayerScripts:FindFirstChild("PlayerModule"); if pm then require(pm:FindFirstChild("ControlModule")):Enable() end end)
                                if root then root.Velocity=Vector3.new(0,0,0); root.RotVelocity=Vector3.new(0,0,0) end
                        end
                end
                for _,obj in ipairs(char:GetDescendants()) do if obj:IsA("Motor6D") and not obj.Enabled then obj.Enabled=true end end
        end)
end
stopAntiRagdoll = function()
        if Conns.antiRag then Conns.antiRag:Disconnect(); Conns.antiRag=nil end
end

local function applyAntiLagDerender(obj)
        pcall(function()
                if obj:IsA("Accessory") or obj:IsA("Hat") then obj:Destroy()
                elseif obj:IsA("BasePart") then obj.Material=Enum.Material.Plastic; obj.Reflectance=0; obj.CastShadow=false
                elseif obj:IsA("Decal") or obj:IsA("Texture") then obj.Transparency=1
                elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Beam") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then obj.Enabled=false
                elseif obj:IsA("AnimationController") or obj:IsA("Animator") then
                        for _,t in ipairs(obj:GetPlayingAnimationTracks()) do pcall(function() t:Stop(0) end) end
                end
        end)
end
enableAntiLag = function()
        removeAccessoriesEnabled = true
        antiLagEnabled = true
        State.fpsBoostEnabled = true
        local lighting = game:GetService("Lighting")
        defLightBrightness = defLightBrightness or lighting.Brightness
        defLightClock      = defLightClock      or lighting.ClockTime
        defLightAmbient    = defLightAmbient    or lighting.OutdoorAmbient
        lighting.GlobalShadows = false; lighting.FogEnd = 1e10; lighting.Brightness = 1
        lighting.EnvironmentDiffuseScale = 0; lighting.EnvironmentSpecularScale = 0
        for _, e in pairs(lighting:GetChildren()) do
                pcall(function()
                        if e:IsA("BlurEffect") or e:IsA("SunRaysEffect") or e:IsA("ColorCorrectionEffect") or e:IsA("BloomEffect") or e:IsA("DepthOfFieldEffect") then e.Enabled = false end
                end)
        end
        for _, obj in ipairs(workspace:GetDescendants()) do applyAntiLagDerender(obj) end
        if antiLagDescConn then antiLagDescConn:Disconnect() end
        antiLagDescConn = workspace.DescendantAdded:Connect(function(obj)
                if removeAccessoriesEnabled then applyAntiLagDerender(obj) end
        end)
end
disableAntiLag = function()
        removeAccessoriesEnabled = false
        antiLagEnabled = false
        State.fpsBoostEnabled = false
        if antiLagDescConn then antiLagDescConn:Disconnect(); antiLagDescConn = nil end
        pcall(function()
                local lighting = game:GetService("Lighting")
                if defLightBrightness then lighting.Brightness = defLightBrightness end
                if defLightClock      then lighting.ClockTime  = defLightClock      end
                if defLightAmbient    then lighting.OutdoorAmbient = defLightAmbient end
                lighting.ExposureCompensation = 0
        end)
end

enableStretchRez = function()
        State.stretchRezEnabled = true
        workspace.CurrentCamera.FieldOfView = 107
        if stretchRezConn then stretchRezConn:Disconnect() end
        stretchRezConn = RunService.RenderStepped:Connect(function()
                if not State.stretchRezEnabled then stretchRezConn:Disconnect(); stretchRezConn = nil; return end
                workspace.CurrentCamera.FieldOfView = 107
        end)
end

disableStretchRez = function()
        State.stretchRezEnabled = false
        if stretchRezConn then stretchRezConn:Disconnect(); stretchRezConn = nil end
        workspace.CurrentCamera.FieldOfView = 70
end

local function getBat()
        local char=LP.Character; if not char then return nil end
        local tool=char:FindFirstChild("Bat"); if tool then return tool end
        local bp2=LP:FindFirstChild("Backpack")
        if bp2 then tool=bp2:FindFirstChild("Bat"); if tool then tool.Parent=char; return tool end end
        return nil
end
local function tryHitBat()
        if State.hittingCooldown then return end; State.hittingCooldown=true
        pcall(function()
                local bat=getBat(); if bat then
                        bat:Activate(); local ev=bat:FindFirstChildWhichIsA("RemoteEvent")
                        if ev then ev:FireServer() end
                end
        end)
        task.delay(0.08, function() State.hittingCooldown=false end)
end

local function loadConfig()
        local hasFile=false; pcall(function() hasFile=isfile("HanamiHubConfig.json") end)
        if not hasFile then return end
        local ok,cfg=pcall(function() return HttpService:JSONDecode(readfile("HanamiHubConfig.json")) end)
        if not ok or not cfg then return end
        if cfg.normalSpeed and type(cfg.normalSpeed)=="number" then State.normalSpeed=cfg.normalSpeed; normalBox.Text=tostring(cfg.normalSpeed) end
        if cfg.carrySpeed  and type(cfg.carrySpeed)=="number"  then State.carrySpeed=cfg.carrySpeed;   carryBox.Text=tostring(cfg.carrySpeed)   end
        if cfg.laggerSpeed and type(cfg.laggerSpeed)=="number" then State.laggerSpeed=cfg.laggerSpeed; laggerBox.Text=tostring(cfg.laggerSpeed) end
        if cfg.laggerCarrySpeed and type(cfg.laggerCarrySpeed)=="number" then State.laggerCarrySpeed=cfg.laggerCarrySpeed; carryLaggerBox.Text=tostring(cfg.laggerCarrySpeed) end
        if cfg.speedType == "normal" or cfg.speedType == "carry" then State.speedType = cfg.speedType end
        if type(cfg.laggerActive) == "boolean" then State.laggerActive = cfg.laggerActive end
        if type(cfg.laggerCarryActive) == "boolean" then State.laggerCarryActive = cfg.laggerCarryActive end
        if cfg.grabRadius and type(cfg.grabRadius)=="number" then
                AutoSteal.Radius=cfg.grabRadius; radiusBox.Text=tostring(cfg.grabRadius); if progressRadLbl then progressRadLbl.Text="Steal Radius: "..cfg.grabRadius end end
        if cfg.autoStealEnabled then AutoSteal.Enabled=true; setInstaGrab(true); pcall(startAutoSteal) end
        if cfg.infJump     then State.infJumpEnabled=true;       setInfJump(true)  end
        if cfg.infJumpMode == "manual" or cfg.infJumpMode == "hold" then
                State.infJumpMode = cfg.infJumpMode; updateInfJumpModeUI()
        end
        if cfg.autoTpDown and type(cfg.autoTpDown)=="boolean" then State.autoTpDownEnabled=cfg.autoTpDown; if setAutoTpDown then setAutoTpDown(cfg.autoTpDown) end end
        if cfg.autoTpDownY and type(cfg.autoTpDownY)=="number" then State.autoTpDownY=cfg.autoTpDownY end
        if cfg.antiRagdoll then State.antiRagdollEnabled=true;   setAntiRag(true); startAntiRagdoll() end
        if cfg.fpsBoost    then State.fpsBoostEnabled=true;      setFps(true);     pcall(enableAntiLag)    end
        if cfg.medusaCounter then State.medusaCounterEnabled=true; setMedusaCounter(true); setupMedusaCounter(LP.Character) end
        if cfg.unwalkEnabled then
                setUnwalkToggle(true)
                task.spawn(function()
                        task.wait(0.5); State.unwalkEnabled = false; startUnwalk()
                end)
        end
        if type(cfg.autoBatToggled) == "boolean" then State.autoBatToggled = cfg.autoBatToggled end
        if type(cfg.autoLeftEnabled) == "boolean" then State.autoLeftEnabled = cfg.autoLeftEnabled end
        if type(cfg.autoRightEnabled) == "boolean" then State.autoRightEnabled = cfg.autoRightEnabled end
        if cfg.mobileVisible ~= nil then
                MobileButtons.Visible = cfg.mobileVisible
                for _, c in ipairs(MobileButtons.Containers) do c.Visible = MobileButtons.Visible end
        end
        if cfg.mobileLocked ~= nil then
                MobileButtons.Locked = cfg.mobileLocked
                if setLockToggle then setLockToggle(cfg.mobileLocked) end
        end
        -- Load keybinds
        local function loadKey(cfgKey, stateKey)
                if cfg[cfgKey] and type(cfg[cfgKey]) == "string" then
                        local ok2, kc = pcall(function() return Enum.KeyCode[cfg[cfgKey]] end)
                        if ok2 and kc then State[stateKey] = kc end
                end
        end
        loadKey("keyAutoLeft",  "keyAutoLeft")
        loadKey("keyAutoRight", "keyAutoRight")
        loadKey("keyDropBR",    "keyDropBR")
        loadKey("keyTpDown",    "keyTpDown")
        loadKey("keyAutoBat",   "keyAutoBat")
        task.spawn(function()
                task.wait(0.6)
                if MobileButtons.Buttons.autoBat then MobileButtons.Buttons.autoBat(State.autoBatToggled) end
                if MobileButtons.Buttons.carrySpeed then MobileButtons.Buttons.carrySpeed(State.speedType == "carry") end
                if MobileButtons.Buttons.lagger then MobileButtons.Buttons.lagger(State.laggerActive) end
                if MobileButtons.Buttons.laggerCarry then MobileButtons.Buttons.laggerCarry(State.laggerCarryActive) end
                if MobileButtons.Buttons.autoLeft then MobileButtons.Buttons.autoLeft(State.autoLeftEnabled) end
                if MobileButtons.Buttons.autoRight then MobileButtons.Buttons.autoRight(State.autoRightEnabled) end
                if State.laggerActive then startAutoSteal() end
                if State.laggerCarryActive then startAutoSteal() end
                if State.autoLeftEnabled then startAutoLeft() end
                if State.autoRightEnabled then startAutoRight() end
        end)
        refreshUIToggles()
end

task.spawn(function()
        task.wait(0.5)
        createMobilePanel()
end)

local function setupChar(char)
        task.wait(0.1)
        originalAnims = nil
        h=char:WaitForChild("Humanoid",5); hrp=char:WaitForChild("HumanoidRootPart",5)
        if not h or not hrp then return end
        local head=char:FindFirstChild("Head")
        if head then
                local oldBB=head:FindFirstChild("SpeedBillboard"); if oldBB then oldBB:Destroy() end
                local bb=Instance.new("BillboardGui",head)
                bb.Name="SpeedBillboard"; bb.Size=UDim2.new(0,180,0,52); bb.StudsOffset=Vector3.new(0,3,0); bb.AlwaysOnTop=true
                -- Steal countdown label (from opium)
                local ragdollLbl=Instance.new("TextLabel",bb)
                ragdollLbl.Name="RagdollTimerLbl"
                ragdollLbl.Size=UDim2.new(1,0,0,26)
                ragdollLbl.Position=UDim2.new(0,0,0,0)
                ragdollLbl.BackgroundTransparency=1
                ragdollLbl.Text=""
                ragdollLbl.TextColor3=Color3.fromRGB(255,80,80)
                ragdollLbl.Font=Enum.Font.GothamBlack
                ragdollLbl.TextScaled=true
                ragdollLbl.TextStrokeTransparency=0.1
                ragdollLbl.TextStrokeColor3=Color3.new(0,0,0)
                ragdollLbl.Visible=false
                -- Speed label
                speedLbl=Instance.new("TextLabel",bb)
                speedLbl.Size=UDim2.new(1,0,0,26)
                speedLbl.Position=UDim2.new(0,0,0,26)
                speedLbl.BackgroundTransparency=1; speedLbl.TextColor3=C_ACCENT2
                speedLbl.Font=Enum.Font.GothamBold; speedLbl.TextScaled=true; speedLbl.TextStrokeTransparency=0
        end
        if State.antiRagdollEnabled and not Conns.antiRag then task.wait(0.5); startAntiRagdoll() end
        if State.medusaCounterEnabled then setupMedusaCounter(char) end
        if State.animEnabled then task.wait(0.3); saveOriginalAnims(char); applyAnimPack(char) end
        if State.unwalkEnabled then
                State.unwalkEnabled = false; task.wait(0.3); startUnwalk()
        end

        -- â”€â”€ Steal countdown (from opium) â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€
        local ragTimerActive = false
        local function startRagdollTimer()
                if ragTimerActive then return end
                ragTimerActive = true
                State.countdownActive = true
                task.spawn(function()
                        local char2 = LP.Character
                        local head2 = char2 and char2:FindFirstChild("Head")
                        local bb2   = head2 and head2:FindFirstChild("SpeedBillboard")
                        local timerLbl = bb2 and bb2:FindFirstChild("RagdollTimerLbl")
                        if not timerLbl then ragTimerActive = false; State.countdownActive = false; return end
                        local countdown = 3
                        timerLbl.Visible = true
                        timerLbl.TextColor3 = Color3.fromRGB(255, 80, 80)
                        while countdown > 0 do
                                timerLbl.Text = tostring(countdown)
                                timerLbl.TextColor3 = ({
                                        Color3.fromRGB(255, 60, 60),
                                        Color3.fromRGB(255, 140, 40),
                                        Color3.fromRGB(255, 220, 40)
                                })[math.max(1, 4 - countdown)]
                                task.wait(1)
                                countdown = countdown - 1
                        end
                        timerLbl.Text = "READY TO STEAL"
                        timerLbl.TextColor3 = Color3.fromRGB(80, 255, 120)
                        repeat task.wait(0.1) until (function()
                                local c3 = LP.Character
                                local hum3 = c3 and c3:FindFirstChildOfClass("Humanoid")
                                if not hum3 then return true end
                                local st3 = hum3:GetState()
                                return st3 ~= Enum.HumanoidStateType.Physics
                                        and st3 ~= Enum.HumanoidStateType.Ragdoll
                                        and st3 ~= Enum.HumanoidStateType.FallingDown
                        end)()
                        timerLbl.Visible = false
                        timerLbl.Text = ""
                        ragTimerActive = false
                        State.countdownActive = false
                end)
        end

        RunService.Heartbeat:Connect(function()
                local hum2 = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
                if not hum2 then return end
                local st = hum2:GetState()
                if st == Enum.HumanoidStateType.Physics
                        or st == Enum.HumanoidStateType.Ragdoll
                        or st == Enum.HumanoidStateType.FallingDown then
                        startRagdollTimer()
                end
        end)
end

LP.CharacterAdded:Connect(setupChar)
if LP.Character then task.spawn(function() setupChar(LP.Character) end) end

RunService.Stepped:Connect(function()
        for _,p in ipairs(Players:GetPlayers()) do
                if p~=LP and p.Character then
                        for _,part in ipairs(p.Character:GetChildren()) do
                                if part:IsA("BasePart") then part.CanCollide=false end
                        end
                end
        end
end)

UIS.JumpRequest:Connect(function()
        if not State.infJumpEnabled then return end
        if State.infJumpMode ~= "manual" then return end
        local char=LP.Character; if not char then return end
        local root=char:FindFirstChild("HumanoidRootPart")
        if root then root.Velocity=Vector3.new(root.Velocity.X,55,root.Velocity.Z) end
end)
RunService.Heartbeat:Connect(function()
        if not State.infJumpEnabled and not State.autoTpDownEnabled then return end
        local char=LP.Character; if not char then return end
        local root=char:FindFirstChild("HumanoidRootPart")
        if not root then return end
        if State.infJumpEnabled then
                local hum2=char:FindFirstChildOfClass("Humanoid")
                if State.infJumpMode == "hold" then
                        local jumpHeld=UIS:IsKeyDown(Enum.KeyCode.Space) or (hum2 and hum2.Jump==true)
                        if jumpHeld and root.Velocity.Y < 30 then
                                root.Velocity=Vector3.new(root.Velocity.X,55,root.Velocity.Z)
                        end
                end
                if root.Velocity.Y<-120 then root.Velocity=Vector3.new(root.Velocity.X,-120,root.Velocity.Z) end
        end
        if State.autoTpDownEnabled then
                local curY = root.Position.Y
                if curY >= State.autoTpDownY then
                        local rot = root.CFrame.Rotation
                        root.CFrame = CFrame.new(root.Position.X, -8.80, root.Position.Z) * rot
                end
        end
end)

RunService.RenderStepped:Connect(function()
        if not (h and hrp) then return end
        if State._tpInProgress then return end

        if State.autoLeftEnabled or State.autoRightEnabled then
                if speedLbl then
                        local hs=Vector3.new(hrp.Velocity.X,0,hrp.Velocity.Z).Magnitude
                        speedLbl.Text="Speed: "..string.format("%.1f",hs)
                end
                return
        end

        local md = h.MoveDirection
        local spd = getCurrentSpeed()

        if md.Magnitude > 0 then
                State.lastMoveDir = md
                hrp.Velocity = Vector3.new(md.X * spd, hrp.Velocity.Y, md.Z * spd)
        elseif State.antiRagdollEnabled and State.lastMoveDir.Magnitude > 0 then
                local anyHeld = false
                for key in pairs(MOVE_KEYS) do if UIS:IsKeyDown(key) then anyHeld = true; break end end
                if anyHeld then
                        hrp.Velocity = Vector3.new(State.lastMoveDir.X * spd, hrp.Velocity.Y, State.lastMoveDir.Z * spd)
                end
        end

        if speedLbl then
                local hs = Vector3.new(hrp.Velocity.X,0,hrp.Velocity.Z).Magnitude
                speedLbl.Text = "Speed: " .. string.format("%.1f", hs)
        end
end)

-- ======== FEAR HUB BAT AIMBOT ========
local AIMBOT_SPEED = 56.5
local HIT_DISTANCE = 5
-- ======== ADAPT BAT AIMBOT ========
local BAT_SLAP_LIST = { "Bat", "Slap", "Iron Slap", "Gold Slap", "Diamond Slap", "Emerald Slap", "Ruby Slap", "Dark Matter Slap", "Flame Slap", "Nuclear Slap", "Galaxy Slap", "Glitched Slap" }
local ADAPT_HIT_DIST = 8
local ADAPT_SWING_CD = 0.35
local adaptHittingCooldown = false
local adaptAimbotConn = nil
local adaptPrevAutoRotate = nil

local function adaptGetBat()
	local char = LP.Character; if not char then return nil end
	for _, name in ipairs(BAT_SLAP_LIST) do
		local t = char:FindFirstChild(name)
		if t and t:IsA("Tool") then return t end
	end
	local bp = LP:FindFirstChildOfClass("Backpack")
	if bp then
		for _, name in ipairs(BAT_SLAP_LIST) do
			local t = bp:FindFirstChild(name)
			if t and t:IsA("Tool") then
				local hum = char:FindFirstChildOfClass("Humanoid")
				if hum then pcall(function() hum:EquipTool(t) end) end
				return t
			end
		end
	end
	for _, ch in ipairs(char:GetChildren()) do
		if ch:IsA("Tool") and (ch.Name:lower():find("bat") or ch.Name:lower():find("slap")) then return ch end
	end
	if bp then
		for _, ch in ipairs(bp:GetChildren()) do
			if ch:IsA("Tool") and (ch.Name:lower():find("bat") or ch.Name:lower():find("slap")) then
				local hum = char:FindFirstChildOfClass("Humanoid")
				if hum then pcall(function() hum:EquipTool(ch) end) end
				return ch
			end
		end
	end
	return nil
end

local function adaptTrySwing()
	if adaptHittingCooldown then return end
	adaptHittingCooldown = true
	pcall(function()
		local char = LP.Character
		if char then
			local bat = adaptGetBat()
			if bat then
				if bat.Parent ~= char then
					local hum = char:FindFirstChildOfClass("Humanoid")
					if hum then pcall(function() hum:EquipTool(bat) end) end
				end
				pcall(function() bat:Activate() end)
			end
		end
	end)
	task.delay(ADAPT_SWING_CD, function() adaptHittingCooldown = false end)
end

local function adaptGetClosest()
	local char = LP.Character; if not char then return nil, math.huge end
	local hrp = char:FindFirstChild("HumanoidRootPart"); if not hrp then return nil, math.huge end
	local closest, dist = nil, math.huge
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= LP and p.Character then
			local tr = p.Character:FindFirstChild("HumanoidRootPart")
			local ph = p.Character:FindFirstChildOfClass("Humanoid")
			if tr and ph and ph.Health > 0 then
				local d = (hrp.Position - tr.Position).Magnitude
				if d < dist then dist = d; closest = p end
			end
		end
	end
	return closest, dist
end

local function startAdaptAimbot()
	if adaptAimbotConn then return end
	local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
	if hum then
		if adaptPrevAutoRotate == nil then adaptPrevAutoRotate = hum.AutoRotate end
		hum.AutoRotate = false
	end
	adaptAimbotConn = RunService.RenderStepped:Connect(function()
		if not State.autoBatToggled then return end
		local char = LP.Character; if not char then return end
		local root = char:FindFirstChild("HumanoidRootPart"); if not root then return end
		local hum2 = char:FindFirstChildOfClass("Humanoid"); if not hum2 then return end
		if not char:FindFirstChildOfClass("Tool") then
			local bat = adaptGetBat()
			if bat then pcall(function() hum2:EquipTool(bat) end) end
		end
		local targetPlr, targetDist = adaptGetClosest()
		if not targetPlr or not targetPlr.Character then return end
		local target = targetPlr.Character:FindFirstChild("HumanoidRootPart")
		if not target then return end
		local targetVel = target.AssemblyLinearVelocity
		local myPos = root.Position
		local targetPos = target.Position
		local predictPos = targetPos + targetVel * 0.14 + target.CFrame.LookVector * 0.3
		local direction = predictPos - myPos
		local flatDir = Vector3.new(direction.X, 0, direction.Z)
		if flatDir.Magnitude > 0 then flatDir = flatDir.Unit else flatDir = Vector3.new(0,0,0) end
		local desiredHeight = targetPos.Y + 3.7
		local yVel = (desiredHeight - myPos.Y) * 19.5 + targetVel.Y * 0.8
		if hum2.FloorMaterial ~= Enum.Material.Air then yVel = math.max(yVel, 13) end
		yVel = math.clamp(yVel, -70, 110)
		local chaseSpeed = State.normalSpeed or 58
		local desiredVel = Vector3.new(flatDir.X * chaseSpeed, yVel, flatDir.Z * chaseSpeed)
		root.AssemblyLinearVelocity = root.AssemblyLinearVelocity:Lerp(desiredVel, 0.8)
		local speed3 = targetVel.Magnitude
		local predictTime = math.clamp(speed3 / 150, 0.05, 0.2)
		local predictedPos = targetPos + targetVel * predictTime
		local toPredict = predictedPos - myPos
		if toPredict.Magnitude > 0.1 then
			local goalCF = CFrame.lookAt(myPos, predictedPos)
			local diffCF = root.CFrame:Inverse() * goalCF
			local rx, ry, rz = diffCF:ToEulerAnglesXYZ()
			rx = math.clamp(rx, -2.5, 2.5); ry = math.clamp(ry, -2.5, 2.5); rz = math.clamp(rz, -2.5, 2.5)
			root.AssemblyAngularVelocity = root.CFrame:VectorToWorldSpace(Vector3.new(rx*42, ry*42, rz*42))
		end
		if targetDist <= ADAPT_HIT_DIST then adaptTrySwing() end
	end)
end

local function stopAdaptAimbot()
	if adaptAimbotConn then adaptAimbotConn:Disconnect(); adaptAimbotConn = nil end
	local char = LP.Character
	local root = char and char:FindFirstChild("HumanoidRootPart")
	local hum2 = char and char:FindFirstChildOfClass("Humanoid")
	if hum2 then
		hum2.AutoRotate = (adaptPrevAutoRotate == nil) and true or adaptPrevAutoRotate
		hum2.PlatformStand = false
		pcall(function() hum2:ChangeState(Enum.HumanoidStateType.GettingUp) end)
	end
	if root then
		root.AssemblyLinearVelocity = Vector3.new(0, root.AssemblyLinearVelocity.Y * 0.3, 0)
		root.AssemblyAngularVelocity = Vector3.zero
	end
	adaptPrevAutoRotate = nil
end

LP.CharacterAdded:Connect(function() adaptPrevAutoRotate = nil end)

-- Hook into Hanami's autoBat toggle
local _origSetAutoBat = setAutoBat
setAutoBat = function(on)
	if _origSetAutoBat then _origSetAutoBat(on) end
	if on then startAdaptAimbot() else stopAdaptAimbot() end
end
if State.autoBatToggled then startAdaptAimbot() end
-- ======== END ADAPT BAT AIMBOT ========


task.spawn(function()
        while task.wait(0.5) do
                pcall(function() 
                        if progressRadLbl then
                                progressRadLbl.Text = "Steal Radius: "..AutoSteal.Radius
                        end
                end)
        end
end)

loadMainPosition()
loadMiniPosition()
loadStealBarPosition()
refreshUIToggles()
loadConfig()
