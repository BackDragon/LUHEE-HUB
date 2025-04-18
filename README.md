local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local Window = Rayfield:CreateWindow({
Name = "LUHEE HUB",
Icon = 0,
LoadingTitle = "Rayfield Interface Suite",
LoadingSubtitle = "by DEW_KING",
Theme = "Default",

DisableRayfieldPrompts = false,
DisableBuildWarnings = false,

ConfigurationSaving = {
Enabled = true,
FolderName = nil,
FileName = "Big Hub"
},

Discord = {
Enabled = false,
Invite = "noinvitelink",
RememberJoins = true
},

KeySystem = false,
KeySettings = {
Title = "Untitled",
Subtitle = "Key System",
Note = "No method of obtaining the key is provided",
FileName = "Key",
SaveKey = true,
GrabKeyFromSite = false,
Key = {"Hello"}
}
})

local Tab = Window:CreateTab("Main")
local Section = Tab:CreateSection("Main stuff")
local Slider = Tab:CreateSlider({
Name = "Slider Example",
Range = {0, 100},
Increment = 10,
Suffix = "Bananas",
CurrentValue = 10,
Flag = "Slider1",
Callback = function(Value)
end,
})

game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 1000
local Section = Tab:CreateSection("ตั้งค่าต่างๆ")

Tab:CreateSlider({
Name = "ปรับความเร็ว",
Range = {16, 700},
Increment = 1,
Suffix = "Speed",
CurrentValue = 16,
Callback = function(Value)
local humanoid = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid.WalkSpeed = Value
end
end
})

Tab:CreateSlider({
Name = "ปรับความสูงการกระโดด",
Range = {50, 500},
Increment = 10,
Suffix = "Power",
CurrentValue = 50,
Callback = function(Value)
local humanoid = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
if humanoid then
humanoid.UseJumpPower = true
humanoid.JumpPower = Value
end
end
})

local Section = Tab:CreateSection("มองของต่างๆ")

local ESP = {}
local espEnabled = false
local itemESPEnabled = false
local espObjects = {}
local itemObjects = {}

function ESP:createESP(player)
if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
local humanoidRootPart = player.Character.HumanoidRootPart

local nameLabel = Instance.new("BillboardGui")  
    nameLabel.Adornee = humanoidRootPart  
    nameLabel.Size = UDim2.new(0, 200, 0, 50)  
    nameLabel.StudsOffset = Vector3.new(0, 3, 0)  
    nameLabel.MaxDistance = math.huge  

    local nameText = Instance.new("TextLabel")  
    nameText.Size = UDim2.new(1, 0, 1, 0)  
    nameText.Text = player.Name  
    nameText.TextColor3 = Color3.fromRGB(255, 0, 0)  
    nameText.BackgroundTransparency = 1  
    nameText.TextStrokeTransparency = 0  
    nameText.TextScaled = true  
    nameText.Font = Enum.Font.SourceSansBold  
    nameText.Parent = nameLabel  

    nameLabel.Parent = humanoidRootPart  
    espObjects[player] = nameLabel  

    player.CharacterRemoving:Connect(function()  
        if espObjects[player] then  
            espObjects[player]:Destroy()  
            espObjects[player] = nil  
        end  
    end)  
end

end

function ESP:createItemESP(item)
if item:IsA("BasePart") and item.Parent:FindFirstChild("TouchInterest") then
local nameLabel = Instance.new("BillboardGui")
nameLabel.Adornee = item
nameLabel.Size = UDim2.new(0, 150, 0, 50)
nameLabel.StudsOffset = Vector3.new(0, 3, 0)
nameLabel.MaxDistance = math.huge

local nameText = Instance.new("TextLabel")  
    nameText.Size = UDim2.new(1, 0, 1, 0)  
    nameText.Text = item.Name  
    nameText.TextColor3 = Color3.fromRGB(0, 255, 0)  
    nameText.BackgroundTransparency = 1  
    nameText.TextStrokeTransparency = 0  
    nameText.TextScaled = true  
    nameText.Font = Enum.Font.SourceSansBold  
    nameText.Parent = nameLabel  

    nameLabel.Parent = item  
    itemObjects[item] = nameLabel  

    item.AncestryChanged:Connect(function()  
        if not item:IsDescendantOf(game.Workspace) and itemObjects[item] then  
            itemObjects[item]:Destroy()  
            itemObjects[item] = nil  
        end  
    end)  
end

end

function ESP:updateESP()
for _, player in pairs(game.Players:GetPlayers()) do
if player ~= game.Players.LocalPlayer then
if espEnabled then
if not espObjects[player] then
self:createESP(player)
end
else
if espObjects[player] then
espObjects[player]:Destroy()
espObjects[player] = nil
end
end
end
end

if itemESPEnabled then  
    for _, item in pairs(game.Workspace:GetDescendants()) do  
        if item:IsA("BasePart") and item.Parent:FindFirstChild("TouchInterest") and not itemObjects[item] then  
            self:createItemESP(item)  
        end  
    end  
else  
    for _, gui in pairs(itemObjects) do  
        gui:Destroy()  
    end  
    itemObjects = {}  
end

end

local togglePlayerESP = Tab:CreateButton({
Name = "เปิด/ปิด ESP ผู้เล่น",
Callback = function()
espEnabled = not espEnabled
ESP:updateESP()
end
})

local toggleItemESP = Tab:CreateButton({
Name = "เปิด/ปิด ESP สิ่งของ",
Callback = function()
itemESPEnabled = not itemESPEnabled
ESP:updateESP()
end
})

game:GetService("RunService").RenderStepped:Connect(function()
if espEnabled or itemESPEnabled then
ESP:updateESP()
end
end)

game.Players.PlayerAdded:Connect(function(player)
player.CharacterAdded:Connect(function()
task.wait(1)
if espEnabled then
ESP:createESP(player)
end
end)
end)

local Section = Tab:CreateSection("เดินทะลุ")

local noclipEnabled = false

local function toggleNoclip()
noclipEnabled = not noclipEnabled
game:GetService("RunService").Stepped:Connect(function()
if noclipEnabled then
for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
if part:IsA("BasePart") then
part.CanCollide = false
end
end
end
end)
end

local toggleNoclipButton = Tab:CreateButton({
Name = "เปิด/ปิด เดินทะลุ",
Callback = function()
toggleNoclip()
end
})

Rayfield:LoadConfiguration()

