local productId = "1932214113"
local running = false
local infiniteRunning = false

-- إنشاء واجهة المستخدم
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))

local Frame = Instance.new("Frame", ScreenGui)
Frame.Position = UDim2.new(0.8, 0, 0.2, 0)
Frame.Size = UDim2.new(0.18, 0, 0.25, 0)
Frame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local Title = Instance.new("TextLabel", Frame)
Title.Text = "Kick Script"
Title.Size = UDim2.new(1, 0, 0.2, 0)
Title.TextColor3 = Color3.new(1, 1, 1)
Title.BackgroundTransparency = 1

local Button = Instance.new("TextButton", Frame)
Button.Text = "Off"
Button.Size = UDim2.new(0.8, 0, 0.2, 0)
Button.Position = UDim2.new(0.1, 0, 0.25, 0)
Button.TextColor3 = Color3.new(1, 0, 0)
Button.BackgroundColor3 = Color3.new(1, 1, 1)

local InfiniteButton = Instance.new("TextButton", Frame)
InfiniteButton.Text = "Infinite"
InfiniteButton.Size = UDim2.new(0.8, 0, 0.2, 0)
InfiniteButton.Position = UDim2.new(0.1, 0, 0.5, 0)
InfiniteButton.TextColor3 = Color3.new(0, 0, 0)
InfiniteButton.BackgroundColor3 = Color3.new(1, 1, 1)

local TxtButton = Instance.new("TextButton", Frame)
TxtButton.Text = "Txt"
TxtButton.Size = UDim2.new(0.8, 0, 0.2, 0)
TxtButton.Position = UDim2.new(0.1, 0, 0.5, 0)  
TxtButton.TextColor3 = Color3.new(1, 1, 1)
TxtButton.BackgroundColor3 = Color3.new(1, 0, 0)

local ChangeServerButton = Instance.new("TextButton", Frame)
ChangeServerButton.Text = "Change Server"
ChangeServerButton.Size = UDim2.new(0.8, 0, 0.2, 0)
ChangeServerButton.Position = UDim2.new(0.1, 0, 0.75, 0)
ChangeServerButton.TextColor3 = Color3.new(0, 0, 0)
ChangeServerButton.BackgroundColor3 = Color3.new(0, 0, 0)

-- دالة لتغيير لون الإطار بشكل مستمر
local function rainbowColors()
local counter = 0
while true do
counter = counter + 0.01
Frame.BackgroundColor3 = Color3.fromHSV(counter % 1, 1, 1)
wait(0.1)
end
end

-- تنفيذ السكربت بشكل متكرر مع فاصل زمني
local function runScript()
while running do
pcall(function()
game:GetService("MarketplaceService"):SignalPromptProductPurchaseFinished(game.Players.LocalPlayer.UserId, tonumber(productId), true)
end)
wait(1)
end
end


-- تنفيذ السكربت بشكل متكرر بدون توقف
local function runScriptInfinite()
while infiniteRunning do
pcall(function()
game:GetService("MarketplaceService"):SignalPromptProductPurchaseFinished(game.Players.LocalPlayer.UserId, tonumber(productId), true)
end)
end
end

-- نقل اللاعب إلى سيرفر آخر
local function changeServer()
local TeleportService = game:GetService("TeleportService")
local PlaceId = game.PlaceId
TeleportService:Teleport(PlaceId, game.Players.LocalPlayer)
end

-- التحكم في تشغيل وإيقاف السكربت
Button.MouseButton1Click:Connect(function()
running = not running
if running then
Button.Text = "On"
Button.TextColor3 = Color3.new(0, 1, 0)
spawn(runScript)
else
Button.Text = "Off"
Button.TextColor3 = Color3.new(1, 0, 0)
end
end)


local function sendSpamMessage()
TxtButton.BackgroundColor3 = Color3.new(0, 1, 0)
for i = 1, 5 do
game.ReplicatedStorage.DefaultChatSystemChatEvents.SayMessageRequest:FireServer("Goooo To Hell Everyone", "All")
wait(0.1)
end
TxtButton.BackgroundColor3 = Color3.new(1, 0, 0)
end

TxtButton.MouseButton1Click:Connect(sendSpamMessage)


-- التحكم في تشغيل السكربت بشكل لا نهائي وطرد اللاعب أولاً
InfiniteButton.MouseButton1Click:Connect(function()
infiniteRunning = true
game.Players.LocalPlayer:Kick("Done, Sir")
wait(5)
spawn(runScriptInfinite)
end)

-- زر تغيير السيرفر
ChangeServerButton.MouseButton1Click:Connect(changeServer)

-- بدء تغيير الألوان
spawn(rainbowColors)
