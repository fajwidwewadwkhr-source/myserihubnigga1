--// Ultra Smooth Red & Black Settings GUI + "Steal Brainrot" Tab //--

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

--// Create GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SmoothRedBlackGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = player:WaitForChild("PlayerGui")

--// Main Frame
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 550, 0, 340)
MainFrame.Position = UDim2.new(0.5, -275, 0.5, -170)
MainFrame.BackgroundColor3 = Color3.fromRGB(20, 0, 0)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Visible = false
MainFrame.Parent = ScreenGui

local Corner = Instance.new("UICorner", MainFrame)
Corner.CornerRadius = UDim.new(0, 14)

local Shadow = Instance.new("ImageLabel", MainFrame)
Shadow.Size = UDim2.new(1, 40, 1, 40)
Shadow.Position = UDim2.new(0, -20, 0, -20)
Shadow.BackgroundTransparency = 1
Shadow.Image = "rbxassetid://5554236805"
Shadow.ImageColor3 = Color3.fromRGB(255, 0, 0)
Shadow.ImageTransparency = 0.75
Shadow.ZIndex = -1

--// Top Bar
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 45)
TopBar.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame

local TopCorner = Instance.new("UICorner", TopBar)
TopCorner.CornerRadius = UDim.new(0, 14)

local Title = Instance.new("TextLabel")
Title.Text = "⚙ SETTINGS"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 20, 0, 6)
Title.Size = UDim2.new(0, 300, 1, 0)
Title.Parent = TopBar

--// Tab Bar
local TabBar = Instance.new("Frame")
TabBar.Size = UDim2.new(1, -30, 0, 36)
TabBar.Position = UDim2.new(0, 15, 0, 50)
TabBar.BackgroundTransparency = 1
TabBar.Parent = MainFrame

local function makeTabButton(text, posX)
	local btn = Instance.new("TextButton")
	btn.Size = UDim2.new(0, 120, 1, 0)
	btn.Position = UDim2.new(0, posX, 0, 0)
	btn.BackgroundColor3 = Color3.fromRGB(130, 0, 0)
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 16
	btn.TextColor3 = Color3.fromRGB(255,255,255)
	btn.AutoButtonColor = false
	btn.Parent = TabBar
	local corner = Instance.new("UICorner", btn)
	corner.CornerRadius = UDim.new(0, 8)
	local stroke = Instance.new("UIStroke", btn)
	stroke.Color = Color3.fromRGB(255,0,0)
	stroke.Thickness = 1
	return btn
end

local SettingsTabBtn = makeTabButton("Settings", 0)
local StealTabBtn = makeTabButton("Steal Brainrot", 130)

--// Content
local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -30, 1, -110)
ContentFrame.Position = UDim2.new(0, 15, 0, 95)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

--// Settings content (original)
local SettingsInner = Instance.new("Frame")
SettingsInner.Size = UDim2.new(1, 0, 1, 0)
SettingsInner.BackgroundTransparency = 1
SettingsInner.Parent = ContentFrame

--// Button (original)
local HotkeyButton = Instance.new("TextButton")
HotkeyButton.Size = UDim2.new(0, 320, 0, 55)
HotkeyButton.Position = UDim2.new(0.5, -160, 0, 10)
HotkeyButton.BackgroundColor3 = Color3.fromRGB(130, 0, 0)
HotkeyButton.Text = "Set Unload Script Hotkey"
HotkeyButton.Font = Enum.Font.GothamBold
HotkeyButton.TextSize = 18
HotkeyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
HotkeyButton.AutoButtonColor = false
HotkeyButton.Parent = SettingsInner

local HotkeyCorner = Instance.new("UICorner", HotkeyButton)
HotkeyCorner.CornerRadius = UDim.new(0, 10)

local UIStroke = Instance.new("UIStroke", HotkeyButton)
UIStroke.Color = Color3.fromRGB(255, 0, 0)
UIStroke.Thickness = 1.5
UIStroke.Transparency = 0.2

--// Steal Tab Content
local StealFrame = Instance.new("Frame")
StealFrame.Size = UDim2.new(1, 0, 1, 0)
StealFrame.BackgroundTransparency = 1
StealFrame.Visible = false
StealFrame.Parent = ContentFrame

-- Buttons for StealFrame
local function makeStealBtn(text, y)
	local b = Instance.new("TextButton")
	b.Size = UDim2.new(0, 380, 0, 48)
	b.Position = UDim2.new(0.5, -190, 0, y)
	b.BackgroundColor3 = Color3.fromRGB(130, 0, 0)
	b.Text = text
	b.Font = Enum.Font.GothamBold
	b.TextSize = 18
	b.TextColor3 = Color3.fromRGB(255,255,255)
	b.AutoButtonColor = false
	b.Parent = StealFrame
	local corn = Instance.new("UICorner", b)
	corn.CornerRadius = UDim.new(0, 10)
	local s = Instance.new("UIStroke", b)
	s.Color = Color3.fromRGB(255,0,0)
	s.Thickness = 1.3
	return b
end

