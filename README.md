-- โหลด Orion Library
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

-- ตัวแปรควบคุมการทำงาน
local isAutoFarmOn = false

-- ฟังก์ชันค้นหาบอล
function FindBall()
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Part") and obj.Name:lower():find("ball") then
            return obj
        end
    end
    return nil
end

-- ฟังก์ชันค้นหาประตู
function FindGoal()
    for _, v in pairs(workspace:GetChildren()) do
        if v:IsA("Part") and v.Name:lower():find("goal") then
            return v
        end
    end
    return nil
end

-- ฟังก์ชันวาร์ปไปที่ประตู
function WarpToGoal()
    local player = game.Players.LocalPlayer
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        local goal = FindGoal()
        if hrp and goal then
            hrp.CFrame = goal.CFrame + Vector3.new(0, 5, 0) -- วาร์ปไปสูงกว่าประตูนิดหน่อย
        end
    end
end

-- ฟังก์ชันยิงออโต้
function AutoShoot()
    local args = {
        [1] = 79.07267771661282, -- กำหนดแรงยิง
        [4] = Vector3.new(0.13285797834396362, -0.13623803853988647, 0.9817270636558533) -- ทิศทางลูกบอล
    }
    game:GetService("ReplicatedStorage").Packages.Knit.Services.BallService.RE.Shoot:FireServer(unpack(args))
end

-- ฟังก์ชันออโต้ฟาร์ม (ดูดบอล + วาร์ป + ยิง)
function AutoFarm()
    while isAutoFarmOn do
        local player = game.Players.LocalPlayer
        local Ball = FindBall()

        if Ball then
            -- 1. ดูดบอล
            Ball.CFrame = player.Character.HumanoidRootPart.CFrame
            task.wait(2) -- รอให้บอลมาติดตัวก่อน
            
            -- 2. วาร์ปไปที่หน้าประตู
            WarpToGoal()
            task.wait(1)

            -- 3. ยิงออโต้
            AutoShoot()
            task.wait(10) -- รอให้ลูกบอลเกิดใหม่
        end

        task.wait(0.5)
    end
end

 
-- ปุ่ม Toggle ออโต้ฟาร์ม
MainTab:AddToggle({
    Name = "ออโต้ไก่ตัน 🐔🔥",
    Default = false,
    Callback = function(state)
        isAutoFarmOn = state
        if isAutoFarmOn then
            task.spawn(AutoFarm) -- เริ่มฟาร์มออโต้
        end
    end
})

-- ปุ่มปิด UI
MainTab:AddButton({
    Name = "❌ ปิดเมนู",
    Callback = function()
        OrionLib:Destroy()
    end
})

-- แสดง UI
OrionLib:Init()
