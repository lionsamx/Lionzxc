local range = 10000
local player = game:GetService("Players").LocalPlayer

local function isFriendWith(player1, player2)
    return player1:IsFriendsWith(player2.UserId)
end

-- Variable to control if the script is active or not
local isActive = false

-- Create the GUI
local gui = Instance.new("ScreenGui")
gui.Name = "ControlGUI"
gui.Parent = game.CoreGui

-- Create the "On/Off" button
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 100, 0, 50)
toggleButton.Position = UDim2.new(0.5, -50, 0.9, -30)
toggleButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)  -- yellow for "On"
toggleButton.Text = "GO"
toggleButton.Font = Enum.Font.SourceSansBold
toggleButton.TextSize = 20
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.Parent = gui

toggleButton.MouseButton1Click:Connect(function()
    isActive = not isActive
    -- Change button color and text depending on the state
    if isActive then
        toggleButton.BackgroundColor3 = Color3.fromRGB(0, 1000, 0)  -- yellow for "On"
        toggleButton.Text = "kill all"
    else
        toggleButton.BackgroundColor3 = Color3.fromRGB(1000, 0, 0)  -- white for "Off"
        toggleButton.Text = "Kill no"
    end
end)

game:GetService("RunService").RenderStepped:Connect(function()
    if isActive then
        local players = game.Players:GetPlayers()
        for i = 2, #players do
            local otherPlayer = players[i]
            local character = otherPlayer.Character
            if not isFriendWith(player, otherPlayer) then
                local tool = player.Character and player.Character:FindFirstChildOfClass("Tool")
                if tool and tool:FindFirstChild("Handle") then
                    tool:Activate()
                    for _, part in next, character:GetChildren() do
                        if part:IsA("BasePart") then
                            firetouchinterest(tool.Handle, part, 0)
                            firetouchinterest(tool.Handle, part, 1)
                        end
                    end
                end
            end
        end
    end
end)