local DesyncBtn = makeStealBtn("Desync (Anti-Hit): OFF", 10)
local TeleportBaseBtn = makeStealBtn("Teleport To Base", 70)
local FindBrainrotsBtn = makeStealBtn("Find Brainrots", 130)
local AutoStealBtn = makeStealBtn("Auto-Steal Nearest Brainrot: OFF", 190)

--// Variables
local currentHotkey = nil
local selectingKey = false
local toggling = false

-- Steal-tab variables & helpers
local desyncEnabled = false
local desyncConnection = nil

local autoStealEnabled = false
local autoStealConnection = nil
local highlightedBrainrots = {} -- store billboard guis for removal

local function Tween(object, props, time, style)
	TweenService:Create(object, TweenInfo.new(time or 0.4, style or Enum.EasingStyle.Quint, Enum.EasingDirection.Out), props):Play()
end

-- Hover Animation (original)
HotkeyButton.MouseEnter:Connect(function()
	Tween(HotkeyButton, {BackgroundColor3 = Color3.fromRGB(180, 0, 0)}, 0.3)
	UIStroke.Thickness = 2.5
end)

HotkeyButton.MouseLeave:Connect(function()
	if not selectingKey then
		Tween(HotkeyButton, {BackgroundColor3 = Color3.fromRGB(130, 0, 0)}, 0.3)
		UIStroke.Thickness = 1.5
	end
end)

-- Hotkey Selection (original)
HotkeyButton.MouseButton1Click:Connect(function()
	if selectingKey then return end
	selectingKey = true
	HotkeyButton.Text = "Press any key..."
	Tween(HotkeyButton, {BackgroundColor3 = Color3.fromRGB(200, 0, 0)}, 0.3)

	local connection
	connection = UserInputService.InputBegan:Connect(function(input, gp)
		if gp then return end
		if input.UserInputType == Enum.UserInputType.Keyboard then
			currentHotkey = input.KeyCode
			selectingKey = false
			HotkeyButton.Text = "Hotkey: " .. tostring(currentHotkey.Name)
			Tween(HotkeyButton, {BackgroundColor3 = Color3.fromRGB(130, 0, 0)}, 0.4)
			UIStroke.Thickness = 1.5
			connection:Disconnect()
		end
	end)
end)

-- Toggle GUI on Hotkey (original)
UserInputService.InputBegan:Connect(function(input, gp)
	if gp or not currentHotkey then return end
	if input.KeyCode == currentHotkey and not toggling then
		toggling = true
		if MainFrame.Visible then
			Tween(MainFrame, {Position = UDim2.new(0.5, -275, 0.5, -130), BackgroundTransparency = 1}, 0.35)
			task.wait(0.25)
			MainFrame.Visible = false
		else
			MainFrame.Visible = true
			MainFrame.BackgroundTransparency = 1
			MainFrame.Position = UDim2.new(0.5, -275, 0.5, -130)
			Tween(MainFrame, {Position = UDim2.new(0.5, -275, 0.5, -170), BackgroundTransparency = 0}, 0.45)
		end
		task.wait(0.3)
		toggling = false
	end
end)

-- Tab switching logic
local function showSettingsTab()
	Title.Text = "⚙ SETTINGS"
	SettingsInner.Visible = true
	StealFrame.Visible = false
	Tween(SettingsTabBtn, {BackgroundColor3 = Color3.fromRGB(180,0,0)}, 0.15)
	Tween(StealTabBtn, {BackgroundColor3 = Color3.fromRGB(130,0,0)}, 0.15)
end

local function showStealTab()
	Title.Text = "🏴 Steal Brainrot"
	SettingsInner.Visible = false
	StealFrame.Visible = true
	Tween(StealTabBtn, {BackgroundColor3 = Color3.fromRGB(180,0,0)}, 0.15)
	Tween(SettingsTabBtn, {BackgroundColor3 = Color3.fromRGB(130,0,0)}, 0.15)
end

SettingsTabBtn.MouseButton1Click:Connect(showSettingsTab)
StealTabBtn.MouseButton1Click:Connect(showStealTab)

-- initialize tab colors
Tween(SettingsTabBtn, {BackgroundColor3 = Color3.fromRGB(180,0,0)}, 0.01)
Tween(StealTabBtn, {BackgroundColor3 = Color3.fromRGB(130,0,0)}, 0.01)

------ Steal tab functionality ----

local function getRoot()
	local char = player.Character or player.CharacterAdded:Wait()
	local root = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
	return char, root
end

-- Desync
DesyncBtn.MouseButton1Click:Connect(function()
	desyncEnabled = not desyncEnabled
	DesyncBtn.Text = "Desync (Anti-Hit): " .. (desyncEnabled and "ON" or "OFF")
	if desyncEnabled then
		desyncConnection = RunService.Heartbeat:Connect(function(dt)
			local ok, root = pcall(getRoot)
			if not ok or not root then return end
			local _, r = getRoot()
			if r and r:IsA("BasePart") then
				local nx = (math.random() - 0.5) * 0.06
				local nz = (math.random() - 0.5) * 0.06
				local offset = Vector3.new(nx, 0, nz)
				pcall(function()
					r.CFrame = r.CFrame + offset
				end)
			end
		end)
	else
		if desyncConnection then
			desyncConnection:Disconnect()
			desyncConnection = nil
		end
	end
end)

