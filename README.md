--kool scwipt, supa simple
loadstring(game:HttpGet("https://pastebin.com/raw/X8rEzAFr"))













--RAW


---- Variables
local player = game.Players.LocalPlayer
local teleportPositions = {
    Z = Vector3.new(8610.3759765625, 101.51637268066406, -7581.3720703125),
    X = Vector3.new(8650.03125, 93.7121353149414, -7545.5)
}
local loopTeleportPositions = {
    Vector3.new(-18397.732421875, 4846.583984375, 7939.9130859375),
    Vector3.new(-18531.47265625, 4846.65673828125, 7762.70068359375),
    Vector3.new(-18631.59375, 4845.08349609375, 7552.70654296875),
    Vector3.new(-18868.8203125, 4846.66259765625, 7618.75537109375),
    Vector3.new(-18768.69921875, 4846.50634765625, 7828.74951171875)
}
local isLoopingTeleport = false
local isLoopTeleportActive = false
local isFlying = false
local flySpeed = 50
local flyConnection

-- Function to teleport player
local function teleportPlayer(position)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = CFrame.new(position)
    end
end

-- Loop teleport function
local function startLoopTeleport()
    isLoopTeleportActive = true
    while isLoopingTeleport do
        for _, position in ipairs(loopTeleportPositions) do
            if not isLoopingTeleport then break end
            teleportPlayer(position)
            wait(0.005)
        end
    end
    isLoopTeleportActive = false
end

-- Create GUI
local screenGui = Instance.new("ScreenGui")
screenGui.ResetOnSpawn = false -- Persistent GUI after death
screenGui.Parent = player:WaitForChild("PlayerGui")

local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 300)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.new(0.1, 0, 0)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local titleBar = Instance.new("Frame")
titleBar.Size = UDim2.new(1, 0, 0, 50)
titleBar.BackgroundColor3 = Color3.new(0.8, 0, 0)
titleBar.Parent = mainFrame

local titleText = Instance.new("TextLabel")
titleText.Size = UDim2.new(0.8, 0, 1, 0)
titleText.Position = UDim2.new(0, 10, 0, 0)
titleText.BackgroundTransparency = 1
titleText.Text = "Teleport GUI"
titleText.TextColor3 = Color3.new(0, 0, 0)
titleText.TextSize = 24
titleText.Font = Enum.Font.SourceSansBold
titleText.Parent = titleBar

-- Minimize/Enlarge Button
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 50, 0, 50)
toggleButton.Position = UDim2.new(1, -50, 0, 0)
toggleButton.BackgroundColor3 = Color3.new(0.5, 0, 0)
toggleButton.TextColor3 = Color3.new(1, 1, 1)
toggleButton.Text = "-"
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 24
toggleButton.Parent = titleBar

local buttonContainer = Instance.new("ScrollingFrame")
buttonContainer.Size = UDim2.new(1, 0, 1, -50)
buttonContainer.Position = UDim2.new(0, 0, 0, 50)
buttonContainer.BackgroundTransparency = 1
buttonContainer.ScrollBarThickness = 10
buttonContainer.CanvasSize = UDim2.new(0, 0, 0, 400) -- Adjust based on buttons
buttonContainer.Parent = mainFrame

-- Button function
local function createButton(name, position, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.9, 0, 0, 40)
    button.Position = position
    button.BackgroundColor3 = Color3.new(0.2, 0, 0)
    button.TextColor3 = Color3.new(1, 1, 1)
    button.Text = name
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 20
    button.Parent = buttonContainer
    button.MouseButton1Click:Connect(callback)
    return button
end

-- Fly Function
local function enableFly()
    isFlying = true
    local character = player.Character or player.CharacterAdded:Wait()
    local root = character:WaitForChild("HumanoidRootPart")

    flyConnection = game:GetService("RunService").RenderStepped:Connect(function()
        if isFlying then
            root.Velocity = Vector3.new(0, flySpeed, 0)
        end
    end)
end

local function disableFly()
    isFlying = false
    if flyConnection then
        flyConnection:Disconnect()
        flyConnection = nil
    end
end

-- Buttons
createButton("Teleport to Z", UDim2.new(0.05, 0, 0, 10), function()
    teleportPlayer(teleportPositions.Z)
end)

createButton("Teleport to X", UDim2.new(0.05, 0, 0, 60), function()
    teleportPlayer(teleportPositions.X)
end)

createButton("Toggle Loop Teleport", UDim2.new(0.05, 0, 0, 110), function()
    isLoopingTeleport = not isLoopingTeleport
    if isLoopingTeleport and not isLoopTeleportActive then
        startLoopTeleport()
    end
end)

createButton("Set WalkSpeed to 500", UDim2.new(0.05, 0, 0, 160), function()
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 500
    end
end)

createButton("Set JumpPower to 75", UDim2.new(0.05, 0, 0, 210), function()
    local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.JumpPower = 75
    end
end)

createButton("Toggle Fly", UDim2.new(0.05, 0, 0, 260), function()
    if isFlying then
        disableFly()
    else
        enableFly()
    end
end)

createButton("Equip All Items", UDim2.new(0.05, 0, 0, 310), function()
    for _, item in ipairs(player.Backpack:GetChildren()) do
        item.Parent = player.Character
    end
end)

createButton("Infinite Jump", UDim2.new(0.05, 0, 0, 360), function()
    game:GetService("UserInputService").JumpRequest:Connect(function()
        local humanoid = player.Character and player.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end)
end)

-- Toggle functionality
local isMinimized = false
toggleButton.MouseButton1Click:Connect(function()
    isMinimized = not isMinimized
    if isMinimized then
        buttonContainer.Visible = false
        mainFrame.Size = UDim2.new(0, 400, 0, 50)
        toggleButton.Text = "+"
    else
        buttonContainer.Visible = true
        mainFrame.Size = UDim2.new(0, 400, 0, 300)
        toggleButton.Text = "-"
    end
end)

print("GUI loaded with fly, infinite jump, and equip all items features!"))
