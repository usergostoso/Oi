-- Fz4 Script

-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Criando GUI Principal
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = PlayerGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.DisplayOrder = 9999

-- Criando a Janela Principal
local Frame = Instance.new("Frame")
Frame.Parent = ScreenGui
Frame.Active = true
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.3, 0, 0.2, 0)
Frame.Size = UDim2.new(0, 500, 0, 450)

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0.05, 0)
UICorner.Parent = Frame

-- Criando Botão de Fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Parent = Frame
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.Position = UDim2.new(0.95, 0, 0.02, 0)
CloseButton.Size = UDim2.new(0, 20, 0, 20)
CloseButton.Font = Enum.Font.SourceSans
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.TextSize = 14

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(1, 0)
CloseCorner.Parent = CloseButton

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Criando Título
local Title = Instance.new("TextLabel")
Title.Parent = Frame
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0.05, 0, 0.05, 0)
Title.Size = UDim2.new(0, 200, 0, 30)
Title.Font = Enum.Font.GothamBold
Title.Text = "Fz4 Script"
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.TextSize = 22

-- Função de Arrastar Janela
local dragging, dragInput, dragStart, startPos

local function Update(input)
    local delta = input.Position - dragStart
    Frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

Frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Frame.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Frame.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        Update(input)
    end
end)

-- Criando Função de Animação de Carregamento
local function CreateLoadingAnimation(parent)
    local LoadingFrame = Instance.new("Frame")
    LoadingFrame.Size = UDim2.new(1, 0, 1, 0)
    LoadingFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    LoadingFrame.BackgroundTransparency = 0.5
    LoadingFrame.Parent = parent

    local LoadingLabel = Instance.new("TextLabel")
    LoadingLabel.Text = "Carregando..."
    LoadingLabel.Size = UDim2.new(1, 0, 0, 30)
    LoadingLabel.Position = UDim2.new(0, 0, 0.5, -15)
    LoadingLabel.BackgroundTransparency = 1
    LoadingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    LoadingLabel.Font = Enum.Font.GothamBold
    LoadingLabel.TextSize = 16
    LoadingLabel.Parent = LoadingFrame

    return LoadingFrame
end

-- Criando Botões de Execução de Scripts
local function CreateScriptButton(parent, name, scriptUrl, positionY)
    local ButtonFrame = Instance.new("Frame")
    ButtonFrame.Parent = parent
    ButtonFrame.BackgroundColor3 = Color3.fromRGB(29, 29, 29)
    ButtonFrame.Size = UDim2.new(0, 450, 0, 70)
    ButtonFrame.Position = UDim2.new(0.05, 0, positionY, 0)

    local UICorner = Instance.new("UICorner")
    UICorner.Parent = ButtonFrame

    local ScriptLabel = Instance.new("TextLabel")
    ScriptLabel.Parent = ButtonFrame
    ScriptLabel.BackgroundTransparency = 1
    ScriptLabel.Position = UDim2.new(0.05, 0, 0.25, 0)
    ScriptLabel.Size = UDim2.new(0, 250, 0, 30)
    ScriptLabel.Font = Enum.Font.Gotham
    ScriptLabel.Text = name
    ScriptLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    ScriptLabel.TextSize = 18
    ScriptLabel.TextXAlignment = Enum.TextXAlignment.Left

    local LoadButton = Instance.new("TextButton")
    LoadButton.Parent = ButtonFrame
    LoadButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    LoadButton.Position = UDim2.new(0.7, 0, 0.15, 0)
    LoadButton.Size = UDim2.new(0, 120, 0, 40)
    LoadButton.Font = Enum.Font.GothamBold
    LoadButton.Text = "Load"
    LoadButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    LoadButton.TextSize = 18

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.Parent = LoadButton

    LoadButton.MouseButton1Click:Connect(function()
        local LoadingAnimation = CreateLoadingAnimation(Frame)
        wait(2)
        LoadingAnimation:Destroy()
        loadstring(game:HttpGet(scriptUrl))()
    end)
end

-- Adicionando Scripts
CreateScriptButton(Frame, "Fz4 Arsenal", "https://raw.githubusercontent.com/blackowl1231/ZYPHERION/refs/heads/main/Games/ZYPHERION%20Arsenal%20Beta.lua", 0.2)
CreateScriptButton(Frame, "Fz4 Rivals", "https://raw.githubusercontent.com/blackowl1231/ZYPHERION/refs/heads/main/Games/ZYPHERION%20Rivals%20Beta.lua", 0.4)

-- Mensagem de Agradecimento
local FooterLabel = Instance.new("TextLabel")
FooterLabel.Parent = Frame
FooterLabel.BackgroundTransparency = 1
FooterLabel.Position = UDim2.new(0.25, 0, 0.85, 0)
FooterLabel.Size = UDim2.new(0, 250, 0, 30)
FooterLabel.Font = Enum.Font.Gotham
FooterLabel.Text = "Obrigado por usar Fz4 Script!"
FooterLabel.TextColor3 = Color3.fromRGB(217, 217, 217)
FooterLabel.TextSize = 14
FooterLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Execução Automática de Teste
wait(2)
loadstring(game:HttpGet("https://raw.githubusercontent.com/blackowl1231/ZYPHERION/refs/heads/main/Games/Test.lua"))()
