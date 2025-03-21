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
local BallService = ReplicatedStorage.Packages.Knit.Services.BallService.RE
local player = game.Players.LocalPlayer

-- ✅ ออโต้สไลด์ + ดูดบอล
Tab:AddToggle({
	Name = "ออโต้สไลด์ดูดบอล❌",
	Default = false,
	Callback = function(Value)
		if Value then  
			-- 🏃 กดปุ่มสไลด์
			BallService.Slide:FireServer()

			-- 🎯 ดูดบอลเข้าหาตัว
			task.spawn(function()
				while Value do
					local ball = workspace:FindFirstChild("Football")

					if ball and player.Character then
						local root = player.Character:FindFirstChild("HumanoidRootPart")
						if root then
							ball.CFrame = root.CFrame * CFrame.new(0, 0, -2) -- ดูดบอลมาอยู่ข้างหน้า
						end
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
        local args = {
            [1] = 79.07267771661282, -- ค่าพลังยิง
            [4] = Vector3.new(0.132, -0.136, 0.981) + Vector3.new(0.2, 0.1, 0.3) -- เพิ่มแรงโค้ง
        }

        -- 🚀 ยิงบอลแบบโค้ง
        BallService.Shoot:FireServer(unpack(args))
    end
end)
