-- Create the ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CoordinateFinderGUI"
screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Create the main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0.3, 0, 0.2, 0)
mainFrame.Position = UDim2.new(0.35, 0, 0.4, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui
mainFrame.Draggable = true
mainFrame.Active = true

-- Create a UI Corner for rounded edges
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 10)
uiCorner.Parent = mainFrame

-- Create the title label
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, 0, 0.3, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "Coordinate Finder"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextScaled = true
titleLabel.Parent = mainFrame

-- Create the coordinates label
local coordLabel = Instance.new("TextLabel")
coordLabel.Name = "CoordLabel"
coordLabel.Size = UDim2.new(1, 0, 0.4, 0)
coordLabel.Position = UDim2.new(0, 0, 0.3, 0)
coordLabel.BackgroundTransparency = 1
coordLabel.Text = "X: 0, Y: 0, Z: 0"
coordLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
coordLabel.Font = Enum.Font.Gotham
coordLabel.TextScaled = true
coordLabel.Parent = mainFrame

-- Create the copy button
local copyButton = Instance.new("TextButton")
copyButton.Name = "CopyButton"
copyButton.Size = UDim2.new(0.5, 0, 0.3, 0)
copyButton.Position = UDim2.new(0.25, 0, 0.7, 0)
copyButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
copyButton.Text = "Copy Coordinates"
copyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
copyButton.Font = Enum.Font.Gotham
copyButton.TextScaled = true
copyButton.Parent = mainFrame

-- Add rounded corners to the button
local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 8)
buttonCorner.Parent = copyButton

-- Function to update the coordinates label
local function updateCoordinates()
    local player = game.Players.LocalPlayer
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local position = player.Character.HumanoidRootPart.Position
        coordLabel.Text = string.format("X: %.2f, Y: %.2f, Z: %.2f", position.X, position.Y, position.Z)
    end
end

-- Connect the update function to the RenderStepped event
game:GetService("RunService").RenderStepped:Connect(updateCoordinates)

-- Function to copy coordinates to clipboard
local function copyToClipboard()
    local text = coordLabel.Text
    if setclipboard then
        setclipboard(text)
        copyButton.Text = "Copied!"
        wait(1)
        copyButton.Text = "Copy Coordinates"
    else
        warn("Clipboard functionality is not available.")
    end
end

-- Connect the copy function to the button click event
copyButton.MouseButton1Click:Connect(copyToClipboard)
