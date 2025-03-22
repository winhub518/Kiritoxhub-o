local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()

-- สร้าง UI หลัก
local Window = OrionLib:MakeWindow({
    Name = "Ceera Hub⚡",
    HidePremium = false,
    SaveConfig = true,
    ConfigFolder = "CeeraConfig"
})

local MainTab = Window:MakeTab({
    Name = "Home 🏠",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

-- สร้างหน้า UI
local MainTab = Window:MakeTab({
    Name = "Main",
    Icon = "rbxassetid://4483345998",
    PremiumOnly = false
})

MainTab:AddToggle({
    Name = "ออโต้ไก่ตัน 🐔🔥",
    Default = false,
    Callback = function()
-- ดรอปของ 

while Loop do
game:GetService("ReplicatedStorag").Remotes.DropItem:FireServer()
       wait(0.5)

end
})

MainTab:AddToggle({
    Name = "ออโต้เก็บไอเทม",
    Default = false,
    Callback = function()
-- เก็บไอเทม
  
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local distanceThreshold = 5 -- ระยะที่ต้องเข้าใกล้ก่อนเก็บของ

local function collectItem(item)
    if item then
        humanoid:MoveTo(item.Position) -- เดินไปหาไอเท็ม
        
        -- ตรวจสอบระยะห่างระหว่างตัวละครกับไอเท็ม
        while (humanoidRootPart.Position - item.Position).Magnitude > distanceThreshold do
            wait(0.1) -- รอจนกว่าจะเข้าใกล้พอ
        end
        
        wait(0.5) -- หน่วงเวลาเล็กน้อย
        local remote = game:GetService("ReplicatedStorage").Remotes:FindFirstChild("StoreItem")
        if remote then
            remote:FireServer(item) -- ส่งคำสั่งเก็บของไปที่เซิร์ฟเวอร์
        end
    end
end

while true do
    local item = workspace.RuntimeItems:FindFirstChild("Newspaper") -- ค้นหาไอเท็ม Newspaper
    if item then
        collectItem(item)
    end
    wait(1) -- ตรวจสอบทุก 1 วินาที
end
})        



