local TS = game:GetService("TweenService")
local newCorrection = Instance.new("ColorCorrectionEffect", game.Lighting)
local newSoundGroup = Instance.new("SoundGroup", game.SoundService)
newSoundGroup.Name = "Meteorites"

local running = true
local canSpawnMassive = true

local function NewMeteorite()
	local part = Instance.new("Part")
	local fire = Instance.new("Fire", part)
	part.Color = Color3.fromRGB(58, 46, 44)
	part.Material = Enum.Material.Slate
	part:AddTag("Meteor")
	return part
end

local function Position(meteor:Part)
	local fire = meteor:FindFirstChildWhichIsA("Fire")
	meteor.Transparency = 1
	TS:Create(meteor, TweenInfo.new(2), {Transparency = 0}):Play()
	local MeteorType = "Normal"
	
	-- SETS METEOR TYPE
	
	if math.random(1,5) == 1 then
		meteor.Size = Vector3.new(math.random(1,50),math.random(1,50),math.random(1,50))
		MeteorType = "Medium"
	else
		meteor.Size = Vector3.new(math.random(1,5),math.random(1,5),math.random(1,5))
	end
	
	if math.random(1,100) == 1 then
		meteor.Size = Vector3.new(math.random(200,250),math.random(200,250),math.random(200,250))
		MeteorType = "Huge"
	end
	
	if math.random(1,1000) == 1 and canSpawnMassive == true then
		canSpawnMassive = false
		task.delay(20, function()
			canSpawnMassive = true
		end)
		meteor.Size = Vector3.new(math.random(200,250),math.random(200,250),math.random(200,250))
		local BlockMesh = Instance.new("BlockMesh", meteor)
		BlockMesh.Scale = Vector3.new(25,25,25)
		MeteorType = "Massive"
		TS:Create(meteor, TweenInfo.new(15), {Transparency = 0}):Play()
		--TS:Create(newCorrection, TweenInfo.new(10), {Brightness = -0.2, Contrast = 0.2, Saturation = 1, TintColor = Color3.fromRGB(255, 34, 34)}):Play()
	end
	
	
	if MeteorType == "Massive" then
		meteor.CFrame = CFrame.new(math.random(-1500,1500), math.random(15000, 25500), math.random(-1500,1500))
	else
		meteor.CFrame = CFrame.new(math.random(-1500,1500), math.random(500, 2500), math.random(-1500,1500))
	end
	meteor.AssemblyLinearVelocity = Vector3.new(90, -200, 90)
	if MeteorType == "Huge" then
		meteor.AssemblyAngularVelocity = Vector3.new(math.random(-2.5,2.5), math.random(-2.5,2.5), math.random(-2.5,2.5))
	elseif MeteorType == "Massive" then
		meteor.AssemblyAngularVelocity = Vector3.new(math.random(-1,1), math.random(-1,1), math.random(-1,1))
		meteor.AssemblyLinearVelocity = Vector3.zero
	else
		meteor.AssemblyAngularVelocity = Vector3.new(math.random(-20,20), math.random(-20,20), math.random(-20,20))
	end
	
	local connection = nil

	connection = meteor.Touched:Connect(function(part)
		if not part:HasTag("Meteor") then
			connection:Disconnect()
			local explosion = Instance.new("Explosion", workspace)
			local newSound = Instance.new("Sound", meteor)
			newSound.SoundGroup = newSoundGroup
			newSound.SoundId = math.random(1,2) == 1 and "rbxassetid://9114224721" or "rbxassetid://9114224432"
			newSound.RollOffMode = Enum.RollOffMode.Linear
			newSound.RollOffMaxDistance = meteor.Size.Magnitude * 20
			newSound.PlaybackSpeed = Random.new():NextNumber(0.8,1.35)
			if MeteorType == "Huge" or "Massive" then
				newSound.PlaybackSpeed = Random.new():NextNumber(0.3,0.6)
			end
			if MeteorType == "Massive" then
				task.delay(20, function()
					running = false
				end)
				local equalizer = Instance.new("EqualizerSoundEffect", newSoundGroup)
				local Blur = Instance.new("BlurEffect", game.Lighting)
				local correction = Instance.new("ColorCorrectionEffect", game.Lighting)
				Blur.Size = 50
				correction.Brightness = 1
				local ringing = Instance.new("Sound", workspace)
				ringing.SoundId = "rbxassetid://9069161602"
				ringing:Play()
				ringing.Ended:Connect(function()
					ringing:Destroy()
				end)
				equalizer.HighGain = -80
				equalizer.LowGain = 10
				equalizer.MidGain = -80
				TS:Create(equalizer, TweenInfo.new(25), {HighGain = 0, LowGain = 0, MidGain = 0}):Play()
				TS:Create(Blur, TweenInfo.new(25), {Size = 0}):Play()
				TS:Create(correction, TweenInfo.new(25), {Brightness = 0}):Play()
				task.delay(25, function()
					equalizer:Destroy()
					Blur:Destroy()
					correction:Destroy()
				end)
				--TS:Create(newCorrection, TweenInfo.new(10), {Brightness = 0, Contrast = 0, Saturation = 0, TintColor = Color3.fromRGB(255, 255, 255)}):Play()
			end
			newSound:Play()
			explosion.Position = meteor.Position
			explosion.BlastRadius = meteor.Size.Magnitude
			meteor.Material = Enum.Material.Neon
			meteor.Color = Color3.fromRGB(255, 136, 39)
			TS:Create(meteor, TweenInfo.new(1), {Transparency = 1, Size = meteor.Size * 3.5}):Play()
			meteor.CanCollide = false
			meteor.CanTouch = false
			meteor.CastShadow = false
			meteor.CanQuery = false
			meteor.Anchored = true
			if fire then
				fire:Destroy()
			end
			task.delay(20, function()
				meteor:Destroy()
			end)
		end
	end)
end

task.spawn(function()
	while running do
		task.wait()
		local meteor = NewMeteorite()
		meteor.Parent = workspace
		Position(meteor)
	end
end)
