local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/jensonhirst/Orion/main/source')))()

local Window = OrionLib:MakeWindow({Name = "Kirito hub🌚", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionTest"})

local Tab = Window:MakeTab({
	Name = "Mani⚡",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

local Section = Tab:AddSection({
	Name = "ออโต้ต่างๆ🔥"
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- ✅ ออโต้สไลด์ + ดูดบอล
Tab:AddToggle({
	Name = "ออโต้สไลด์ดูดบอล❌",
	Default = false,
	Callback = function(Value)
		if Value then  
			local SlideEvent = ReplicatedStorage.Packages.Knit.Services.BallService.RE.Slide

			-- 🏃 กดปุ่มสไลด์
			SlideEvent:FireServer()

			-- 🎯 ดูดบอลเข้าหาตัว
			task.spawn(function()
				while Value do
					local ball
					for _, obj in pairs(workspace:GetChildren()) do
						if obj:IsA("Model") and obj:FindFirstChild("BallAnims") then
							ball = obj
							break
						end
					end

					if character and ball then
						ball:PivotTo(humanoidRootPart.CFrame * CFrame.new(0, 0, -2)) -- ย้ายบอลมาอยู่ข้างหน้า
					end

					task.wait(0.1) -- ปรับความเร็ว
				end
			end)
		end
	end
})

-- ✅ ยิงโค้งอัตโนมัติ
local autoCurveShot = false

Tab:AddToggle({
    Name = "ยิงโค้งเมื่อกดปุ่มยิง⚽",
    Default = false,
    Callback = function(Value)
        autoCurveShot = Value
    end    
})

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    if autoCurveShot and input.UserInputType == Enum.UserInputType.MouseButton1 then -- คลิกซ้ายยิง
        local ShootEvent = ReplicatedStorage.Packages.Knit.Services.BallService.RE.Shoot

        -- 📌 ดักค่าที่ถูกต้อง (ต้องลองเอง)
        local direction = humanoidRootPart.CFrame.LookVector * 100
        local curveEffect = Vector3.new(15, 5, 25) -- ปรับให้บอลโค้ง

        -- 🚀 ยิงบอล
        ShootEvent:FireServer(direction + curveEffect)
    end
end)
