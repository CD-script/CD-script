local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local ScriptBox = Instance.new("TextBox")
local ExecuteButton = Instance.new("TextButton")
local ClearButton = Instance.new("TextButton")
local LoadButton = Instance.new("TextButton")
local TitleBar = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local MinimizeButton = Instance.new("TextButton")
local RestoreButton = Instance.new("TextButton")
local UIS = game:GetService("UserInputService") -- Serviço para lidar com drag

-- Configuração do ScreenGui
ScreenGui.Name = "SekoExecutor"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Configuração do MainFrame
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
MainFrame.Position = UDim2.new(0.5, -250, 0.5, -200)
MainFrame.Size = UDim2.new(0, 500, 0, 400)

-- Configuração do TitleBar
TitleBar.Name = "TitleBar"
TitleBar.Parent = MainFrame
TitleBar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TitleBar.Size = UDim2.new(1, 0, 0.1, 0)

-- Configuração do TitleLabel
TitleLabel.Name = "TitleLabel"
TitleLabel.Parent = TitleBar
TitleLabel.BackgroundTransparency = 1
TitleLabel.Size = UDim2.new(0.9, 0, 1, 0)
TitleLabel.Font = Enum.Font.SourceSansBold
TitleLabel.Text = "Seko Executor"
TitleLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
TitleLabel.TextScaled = true

-- Botão para minimizar
MinimizeButton.Name = "MinimizeButton"
MinimizeButton.Parent = TitleBar
MinimizeButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
MinimizeButton.Position = UDim2.new(0.9, 0, 0, 0)
MinimizeButton.Size = UDim2.new(0.1, 0, 1, 0)
MinimizeButton.Font = Enum.Font.SourceSansBold
MinimizeButton.Text = "-"
MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)

-- Botão de restaurar
RestoreButton.Name = "RestoreButton"
RestoreButton.Parent = ScreenGui
RestoreButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
RestoreButton.Position = UDim2.new(0.5, -50, 0.5, -25)
RestoreButton.Size = UDim2.new(0, 100, 0, 50)
RestoreButton.Font = Enum.Font.SourceSansBold
RestoreButton.Text = "Abrir"
RestoreButton.TextColor3 = Color3.fromRGB(0, 0, 0)
RestoreButton.Visible = false

-- Configuração do ScriptBox
ScriptBox.Name = "ScriptBox"
ScriptBox.Parent = MainFrame
ScriptBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
ScriptBox.Position = UDim2.new(0.05, 0, 0.15, 0)
ScriptBox.Size = UDim2.new(0.9, 0, 0.6, 0)
ScriptBox.Font = Enum.Font.Code
ScriptBox.PlaceholderText = "Digite ou cole seu script aqui..."
ScriptBox.Text = ""
ScriptBox.TextColor3 = Color3.fromRGB(0, 255, 0)
ScriptBox.TextSize = 14

-- Configuração dos Botões
local function configureButton(button, name, position, color, text)
    button.Name = name
    button.Parent = MainFrame
    button.BackgroundColor3 = color
    button.Position = position
    button.Size = UDim2.new(0.27, 0, 0.1, 0)
    button.Font = Enum.Font.SourceSansBold
    button.Text = text
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.TextSize = 18
end

configureButton(ExecuteButton, "ExecuteButton", UDim2.new(0.05, 0, 0.8, 0), Color3.fromRGB(0, 100, 0), "Executar")
configureButton(ClearButton, "ClearButton", UDim2.new(0.37, 0, 0.8, 0), Color3.fromRGB(100, 0, 0), "Limpar")
configureButton(LoadButton, "LoadButton", UDim2.new(0.69, 0, 0.8, 0), Color3.fromRGB(0, 0, 100), "Carregar")

-- Minimizar e Restaurar
MinimizeButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    RestoreButton.Visible = true
end)

RestoreButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    RestoreButton.Visible = false
end)

-- Drag and Drop
local dragging = false
local dragStart, startPos

TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = MainFrame.Position
    end
end)

UIS.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = UIS:GetMouseLocation() - dragStart.Position
        MainFrame.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

TitleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
