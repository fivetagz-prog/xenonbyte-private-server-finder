local TweenService = game:GetService("TweenService")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")
local UserInputService = game:GetService("UserInputService")
local Stats = game:GetService("Stats")
local Players = game:GetService("Players")

local LocalPlayer = Players.LocalPlayer
local PlaceId = game.PlaceId

local autoHopEnabled = false
local foundServers = {}

local httpReq = (syn and syn.request) or (http and http.request) or http_request or request

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FreePrivateServerGui"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 360, 0, 320)
MainFrame.Position = UDim2.new(0.5, -180, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = false
MainFrame.Parent = ScreenGui

local MainUICorner = Instance.new("UICorner")
MainUICorner.CornerRadius = UDim.new(0, 20)
MainUICorner.Parent = MainFrame

local UIGradient = Instance.new("UIGradient")
UIGradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 20, 20)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(225, 225, 225))
})
UIGradient.Rotation = 45
UIGradient.Parent = MainFrame

local MainUIStroke = Instance.new("UIStroke")
MainUIStroke.Thickness = 1.5
MainUIStroke.Color = Color3.fromRGB(255, 255, 255)
MainUIStroke.Transparency = 0.6
MainUIStroke.Parent = MainFrame

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Size = UDim2.new(1, -120, 0, 40)
TitleLabel.Position = UDim2.new(0, 15, 0, 5)
TitleLabel.BackgroundTransparency = 1
TitleLabel.Text = "XenonByte — Solo Finder"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.TextSize = 13
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextXAlignment = Enum.TextXAlignment.Left
TitleLabel.Parent = MainFrame

local function showNotification(text)
	local notifFrame = Instance.new("Frame")
	notifFrame.Size = UDim2.new(0, 220, 0, 32)
	notifFrame.Position = UDim2.new(0.5, -110, 0, -45)
	notifFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
	notifFrame.BorderSizePixel = 0
	notifFrame.BackgroundTransparency = 1
	notifFrame.Parent = MainFrame

	local notifCorner = Instance.new("UICorner")
	notifCorner.CornerRadius = UDim.new(0, 16)
	notifCorner.Parent = notifFrame

	local notifGrad = Instance.new("UIGradient")
	notifGrad.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, Color3.fromRGB(15, 15, 15)),
		ColorSequenceKeypoint.new(1, Color3.fromRGB(200, 200, 200))
	})
	notifGrad.Rotation = 90
	notifGrad.Parent = notifFrame

	local notifStroke = Instance.new("UIStroke")
	notifStroke.Thickness = 1
	notifStroke.Color = Color3.fromRGB(255, 255, 255)
	notifStroke.Transparency = 0.5
	notifStroke.Parent = notifFrame

	local notifText = Instance.new("TextLabel")
	notifText.Size = UDim2.new(1, 0, 1, 0)
	notifText.BackgroundTransparency = 1
	notifText.Text = text
	notifText.TextColor3 = Color3.fromRGB(255, 255, 255)
	notifText.TextSize = 11
	notifText.Font = Enum.Font.GothamBold
	notifText.Parent = notifFrame

	TweenService:Create(notifFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {BackgroundTransparency = 0, Position = UDim2.new(0.5, -110, 0, -40)}):Play()

	task.delay(2.5, function()
		local fade = TweenService:Create(notifFrame, TweenInfo.new(0.3, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {BackgroundTransparency = 1, Position = UDim2.new(0.5, -110, 0, -50)})
		fade:Play()
		fade.Completed:Connect(function()
			notifFrame:Destroy()
		end)
	end)
end

local ControlContainer = Instance.new("Frame")
ControlContainer.Size = UDim2.new(0, 100, 0, 30)
ControlContainer.Position = UDim2.new(1, -110, 0, 10)
ControlContainer.BackgroundTransparency = 1
ControlContainer.Parent = MainFrame

local DiscordBtn = Instance.new("ImageButton")
DiscordBtn.Size = UDim2.new(0, 28, 0, 28)
DiscordBtn.Position = UDim2.new(0, 0, 0, 1)
DiscordBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
DiscordBtn.BackgroundTransparency = 0.5
DiscordBtn.Image = "rbxassetid://106435404864205"
DiscordBtn.ZIndex = 1
DiscordBtn.Parent = ControlContainer

local DiscordCorner = Instance.new("UICorner")
DiscordCorner.CornerRadius = UDim.new(1, 0)
DiscordCorner.Parent = DiscordBtn

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 28, 0, 28)
MinimizeBtn.Position = UDim2.new(0, 34, 0, 1)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MinimizeBtn.BackgroundTransparency = 0.5
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextSize = 16
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.ZIndex = 2
MinimizeBtn.Parent = ControlContainer

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(1, 0)
MinCorner.Parent = MinimizeBtn

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(0, 68, 0, 1)
CloseBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
CloseBtn.BackgroundTransparency = 0.5
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 12
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.ZIndex = 2
CloseBtn.Parent = ControlContainer

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(1, 0)
CloseCorner.Parent = CloseBtn

