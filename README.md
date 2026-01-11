local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Altere o nome se o seu ScreenGui for diferente
local gui = playerGui:WaitForChild("SeuGui")

-- Elementos da interface
local main = gui:WaitForChild("Main")
local floatBtn = gui:WaitForChild("FloatBtn")

-- ================= ESTADO INICIAL =================
main.Visible = false
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.Size = UDim2.new(0.7, 0, 0.6, 0)
main.BackgroundTransparency = 1

-- Tamanho final (mobile)
local closedSize = UDim2.new(0.7, 0, 0.6, 0)
local openSize = UDim2.new(0.85, 0, 0.75, 0)

local isAnimating = false

-- ================= ABRIR HUB =================
local function openHub()
	if isAnimating then return end
	isAnimating = true

	main.Visible = true
	main.Size = closedSize
	main.BackgroundTransparency = 1

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
	isAnimating = false
end

-- ================= BOTÃO FLUTUANTE =================
floatBtn.AutoButtonColor = true
floatBtn.Size = UDim2.new(0, 60, 0, 60)

floatBtn.MouseButton1Click:Connect(function()
	if main.Visible then
		closeHub()
	else
		openHub()
	end
end)

print("Hub carregado com sucesso")
