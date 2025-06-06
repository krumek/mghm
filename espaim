local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local TweenService = game:GetService("TweenService")
local SoundService = game:GetService("SoundService")

-- Settings
local Settings = {
    AimBot = {Enabled = false, FOV = 100, Smoothness = 0.05, Key = Enum.KeyCode.Q},
    ESP = {Enabled = false, Box = true, Tracer = true, Key = Enum.KeyCode.E},
    Misc = {Funny = false, Key = Enum.KeyCode.F},
    WallBang = {Enabled = false, Key = Enum.KeyCode.R},
    KillEffect = {Enabled = false, Key = Enum.KeyCode.T}
}

-- Instances
local Instances = {ESP = {}, KillEffects = {}}
local lastShot = tick()
local lastKill = tick()

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "FestonMenu"
ScreenGui.Enabled = false

local MainFrame = Instance.new("Frame")
MainFrame.Parent = ScreenGui
MainFrame.Size = UDim2.new(0, 200, 0, 350)
MainFrame.Position = UDim2.new(0.5, -100, 0.5, -175)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BorderSizePixel = 0

local TopBar = Instance.new("Frame")
TopBar.Parent = MainFrame
TopBar.Size = UDim2.new(1, 0, 0, 40)
TopBar.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
TopBar.BorderSizePixel = 0

local Title = Instance.new("TextLabel")
Title.Parent = TopBar
Title.Size = UDim2.new(1, 0, 1, 0)
Title.Text = "Feston 18+"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 20

-- Femboy Image
local FemboyImage = Instance.new("ImageLabel")
FemboyImage.Parent = MainFrame
FemboyImage.Size = UDim2.new(0, 180, 0, 120)
FemboyImage.Position = UDim2.new(0, 10, 0, 50)
FemboyImage.BackgroundTransparency = 1
FemboyImage.Image = "rbxassetid://1842809352" -- Замените на актуальный ID

local ButtonFrame = Instance.new("Frame")
ButtonFrame.Parent = MainFrame
ButtonFrame.Size = UDim2.new(1, 0, 0, 180)
ButtonFrame.Position = UDim2.new(0, 0, 0, 170)
ButtonFrame.BackgroundTransparency = 1

local ESPButton = Instance.new("TextButton")
ESPButton.Parent = ButtonFrame
ESPButton.Size = UDim2.new(0, 180, 0, 30)
ESPButton.Position = UDim2.new(0, 10, 0, 0)
ESPButton.Text = "ESP: OFF"
ESPButton.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.Font = Enum.Font.SourceSans
ESPButton.TextSize = 16

local AimBotButton = Instance.new("TextButton")
AimBotButton.Parent = ButtonFrame
AimBotButton.Size = UDim2.new(0, 180, 0, 30)
AimBotButton.Position = UDim2.new(0, 10, 0, 40)
AimBotButton.Text = "AimBot: OFF"
AimBotButton.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
AimBotButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AimBotButton.Font = Enum.Font.SourceSans
AimBotButton.TextSize = 16

local MiscButton = Instance.new("TextButton")
MiscButton.Parent = ButtonFrame
MiscButton.Size = UDim2.new(0, 180, 0, 30)
MiscButton.Position = UDim2.new(0, 10, 0, 80)
MiscButton.Text = "Funny: OFF"
MiscButton.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
MiscButton.TextColor3 = Color3.fromRGB(255, 255, 255)
MiscButton.Font = Enum.Font.SourceSans
MiscButton.TextSize = 16

local WallBangButton = Instance.new("TextButton")
WallBangButton.Parent = ButtonFrame
WallBangButton.Size = UDim2.new(0, 180, 0, 30)
WallBangButton.Position = UDim2.new(0, 10, 0, 120)
WallBangButton.Text = "WallBang: OFF"
WallBangButton.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
WallBangButton.TextColor3 = Color3.fromRGB(255, 255, 255)
WallBangButton.Font = Enum.Font.SourceSans
WallBangButton.TextSize = 16