DiscordBtn.MouseButton1Click:Connect(function()
	if setclipboard then
		setclipboard("https://discord.gg/tFgmr4H5Qn")
	end
	showNotification("discord server copied!")
end)

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -30, 0, 25)
StatusLabel.Position = UDim2.new(0, 15, 0, 40)
StatusLabel.BackgroundTransparency = 1
StatusLabel.Text = "Status: Idle"
StatusLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
StatusLabel.TextSize = 13
StatusLabel.Font = Enum.Font.Gotham
StatusLabel.Parent = MainFrame

local PingLabel = Instance.new("TextLabel")
PingLabel.Size = UDim2.new(1, -30, 0, 20)
PingLabel.Position = UDim2.new(0, 15, 0, 65)
PingLabel.BackgroundTransparency = 1
PingLabel.Text = "Players: " .. #Players:GetPlayers() .. " | Ping: Fetching..."
PingLabel.TextColor3 = Color3.fromRGB(180, 255, 180)
PingLabel.TextSize = 12
PingLabel.Font = Enum.Font.GothamMedium
PingLabel.Parent = MainFrame

task.spawn(function()
	while task.wait(1) do
		if PingLabel and PingLabel.Parent then
			local pingValue = 0
			pcall(function()
				pingValue = math.floor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue())
			end)
			PingLabel.Text = "Players: " .. #Players:GetPlayers() .. " | Ping: " .. pingValue .. " ms"
		else
			break
		end
	end
end)

local AutoHopBtn = Instance.new("TextButton")
AutoHopBtn.Size = UDim2.new(0, 155, 0, 35)
AutoHopBtn.Position = UDim2.new(0, 15, 0, 95)
AutoHopBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
AutoHopBtn.Text = "AutoHop: Off"
AutoHopBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
AutoHopBtn.TextSize = 12
AutoHopBtn.Font = Enum.Font.GothamBold
AutoHopBtn.Parent = MainFrame

local AutoHopCorner = Instance.new("UICorner")
AutoHopCorner.CornerRadius = UDim.new(0, 8)
AutoHopCorner.Parent = AutoHopBtn

local RefreshBtn = Instance.new("TextButton")
RefreshBtn.Size = UDim2.new(0, 155, 0, 35)
RefreshBtn.Position = UDim2.new(1, -170, 0, 95)
RefreshBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
RefreshBtn.Text = "Refresh Servers"
RefreshBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
RefreshBtn.TextSize = 12
RefreshBtn.Font = Enum.Font.GothamBold
RefreshBtn.Parent = MainFrame

local RefreshCorner = Instance.new("UICorner")
RefreshCorner.CornerRadius = UDim.new(0, 8)
RefreshCorner.Parent = RefreshBtn

local ServerListFrame = Instance.new("ScrollingFrame")
ServerListFrame.Name = "ServerListFrame"
ServerListFrame.Size = UDim2.new(1, -30, 0, 165)
ServerListFrame.Position = UDim2.new(0, 15, 0, 140)
ServerListFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
ServerListFrame.BackgroundTransparency = 0.6
ServerListFrame.BorderSizePixel = 0
ServerListFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ServerListFrame.ScrollBarThickness = 4
ServerListFrame.Parent = MainFrame

local ListCorner = Instance.new("UICorner")
ListCorner.CornerRadius = UDim.new(0, 10)
ListCorner.Parent = ServerListFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Parent = ServerListFrame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)

local UIPadding = Instance.new("UIPadding")
UIPadding.PaddingTop = UDim.new(0, 6)
UIPadding.PaddingLeft = UDim.new(0, 6)
UIPadding.PaddingRight = UDim.new(0, 6)
UIPadding.Parent = ServerListFrame

UIListLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	ServerListFrame.CanvasSize = UDim2.new(0, 0, 0, UIListLayout.AbsoluteContentSize.Y + 12)
end)

local dragging, dragInput, dragStart, startPos

local function updateDrag(input)
	local delta = input.Position - dragStart
	local targetPos = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	TweenService:Create(MainFrame, TweenInfo.new(0.12, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {Position = targetPos}):Play()
end

MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

MainFrame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if input == dragInput and dragging then
		updateDrag(input)
	end
end)

local minimized = false
MinimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	local targetSize = minimized and UDim2.new(0, 360, 0, 45) or UDim2.new(0, 360, 0, 320)
	
	TweenService:Create(MainFrame, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.InOut), {Size = targetSize}):Play()
	StatusLabel.Visible = not minimized
	PingLabel.Visible = not minimized
	AutoHopBtn.Visible = not minimized
	RefreshBtn.Visible = not minimized
	ServerListFrame.Visible = not minimized
	MinimizeBtn.Text = minimized and "+" or "-"
end)

CloseBtn.MouseButton1Click:Connect(function()
	ScreenGui:Destroy()
end)

