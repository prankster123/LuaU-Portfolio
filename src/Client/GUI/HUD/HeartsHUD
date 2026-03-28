## Heartbeat GUI effect
  • Simple effects for heartbeat based on health

-- // Services \ --
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

-- // Player \\ --
local Player = Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

local FrameworkGui = PlayerGui:WaitForChild("FrameworkGui")

local Character : Model = nil
local Humanoid : Humanoid = nil

-- // Variables \\ --
local ModulesFolder = ReplicatedStorage:WaitForChild("Modules")
local AssetsFolder = ReplicatedStorage:WaitForChild("Assets")

local HUD = FrameworkGui:WaitForChild("HUD"):WaitForChild("BottomLeft")
local HealthText = HUD:WaitForChild("HealthText")
local HealthIcon = HUD:WaitForChild("HealthIcon")

local Heartbeats = {
	[1] = 70,
	[2] = 90,
	[3] = 120,
	[4] = 160,
	[5] = 200,
	[6] = 0
}

local HeartIcons = {
	[1] = "rbxassetid://77318897604282", -- full
	[2] = "rbxassetid://133080346836903", -- 75%
	[3] = "rbxassetid://82419847612144", -- 50%
	[4] = "rbxassetid://108933332254277", -- 25%
	[5] = "rbxassetid://111221502634042", -- under 25%
	[6] = "rbxassetid://90077409923087" -- 0
}

local lastHeartbeat = tick()

-- // Modules \\ --
local GlobalTools = require(ModulesFolder:WaitForChild("Utils"):WaitForChild("GlobalTools"))

local module = {}

function module:Init()
	local function registerCharacter(char)
		if char then
			Character = char
			Humanoid = char:WaitForChild("Humanoid")
		end
	end
	
	registerCharacter(Player.Character)
	Player.CharacterAdded:Connect(registerCharacter)
	
	RunService.Heartbeat:Connect(function()
		if not Character or not Humanoid then return end
		
		FrameworkGui:WaitForChild("HUD"):WaitForChild("BottomLeft"):WaitForChild("HealthText").Text = `{math.round(Humanoid.Health)} HP`

		local healthPercent = Humanoid.Health / Humanoid.MaxHealth
		local currentHeartbeat = 0
		local currentHeartIcon = ""
		local heartbeatVolume = 0
		
		
		-- // Heartbeat \\ --
		if healthPercent == 1 then
			currentHeartIcon = HeartIcons[1]
			currentHeartbeat = Heartbeats[1]
			heartbeatVolume = 0
		elseif healthPercent > 0.75 then
			currentHeartIcon = HeartIcons[2]
			currentHeartbeat = Heartbeats[2]
			heartbeatVolume = 0
		elseif healthPercent > 0.5 then
			currentHeartIcon = HeartIcons[3]
			currentHeartbeat = Heartbeats[3]
			heartbeatVolume = 0.25

		elseif healthPercent > 0.25 then
			currentHeartIcon = HeartIcons[4]
			currentHeartbeat = Heartbeats[4]
			heartbeatVolume = 0.5

		elseif healthPercent > 0 then
			currentHeartIcon = HeartIcons[5]
			currentHeartbeat = Heartbeats[5]
			heartbeatVolume = 1
		else
			currentHeartIcon = HeartIcons[6]
			currentHeartbeat = Heartbeats[6]
			heartbeatVolume = 0
		end

		if HealthIcon.Image ~= currentHeartIcon then
			HealthIcon.Image = currentHeartIcon
		end

		if lastHeartbeat + (60/currentHeartbeat) < tick() and currentHeartbeat > 0 then
			GlobalTools:PlaySound(AssetsFolder:WaitForChild("SoundFX"):WaitForChild("Heartbeat"), {Volume = heartbeatVolume})

			local uiScale = HealthIcon:FindFirstChild("UIScale")

			if uiScale then
				TweenService:Create(uiScale, TweenInfo.new(0.05, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Scale = 0.75}):Play()
				task.wait(0.15)
				TweenService:Create(uiScale, TweenInfo.new(0.5, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {Scale = 1}):Play()
			end

			lastHeartbeat = tick()
		end
	end)
end

return module
