local TweenService = game:GetService("TweenService")

-- Estado inicial
main.Visible = false
main.Size = UDim2.new(0, 300, 0, 380)
main.BackgroundTransparency = 1

-- Guardar tamanho final
local finalSize = UDim2.new(0, 350, 0, 440)

-- Função abrir
local function openHub()
    main.Visible = true
    main.Size = UDim2.new(0, 300, 0, 380)
    main.BackgroundTransparency = 1

    TweenService:Create(
        main,
        TweenInfo.new(0.25, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
        {
            Size = finalSize,
            BackgroundTransparency = 0
        }
    ):Play()
end

-- Função fechar
local function closeHub()
    local tween = TweenService:Create(
        main,
        TweenInfo.new(0.2, Enum.EasingStyle.Quint, Enum.EasingDirection.In),
        {
            Size = UDim2.new(0, 300, 0, 380),
            BackgroundTransparency = 1
        }
    )
    tween:Play()
    tween.Completed:Wait()
    main.Visible = false
end

-- ================= BOTÃO FLUTUANTE =================
floatBtn.MouseButton1Click:Connect(function()
    if main.Visible then
        closeHub()
    else
        openHub()
    end
end)