local KillEffectButton = Instance.new("TextButton")
KillEffectButton.Parent = ButtonFrame
KillEffectButton.Size = UDim2.new(0, 180, 0, 30)
KillEffectButton.Position = UDim2.new(0, 10, 0, 160)
KillEffectButton.Text = "KillEffect: OFF"
KillEffectButton.BackgroundColor3 = Color3.fromRGB(255, 0, 255)
KillEffectButton.TextColor3 = Color3.fromRGB(255, 255, 255)
KillEffectButton.Font = Enum.Font.SourceSans
KillEffectButton.TextSize = 16

-- ESP Logic (Target: Head)
local function createESP(player)
    local box = Drawing.new("Square")
    box.Thickness = 2
    box.Color = Color3.fromRGB(255, 0, 255)
    box.Filled = false

    local tracer = Drawing.new("Line")
    tracer.Thickness = 2
    tracer.Color = Color3.fromRGB(255, 0, 255)

    Instances.ESP[player] = {box = box, tracer = tracer}
    player.CharacterAdded:Connect(function()
        box.Visible = false
        tracer.Visible = false
    end)

    -- Cleanup on player leave
    player.AncestryChanged:Connect(function()
        if not player.Parent then
            box:Remove()
            tracer:Remove()
            Instances.ESP[player] = nil
        end
    end)
end

RunService.RenderStepped:Connect(function()
    if Settings.ESP.Enabled then
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and not Instances.ESP[player] then
                createESP(player)
            end
            local esp = Instances.ESP[player]
            if esp and player.Character and player.Character:FindFirstChild("Head") and player.Character.Humanoid.Health > 0 then
                local head = player.Character.Head
                local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)

                if onScreen then
                    local pos = Camera:WorldToViewportPoint(head.Position)
                    local size = (Camera:WorldToViewportPoint(head.Position - Vector3.new(0, 2, 0)) - pos).Y
                    local width = size / 2

                    if Settings.ESP.Box then
                        esp.box.Size = Vector2.new(width, size)
                        esp.box.Position = Vector2.new(pos.X - width / 2, pos.Y - size)
                        esp.box.Visible = true
                    else
                        esp.box.Visible = false
                    end

                    if Settings.ESP.Tracer then
                        esp.tracer.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                        esp.tracer.To = Vector2.new(pos.X, pos.Y + size / 2)
                        esp.tracer.Visible = true
                    else
                        esp.tracer.Visible = false
                    end
                else
                    esp.box.Visible = false
                    esp.tracer.Visible = false
                end
            else
                if esp then
                    esp.box.Visible = false
                    esp.tracer.Visible = false
                end
            end
        end
    else
        for _, esp in pairs(Instances.ESP) do
            esp.box.Visible = false
            esp.tracer.Visible = false
        end
    end

    if Settings.AimBot.Enabled and UserInputService:IsKeyDown(Settings.AimBot.Key) and LocalPlayer.Character then
        local target = nil
        local shortest = math.huge
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") and player.Character.Humanoid.Health > 0 then
                local headPos = Camera:WorldToViewportPoint(player.Character.Head.Position)
                local distance = (Vector2.new(headPos.X, headPos.Y) - UserInputService:GetMouseLocation()).Magnitude
                if distance < shortest and distance <= Settings.AimBot.FOV then
                    shortest = distance
                    target = player.Character.Head
                end
            end
        end
        if target then
            Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
            TweenService:Create(Camera, TweenInfo.new(Settings.AimBot.Smoothness), {CFrame = CFrame.new(Camera.CFrame.Position, target.Position)}):Play()
        end
    end
end)

-- Bullet Tracers for WallBang
local function createBulletTracer(startPos, endPos)
    local tracer = Instance.new("Part")
    tracer.Parent = workspace
    tracer.Size = Vector3.new(0.1, 0.1, (startPos - endPos).Magnitude)
    tracer.CFrame = CFrame.lookAt(startPos, endPos) * CFrame.new(0, 0, -tracer.Size.Z / 2)
    tracer.Material = Enum.Material.Neon
    tracer.Color = Color3.fromRGB(255, 0, 0)
    tracer.Anchored = true
    spawn(function()
        for i = 0, 1, 0.1 do
            tracer.Transparency = i
            wait(0.05)
        end
        tracer:Destroy()
    end)
