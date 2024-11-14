game.StarterGui:SetCore("SendNotification", {
Title = "Script Activated";
Text = "Aim Track enabled!";
Duration = 5;
})

local aimTrackEnabled = true

-- إنشاء الدائرة في منتصف الشاشة
local function createCircle()
local camera = game.Workspace.CurrentCamera
local circle = Drawing.new("Circle")
circle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
circle.Radius = 100
circle.Color = Color3.fromRGB(255, 0, 0) -- اللون الأحمر
circle.Thickness = 2
circle.Transparency = 0.5
circle.Visible = true
return circle
end

local circle = createCircle()

-- إنشاء الزر على يسار الشاشة
local function createToggleButton()
local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local ToggleButton = Instance.new("TextButton", screenGui)
ToggleButton.Size = UDim2.new(0, 100, 0, 50)
ToggleButton.Position = UDim2.new(0, 10, 0.5, -25)
ToggleButton.BackgroundColor3 = Color3.new(0, 0, 0)
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.Text = "إغلاق"

ToggleButton.MouseButton1Click:Connect(function()
aimTrackEnabled = not aimTrackEnabled
if aimTrackEnabled then
circle.Visible = true
ToggleButton.Text = "إغلاق"
else
circle.Visible = false
ToggleButton.Text = "تشغيل"
end
end)
end

createToggleButton()

-- Aim Track مع الدائرة والتفعيل فقط على الأعداء
local function enableAimTrack()
local player = game.Players.LocalPlayer
local camera = game.Workspace.CurrentCamera

local function isTargetVisible(target)
local origin = camera.CFrame.Position
local direction = (target.Position - origin).unit
local ray = Ray.new(origin, direction * 5000)
local hit = game.Workspace:FindPartOnRayWithIgnoreList(ray, {player.Character})
return hit and hit:IsDescendantOf(target.Parent)
end

local function isEnemy(character)
return character:FindFirstChild("TeamColor") and character.TeamColor.Value == "Red"
end

local function getClosestTarget()
local closestTarget = nil
local shortestDistance = math.huge

for i, v in pairs(game.Players:GetPlayers()) do
if v ~= player and v.Character and v.Character:FindFirstChild("Head") and isEnemy(v.Character) then
local screenPoint = camera:WorldToViewportPoint(v.Character.Head.Position)
local distance = (Vector2.new(screenPoint.X, screenPoint.Y) - circle.Position).magnitude
if distance < circle.Radius and distance < shortestDistance and isTargetVisible(v.Character.Head) then
closestTarget = v
shortestDistance = distance
end
end
end

return closestTarget
end

game:GetService("RunService").RenderStepped:Connect(function()
if aimTrackEnabled then
local target = getClosestTarget()
if target and target.Character and target.Character:FindFirstChild("Head") then
camera.CFrame = CFrame.new(camera.CFrame.Position, target.Character.Head.Position)
end
end
end)
end

enableAimTrack()