local function scanLowestServers()
	foundServers = {}
	local cursor = ""

	if not httpReq then
		StatusLabel.Text = "Executor does not support HTTP requests"
		StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
		return {}
	end

	for page = 1, 20 do
		local url = "https://games.roblox.com/v1/games/" .. PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"
		if cursor ~= "" then
			url = url .. "&cursor=" .. cursor
		end

		local response = httpReq({
			Url = url,
			Method = "GET"
		})

		if response and response.Body then
			local success, data = pcall(function()
				return HttpService:JSONDecode(response.Body)
			end)

			if success and data and data.data then
				for _, server in ipairs(data.data) do
					if server.id ~= game.JobId and server.playing == 1 then
						table.insert(foundServers, server)
					end
				end

				if #foundServers >= 20 or not data.nextPageCursor then
					break
				end
				cursor = data.nextPageCursor
			else
				break
			end
		else
			break
		end
	end

	return foundServers
end

local function refreshUI()
	for _, child in ipairs(ServerListFrame:GetChildren()) do
		if child:IsA("Frame") then
			child:Destroy()
		end
	end

	StatusLabel.Text = "Scanning 1-player servers..."
	StatusLabel.TextColor3 = Color3.fromRGB(255, 200, 100)

	task.spawn(function()
		local servers = scanLowestServers()

		if #servers > 0 then
			StatusLabel.Text = "Found " .. #servers .. " 1-Player Server(s)"
			StatusLabel.TextColor3 = Color3.fromRGB(180, 255, 180)

			for i, serverData in ipairs(servers) do
				local itemFrame = Instance.new("Frame")
				itemFrame.Size = UDim2.new(1, 0, 0, 32)
				itemFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
				itemFrame.BorderSizePixel = 0
				itemFrame.Parent = ServerListFrame

				local itemCorner = Instance.new("UICorner")
				itemCorner.CornerRadius = UDim.new(0, 6)
				itemCorner.Parent = itemFrame

				local serverPingText = (serverData.ping and serverData.ping > 0) and (serverData.ping .. " ms") or "N/A"

				local infoLabel = Instance.new("TextLabel")
				infoLabel.Size = UDim2.new(1, -75, 1, 0)
				infoLabel.Position = UDim2.new(0, 8, 0, 0)
				infoLabel.BackgroundTransparency = 1
				infoLabel.Text = "#" .. i .. " | " .. serverData.playing .. "/" .. serverData.maxPlayers .. " P | Ping: " .. serverPingText
				infoLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
				infoLabel.TextSize = 11
				infoLabel.Font = Enum.Font.GothamMedium
				infoLabel.TextXAlignment = Enum.TextXAlignment.Left
				infoLabel.Parent = itemFrame

				local joinBtn = Instance.new("TextButton")
				joinBtn.Size = UDim2.new(0, 60, 0, 22)
				joinBtn.Position = UDim2.new(1, -66, 0.5, -11)
				joinBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
				joinBtn.Text = "Join"
				joinBtn.TextColor3 = Color3.fromRGB(0, 0, 0)
				joinBtn.TextSize = 11
				joinBtn.Font = Enum.Font.GothamBold
				joinBtn.Parent = itemFrame

				local joinCorner = Instance.new("UICorner")
				joinCorner.CornerRadius = UDim.new(0, 4)
				joinCorner.Parent = joinBtn

				joinBtn.MouseButton1Click:Connect(function()
					StatusLabel.Text = "Joining Server #" .. i .. "..."
					local tpSuccess, err = pcall(function()
						TeleportService:TeleportToPlaceInstance(PlaceId, serverData.id, LocalPlayer)
					end)
					if not tpSuccess then
						StatusLabel.Text = "Server full! Refreshing..."
						StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
						task.wait(1)
						refreshUI()
					end
				end)
			end
		else
			if StatusLabel.Text ~= "Executor does not support HTTP requests" then
				StatusLabel.Text = "No 1-Player Servers Found"
				StatusLabel.TextColor3 = Color3.fromRGB(255, 150, 150)
			end
		end
	end)
end

TeleportService.TeleportInitFailed:Connect(function(player, teleportResult, errorMessage)
	if player == LocalPlayer then
		StatusLabel.Text = "Teleport failed: Server closed."
		StatusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
		task.wait(1.5)
		refreshUI()
	end
end)

AutoHopBtn.MouseButton1Click:Connect(function()
	autoHopEnabled = not autoHopEnabled
	if autoHopEnabled then
		AutoHopBtn.Text = "AutoHop: On"
		AutoHopBtn.BackgroundColor3 = Color3.fromRGB(0, 180, 80)
		refreshUI()
		task.delay(1.5, function()
			if #foundServers > 0 then
				TeleportService:TeleportToPlaceInstance(PlaceId, foundServers[1].id, LocalPlayer)
			end
		end)
	else
		AutoHopBtn.Text = "AutoHop: Off"
		AutoHopBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
		StatusLabel.Text = "Status: Idle"
		StatusLabel.TextColor3 = Color3.fromRGB(220, 220, 220)
	end
end)

RefreshBtn.MouseButton1Click:Connect(function()
	refreshUI()
end)

refreshUI()