end

RunService.RenderStepped:Connect(function()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChildWhichIsA("Tool") then
        local tool = LocalPlayer.Character:FindFirstChildWhichIsA("Tool")
        local fireSound = tool:FindFirstChild("Fire", true)
        if fireSound and fireSound.IsPlaying and tick() - lastShot > 0.1 then
            lastShot = tick()
            local startPos = tool:FindFirstChild("Handle") and tool.Handle.Position or LocalPlayer.Character.HumanoidRootPart.Position
            local ray = Ray.new(startPos, Camera.CFrame.LookVector * 1000)
            local ignoreList = {LocalPlayer.Character}
            if not Settings.WallBang.Enabled then
                table.insert(ignoreList, workspace)
            end
            local hit, endPos = workspace:FindPartOnRayWithIgnoreList(ray, ignoreList)
            if hit then
                createBulletTracer(startPos, endPos)
                if hit.Parent:FindFirstChild("Humanoid") then
                    hit.Parent.Humanoid:TakeDamage(100)
                end
            else
                createBulletTracer(startPos, startPos + Camera.CFrame.LookVector * 1000)
            end
        end
    end
end)

-- Kill Effects
RunService.RenderStepped:Connect(function()
    if Settings.KillEffect.Enabled and tick() - lastKill > 0.5 then
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health <= 0 and not Instances.KillEffects[player] then
                lastKill = tick()
                Instances.KillEffects[player] = true
                local explosion = Instance.new("Explosion")
                explosion.Position = player.Character.HumanoidRootPart.Position
                explosion.BlastRadius = 5
                explosion.BlastPressure = 0
                explosion.Parent = workspace
                player.CharacterAdded:Connect(function()
                    Instances.KillEffects[player] = nil
                end)
            end
        end
    end
end)

-- Input Handling
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Insert then
        ScreenGui.Enabled = not ScreenGui.Enabled
    elseif input.KeyCode == Settings.ESP.Key then
        Settings.ESP.Enabled = not Settings.ESP.Enabled
        ESPButton.Text = "ESP: " .. (Settings.ESP.Enabled and "ON" or "OFF")
    elseif input.KeyCode == Settings.AimBot.Key then
        Settings.AimBot.Enabled = not Settings.AimBot.Enabled
        AimBotButton.Text = "AimBot: " .. (Settings.AimBot.Enabled and "ON" or "OFF")
    elseif input.KeyCode == Settings.Misc.Key then
        Settings.Misc.Funny = not Settings.Misc.Funny
        MiscButton.Text = "Funny: " .. (Settings.Misc.Funny and "ON" or "OFF")
        if Settings.Misc.Funny then
            local sound = Instance.new("Sound")
            sound.Parent = SoundService
            sound.SoundId = "rbxassetid://1837846153" -- Замените на звук 18+
            sound.Volume = 1
            sound:Play()
            sound.Ended:Connect(function() sound:Destroy() end)
        end
    elseif input.KeyCode == Settings.WallBang.Key then
        Settings.WallBang.Enabled = not Settings.WallBang.Enabled
        WallBangButton.Text = "WallBang: " .. (Settings.WallBang.Enabled and "ON" or "OFF")
    elseif input.KeyCode == Settings.KillEffect.Key then
        Settings.KillEffect.Enabled = not Settings.KillEffect.Enabled
        KillEffectButton.Text = "KillEffect: " .. (Settings.KillEffect.Enabled and "ON" or "OFF")
    end
end)

-- Watermark
local Watermark = Instance.new("TextLabel")
Watermark.Parent = ScreenGui
Watermark.Size = UDim2.new(0, 150, 0, 20)
Watermark.Position = UDim2.new(0, 10, 0, 10)
Watermark.BackgroundTransparency = 1
Watermark.TextColor3 = Color3.fromRGB(255, 0, 255)
Watermark.Text = "Feston - 06/06/2025"
Watermark.Font = Enum.Font.SourceSansBold
Watermark.TextSize = 14
