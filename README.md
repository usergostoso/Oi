local function executeScript(script)
    local func, err = loadstring(script)
    if not func then
        return "Erro ao compilar script: " .. err
    end
    local success, result = pcall(func)
    if success then
        return result or "Executado com sucesso."
    else
        return "Erro ao executar script: " .. result
    end
end

-- Criando UI simples (usando Instance.new para o Delta)
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local TextBox = Instance.new("TextBox")
local ExecuteButton = Instance.new("TextButton")
local OutputBox = Instance.new("TextLabel")

-- Configurando UI
ScreenGui.Parent = game.CoreGui

Frame.Parent = ScreenGui
Frame.Size = UDim2.new(0, 300, 0, 200)
Frame.Position = UDim2.new(0.5, -150, 0.5, -100)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

TextBox.Parent = Frame
TextBox.Size = UDim2.new(1, -10, 0.6, 0)
TextBox.Position = UDim2.new(0, 5, 0, 5)
TextBox.Text = "-- Digite seu script aqui"
TextBox.TextWrapped = true
TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
TextBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

ExecuteButton.Parent = Frame
ExecuteButton.Size = UDim2.new(1, -10, 0.2, 0)
ExecuteButton.Position = UDim2.new(0, 5, 0.65, 5)
ExecuteButton.Text = "Executar"
ExecuteButton.BackgroundColor3 = Color3.fromRGB(70, 130, 180)

OutputBox.Parent = Frame
OutputBox.Size = UDim2.new(1, -10, 0.2, 0)
OutputBox.Position = UDim2.new(0, 5, 0.85, 5)
OutputBox.Text = "Saída:"
OutputBox.TextWrapped = true
OutputBox.TextColor3 = Color3.fromRGB(255, 255, 255)
OutputBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

-- Evento de execução
ExecuteButton.MouseButton1Click:Connect(function()
    local result = executeScript(TextBox.Text)
    OutputBox.Text = result
end)
