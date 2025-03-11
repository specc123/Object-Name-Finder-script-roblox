local player = game.Players.LocalPlayer

-- Create HUD
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 350, 0, 400)
frame.Position = UDim2.new(0.05, 0, 0.1, 0)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 2
frame.Parent = screenGui
frame.Active = true
frame.Draggable = true -- Movable HUD

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0.1, 0)
title.BackgroundTransparency = 1
title.Text = "Object & NPC Detector"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextScaled = true
title.Parent = frame

local searchBox = Instance.new("TextBox")
searchBox.Size = UDim2.new(1, -10, 0.1, 0)
searchBox.Position = UDim2.new(0, 5, 0.1, 0)
searchBox.PlaceholderText = "Search objects..."
searchBox.TextColor3 = Color3.fromRGB(255, 255, 255)
searchBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
searchBox.Parent = frame

local scrollFrame = Instance.new("ScrollingFrame")
scrollFrame.Size = UDim2.new(1, -10, 0.65, 0)
scrollFrame.Position = UDim2.new(0, 5, 0.22, 0)
scrollFrame.CanvasSize = UDim2.new(0, 0, 5, 0)
scrollFrame.ScrollBarThickness = 5
scrollFrame.Parent = frame

local refreshButton = Instance.new("TextButton")
refreshButton.Size = UDim2.new(1, -10, 0.1, 0)
refreshButton.Position = UDim2.new(0, 5, 0.9, 0)
refreshButton.Text = "Refresh List"
refreshButton.TextColor3 = Color3.fromRGB(255, 255, 255)
refreshButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
refreshButton.Font = Enum.Font.SourceSansBold
refreshButton.Parent = frame

local objectList = {}

-- Function to classify object type
local function getObjectType(obj)
    if obj:IsA("Model") then
        if obj:FindFirstChildOfClass("Humanoid") then
            return "NPC"
        else
            return "Object"
        end
    elseif obj:IsA("BasePart") then
        return "Physical Object"
    elseif obj:IsA("Tool") then
        return "Collectible"
    elseif obj:IsA("ProximityPrompt") then
        return "Interactable"
    elseif obj:IsA("RemoteEvent") then
        return "Scripted Event"
    else
        return "Unknown"
    end
end

-- Function to update UI
local function updateUI()
    local searchText = searchBox.Text:lower()
    local yOffset = 0

    for name, data in pairs(objectList) do
        local label = data.label
        if searchText == "" or name:lower():find(searchText) then
            label.Visible = true
            label.Position = UDim2.new(0, 0, 0, yOffset)
            yOffset = yOffset + 25
        else
            label.Visible = false
        end
    end

    scrollFrame.CanvasSize = UDim2.new(0, 0, 0, yOffset)
end

-- Function to scan objects only when button is pressed
local function scanObjects()
    -- Clear old labels
    for _, data in pairs(objectList) do
        if data.label then data.label:Destroy() end
    end
    objectList = {}

    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("Model") or obj:IsA("BasePart") or obj:IsA("Tool") or obj:IsA("ProximityPrompt") then
            local objType = getObjectType(obj)
            local objName = obj.Name .. " (" .. objType .. ")"

            -- Create label
            local objLabel = Instance.new("TextLabel")
            objLabel.Size = UDim2.new(1, 0, 0, 25)
            objLabel.Text = objName
            objLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            objLabel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
            objLabel.Parent = scrollFrame

            objectList[objName] = {instance = obj, label = objLabel}
        end
    end

    updateUI()
end

-- Connect button click event
refreshButton.MouseButton1Click:Connect(scanObjects)

-- Search box event
searchBox.Changed:Connect(updateUI)