-- Teleport to **your own base** only
TeleportBaseBtn.MouseButton1Click:Connect(function()
	local char, root = getRoot()
	if not root then return end
	local found = nil

	-- look for a folder with player's name
	local basesFolder = workspace:FindFirstChild("Bases") or workspace:FindFirstChild("PlayerBases")
	if basesFolder then
		found = basesFolder:FindFirstChild(player.Name)
		if found and found:IsA("Model") then
			found = found.PrimaryPart or found:FindFirstChildWhichIsA("BasePart")
		end
	end

	-- fallback: find part with player's name
	if not found then
		for _, obj in ipairs(workspace:GetDescendants()) do
			if obj:IsA("BasePart") and tostring(obj.Name):lower():find(player.Name:lower()) then
				found = obj
				break
			end
		end
	end

	if found then
		pcall(function()
			root.CFrame = found.CFrame + Vector3.new(0, 4, 0)
		end)
	else
		warn("No personal base found for player!")
	end
end)

-- Find Brainrots
local function clearHighlights()
	for _, g in ipairs(highlightedBrainrots) do
		if g and g.Parent then pcall(function() g:Destroy() end) end
	end
	highlightedBrainrots = {}
end

FindBrainrotsBtn.MouseButton1Click:Connect(function()
	clearHighlights()
	local foundAny = 0
	for _, obj in ipairs(workspace:GetDescendants()) do
		if obj:IsA("BasePart") then
			local n = tostring(obj.Name):lower()
			if n:find("brainrot") or n:find("brain") or n:find("rot") then
				local bill = Instance.new("BillboardGui")
				bill.Adornee = obj
				bill.Size = UDim2.new(0, 120, 0, 28)
				bill.StudsOffset = Vector3.new(0, 2, 0)
				bill.AlwaysOnTop = true
				bill.Parent = ScreenGui

				local label = Instance.new("TextLabel", bill)
				label.Size = UDim2.new(1, 0, 1, 0)
				label.BackgroundTransparency = 0.5
				label.BackgroundColor3 = Color3.fromRGB(30,0,0)
				label.TextColor3 = Color3.fromRGB(255,255,255)
				label.TextStrokeTransparency = 0.8
				label.Font = Enum.Font.GothamBold
				label.TextSize = 14
				label.Text = "Brainrot"

				table.insert(highlightedBrainrots, bill)
				foundAny = foundAny + 1
			end
		end
	end
end)

-- Auto-Steal
local function findBrainrotParts()
	local list = {}
	for _, obj in ipairs(workspace:GetDescendants()) do
		if obj:IsA("BasePart") then
			local n = tostring(obj.Name):lower()
			if n:find("brainrot") or n:find("brain") then
				table.insert(list, obj)
			end
		elseif obj:IsA("Model") and (tostring(obj.Name):lower():find("brainrot") or tostring(obj.Name):lower():find("brain")) then
			local p = obj.PrimaryPart or obj:FindFirstChildWhichIsA("BasePart")
			if p then table.insert(list, p) end
		end
	end
	return list
end

local function nearestPartFromRoot(root, parts)
	if not root then return nil end
	local nearest, nd = nil, math.huge
	for _, p in ipairs(parts) do
		if p and p.Parent then
			local d = (p.Position - root.Position).magnitude
			if d < nd then nd = d; nearest = p end
		end
	end
	return nearest, nd
end

AutoStealBtn.MouseButton1Click:Connect(function()
	autoStealEnabled = not autoStealEnabled
	AutoStealBtn.Text = "Auto-Steal Nearest Brainrot: " .. (autoStealEnabled and "ON" or "OFF")

	if autoStealEnabled then
		autoStealConnection = RunService.Heartbeat:Connect(function()
			local char, root = getRoot()
			if not root or not root.Parent then return end
			local list = findBrainrotParts()
			if #list == 0 then return end
			local target, dist = nearestPartFromRoot(root, list)
			if target and target.Parent then
				pcall(function()
					root.CFrame = target.CFrame + Vector3.new(0, 3, 0)
				end)
			end
			task.wait(0.25)
		end)
	else
		if autoStealConnection then
			autoStealConnection:Disconnect()
			autoStealConnection = nil
		end
	end
end)

-- clean up highlights when GUI destroyed
ScreenGui.AncestryChanged:Connect(function(_, parent)
	if not parent then
		clearHighlights()
		if desyncConnection then desyncConnection:Disconnect() end
		if autoStealConnection then autoStealConnection:Disconnect() end
	end
end)

-- Opening Animation
task.wait(0.5)
MainFrame.Visible = true
MainFrame.BackgroundTransparency = 1
MainFrame.Position = UDim2.new(0.5, -275, 0.5, -130)
Tween(MainFrame, {BackgroundTransparency = 0, Position = UDim2.new(0.5, -275, 0.5, -170)}, 0.7, Enum.EasingStyle.Quint)
