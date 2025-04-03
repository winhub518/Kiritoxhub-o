local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Orion/main/source"))() local Window = OrionLib:MakeWindow({Name = "Fly GUI", HidePremium = false, SaveConfig = true, ConfigFolder = "OrionConfig"})

local FlySpeed = 50 local Flying = false local AutoAttack = false local DamageMultiplier = 1 local Player = game.Players.LocalPlayer local Character = Player.Character or Player.CharacterAdded:Wait() local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart") local BodyVelocity

local function StartFlying() if Flying then return end Flying = true BodyVelocity = Instance.new("BodyVelocity") BodyVelocity.Velocity = Vector3.new(0, FlySpeed, 0) BodyVelocity.MaxForce = Vector3.new(4000, 4000, 4000) BodyVelocity.Parent = HumanoidRootPart end

local function StopFlying() Flying = false if BodyVelocity then BodyVelocity:Destroy() BodyVelocity = nil end end

local function StartAutoAttack() AutoAttack = true while AutoAttack do local Tool = Player.Character:FindFirstChildOfClass("Tool") if Tool and Tool:FindFirstChild("Activate") then Tool:Activate() end wait(0.5) end end

local function StopAutoAttack() AutoAttack = false end

local function ApplyDamageMultiplier() for _, tool in pairs(Player.Backpack:GetChildren()) do if tool:IsA("Tool") and tool:FindFirstChild("Handle") then local DamageScript = tool:FindFirstChild("Damage") if DamageScript and DamageScript:IsA("NumberValue") then DamageScript.Value = DamageScript.Value * DamageMultiplier end end end end

Window:MakeTab({ Name = "Main", Icon = "rbxassetid://4483345998", PremiumOnly = false })

local MainTab = Window:MakeTab({ Name = "Fly Controls", Icon = "rbxassetid://4483345998", PremiumOnly = false })

MainTab:AddToggle({ Name = "Enable Fly", Default = false, Callback = function(Value) if Value then StartFlying() else StopFlying() end end })

MainTab:AddSlider({ Name = "Fly Speed", Min = 10, Max = 200, Default = 50, Increment = 5, Callback = function(Value) FlySpeed = Value if Flying and BodyVelocity then BodyVelocity.Velocity = Vector3.new(0, FlySpeed, 0) end end })

MainTab:AddToggle({ Name = "Auto Attack", Default = false, Callback = function(Value) if Value then StartAutoAttack() else StopAutoAttack() end end })

MainTab:AddSlider({ Name = "Damage Multiplier", Min = 1, Max = 10, Default = 1, Increment = 0.5, Callback = function(Value) DamageMultiplier = Value ApplyDamageMultiplier() end })

OrionLib:Init()

