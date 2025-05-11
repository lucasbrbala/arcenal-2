-- Aimbot Arsenal | Created By ttk
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- Variáveis
local fov = 100
local teamCheck = true
local aimbotEnabled = false
local isAiming = false
local showMenu = true
local discordLink = "https://discord.gg/seu-link-aqui"

-- Criar GUI
local gui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
gui.Name = "AimbotGUI"
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 220, 0, 300)
frame.Position = UDim2.new(0.02, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "Aimbot Arsenal"
title.BackgroundTransparency = 1
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.TextSize = 20

local y = 40
local function criarBotao(txt, callback)
	local btn = Instance.new("TextButton", frame)
	btn.Size = UDim2.new(1, -10, 0, 30)
	btn.Position = UDim2.new(0, 5, 0, y)
	btn.Text = txt
	btn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
	btn.TextColor3 = Color3.new(1,1,1)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 16
	btn.MouseButton1Click:Connect(function()
		callback(btn)
	end)
	y = y + 35
	return btn
end

-- Botão Aimbot
local aimbotBtn = criarBotao("Aimbot: OFF", function(btn)
	aimbotEnabled = not aimbotEnabled
	btn.Text = "Aimbot: " .. (aimbotEnabled and "ON" or "OFF")
end)

-- Aumentar/Diminuir FOV
criarBotao("Aumentar FOV", function()
	fov = fov + 10
end)

criarBotao("Diminuir FOV", function()
	fov = math.max(10, fov - 10)
end)

-- Team Check
local teamBtn = criarBotao("Team Check: ON", function(btn)
	teamCheck = not teamCheck
	btn.Text = "Team Check: " .. (teamCheck and "ON" or "OFF")
end)

-- Botão Discord
criarBotao("Discord", function()
	setclipboard(discordLink)
end)

-- Créditos
local credit = Instance.new("TextLabel", frame)
credit.Size = UDim2.new(1, 0, 0, 30)
credit.Position = UDim2.new(0, 0, 1, -30)
credit.Text = "Created By ttk"
credit.BackgroundTransparency = 1
credit.TextColor3 = Color3.new(1,1,1)
credit.Font = Enum.Font.Gotham
credit.TextSize = 14

-- Toggle menu com tecla P
UserInputService.InputBegan:Connect(function(input)
	if input.KeyCode == Enum.KeyCode.P then
		showMenu = not showMenu
		frame.Visible = showMenu
	end
end)

-- FOV Círculo
local circle = Drawing.new("Circle")
circle.Color = Color3.fromRGB(255, 255, 255)
circle.Thickness = 1
circle.Filled = false
circle.Visible = true

RunService.RenderStepped:Connect(function()
	local mouse = UserInput
