local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Crear el ScreenGui principal de nuestro cartel
local gui = Instance.new("ScreenGui")
gui.Name = "TradeVisualSimulator"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = playerGui

-- Panel principal ("Objeto Desconocido")
local panel = Instance.new("Frame")
panel.Size = UDim2.fromOffset(270, 190)
panel.BackgroundColor3 = Color3.fromRGB(20, 29, 38)
panel.BorderSizePixel = 0
panel.Visible = false
panel.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 9)
corner.Parent = panel

-- Encabezado
local header = Instance.new("TextLabel")
header.Size = UDim2.new(1, 0, 0, 35)
header.BackgroundColor3 = Color3.fromRGB(55, 75, 88)
header.Text = "Objeto Desconocido"
header.TextColor3 = Color3.new(1, 1, 1)
header.TextSize = 17
header.Font = Enum.Font.GothamBold
header.Parent = panel

-- Información
local info = Instance.new("TextLabel")
info.Position = UDim2.fromOffset(5, 43)
info.Size = UDim2.new(1, -10, 0, 70)
info.BackgroundTransparency = 1
info.Text = "Sin Verificar\n\nValor: ???"
info.TextColor3 = Color3.fromRGB(255, 220, 40)
info.TextSize = 17
info.Font = Enum.Font.GothamBold
info.Parent = panel

-- Mensaje
local description = Instance.new("TextLabel")
description.Position = UDim2.fromOffset(10, 112)
description.Size = UDim2.new(1, -20, 0, 42)
description.BackgroundTransparency = 1
description.TextWrapped = true
description.Text = "La vista previa del objeto no está disponible debido a modificaciones visuales activas."
description.TextColor3 = Color3.fromRGB(220, 220, 220)
description.TextSize = 10
description.Parent = panel

-- Aviso
local warning = Instance.new("TextLabel")
warning.Position = UDim2.fromOffset(5, 143)
warning.Size = UDim2.new(1, -10, 0, 38)
warning.BackgroundColor3 = Color3.fromRGB(150, 45, 45)
warning.Text = "SIMULACIÓN: Si experimentas errores visuales en la oferta, visita la Plaza de intercambio para continuar con seguridad."
warning.TextColor3 = Color3.new(1, 1, 1)
warning.TextSize = 9
warning.TextWrapped = true
warning.Font = Enum.Font.GothamBold
warning.Parent = panel

local warningCorner = Instance.new("UICorner")
warningCorner.CornerRadius = UDim.new(0, 5)
warningCorner.Parent = warning

local menuActivo = false

-- Tecla H para abrir/cerrar el menú de forma segura
UserInputService.InputBegan:Connect(function(input, processed)
	if processed then return end

	if input.KeyCode == Enum.KeyCode.H then
		menuActivo = not menuActivo
		panel.Visible = menuActivo
	end
end)

-- Seguir al mouse ordenadamente arriba/al lado del cursor
RunService.RenderStepped:Connect(function()
	if not menuActivo then return end

	local mousePosition = UserInputService:GetMouseLocation()

	local x = math.clamp(mousePosition.X + 15, 0, workspace.CurrentCamera.ViewportSize.X - 270)
	local y = math.clamp(mousePosition.Y - 120, 0, workspace.CurrentCamera.ViewportSize.Y - 190)

	panel.Position = UDim2.fromOffset(x, y)
end)
