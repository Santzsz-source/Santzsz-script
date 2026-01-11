	-- Serviços
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

-- Player e GUI
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local gui = playerGui:WaitForChild("SeuGui")

-- Elementos
local main = gui:WaitForChild("Main")
local floatBtn = gui:WaitForChild("FloatBtn")
local blackBG = gui:WaitForChild("BlackBG")

-- ================= CONFIGURAÇÃO INICIAL =================
main.Visible = false
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.Size = UDim2.new(0.7, 0, 0.6, 0)
main.BackgroundTransparency = 1

blackBG.Visible = false
blackBG.Size = UDim2.new(1, 0, 1, 0)
blackBG.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
blackBG.BackgroundTransparency = 1
blackBG.ZIndex = 1
main.ZIndex = 2

-- Tamanhos
local closedSize = UDim2.new(0.7, 0, 0.6, 0)
local openSize = UDim2.new(0.85, 0, 0.75, 0)

local isAnimating = false

-- ================= ABRIR HUB =================
local function openHub()
	if isAnimating then return end
	isAnimating = true

	main.Visible = true
	blackBG.Visible = true

	main.Size = closedSize
	main.BackgroundTransparency = 1
	blackBG.BackgroundTransparency = 1

	-- Fundo escuro
	TweenService:Create(
		blackBG,
		TweenInfo.new(0.25, Enum.EasingStyle.Sine, Enum.EasingDirection.Out),
		{ BackgroundTransparency = 0.4 }
	):Play()

	-- Hub
	local tween = TweenService:Create(
		main,
		TweenInfo.new(0.25, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
		{
			Size = openSize,
			BackgroundTransparency = 0
		}
	)

	tween:Play()
	tween.Completed:Wait()
	isAnimating = false
end

-- ================= FECHAR HUB =================
local function closeHub()
	if isAnimating then return end
	isAnimating = true

	-- Fundo
	TweenService:Create(
		blackBG,
		TweenInfo.new(0.2, Enum.EasingStyle.Sine, Enum.EasingDirection.In),
		{ BackgroundTransparency = 1 }
	):Play()

	-- Hub
	local tween = TweenService:Create(
		main,
		TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.In),
		{
			Size = closedSize,
			BackgroundTransparency = 1
		}
	)

	tween:Play()
	tween.Completed:Wait()

	main.Visible = false
	blackBG.Visible = false
	isAnimating = false
end

-- ================= BOTÃO =================
floatBtn.Size = UDim2.new(0, 60, 0, 60)

floatBtn.MouseButton1Click:Connect(function()
	if main.Visible then
		closeHub()
	else
		openHub()
	end
end)

-- Fecha ao tocar no fundo
blackBG.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		if main.Visible then
			closeHub()
		end
	end
end)

print("Hub com fundo preto carregado")
