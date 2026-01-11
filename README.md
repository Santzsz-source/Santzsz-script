local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")

-- Estado inicial
main.Visible = false
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.Size = UDim2.new(0.7, 0, 0.6, 0)
main.BackgroundTransparency = 1

-- Tamanho final (mobile-friendly)
local finalSize = UDim2.new(0.85, 0, 0.75, 0)

local isAnimating = false

-- Abrir hub
local function openHub()
	if isAnimating then return end
	isAnimating = true

	main.Visible = true
	main.Size = UDim2.new(0.7, 0, 0.6, 0)
	main.BackgroundTransparency = 1

	local tween = TweenService:Create(
		main,
		TweenInfo.new(0.25, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
		{
			Size = finalSize,
			BackgroundTransparency = 0
		}
	)

	tween:Play()
	tween.Completed:Wait()
	isAnimating = false
end

-- Fechar hub
local function closeHub()
	if isAnimating then return end
	isAnimating = true

	local tween = TweenService:Create(
		main,
		TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.In),
		{
			Size = UDim2.new(0.7, 0, 0.6, 0),
			BackgroundTransparency = 1
		}
	)

	tween:Play()
	tween.Completed:Wait()
	main.Visible = false
	isAnimating = false
end

-- Botão flutuante (touch-friendly)
floatBtn.AutoButtonColor = true
floatBtn.Size = UDim2.new(0, 60, 0, 60)

floatBtn.MouseButton1Click:Connect(function()
	if main.Visible then
		closeHub()
	else
		openHub()
	end
end)
