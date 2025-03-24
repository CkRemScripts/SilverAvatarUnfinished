local Players = game:GetService("Players")

function morphToUser(userId)
    local player = Players.LocalPlayer
    local character = player.Character
    if not character then return end
    
    local appearance = Players:GetCharacterAppearanceAsync(userId)
    if not appearance then return end
    
    local toRemove = {
        ["Accessory"] = true,
        ["Shirt"] = true,
        ["Pants"] = true,
        ["CharacterMesh"] = true,
        ["BodyColors"] = true
    }
    
    for _, item in ipairs(character:GetChildren()) do
        if toRemove[item.ClassName] then
            item:Destroy()
        end
    end
    
    local face = character.Head:FindFirstChild("face")
    if face then face:Destroy() end
    
    for _, item in ipairs(appearance:GetChildren()) do
        if item:IsA("Shirt") or item:IsA("Pants") or item:IsA("BodyColors") then
            item.Parent = character
        elseif item:IsA("Accessory") then
            character.Humanoid:AddAccessory(item)
        elseif item.Name == character.Humanoid.RigType.Name then
            local mesh = item:FindFirstChildOfClass("CharacterMesh")
            if mesh then mesh.Parent = character end
        end
    end
    
    local newFace = appearance:FindFirstChild("face")
    if newFace then
        newFace.Parent = character.Head
    else
        local defaultFace = Instance.new("Decal")
        defaultFace.Name = "face"
        defaultFace.Face = "Front"
        defaultFace.Texture = "rbxasset://textures/face.png"
        defaultFace.Parent = character.Head
    end
    
    character.Parent = nil
    character.Parent = workspace
end

morphToUser(3461389772)

-- toji
local player = game.Players.LocalPlayer
local char = player.Character
local Humanoid = char.Humanoid
local hot = player.PlayerGui:WaitForChild("Hotbar")
local hotbar = hot:WaitForChild("Backpack"):WaitForChild("Hotbar")

local function cloneToolName(slot, text)
    local toolName = slot.ToolName
    if not slot:FindFirstChild("SkibidiGame") then
        toolName.Visible = false
        local clone = toolName:Clone()
        clone.Name = "SkibidiGame"
        clone.Parent = slot
        clone.Text = text
        clone.Visible = true
    elseif slot:FindFirstChild("SkibidiGame") then
        if slot:FindFirstChild("SkibidiGame").Text ~= text then
            toolName.Visible = false
            slot:FindFirstChild("SkibidiGame").Text = text
        end
    end
end

local magichealth = player.PlayerGui:WaitForChild("ScreenGui"):WaitForChild("MagicHealth")
local UltLabel = player.PlayerGui:WaitForChild("ScreenGui"):WaitForChild("MagicHealth"):WaitForChild("TextLabel")
UltLabel.Visible = false
local UltLabel = UltLabel:Clone()
UltLabel.Visible = true
UltLabel.Name = "SkibidiRizzlerGyattOhio"
UltLabel.Parent = magichealth

task.spawn(function()
    while true do
        UltLabel.Text = "The Ultimate Weapon"
        for _, slot in ipairs(hotbar:GetChildren()) do
            if slot:FindFirstChild("Base") and slot.Base:FindFirstChild("ToolName") then
                local toolNameText = slot.Base.ToolName.Text
                if toolNameText == "Normal Punch" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "Shove" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "Uppercut" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "Death Counter" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "Table Flip" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "" then
                    cloneToolName(slot.Base, "")
                elseif toolNameText == "" then
                    cloneToolName(slot.Base, "")
                end
            end
        end
        task.wait(1)
    end
end)

--[[Animations]]

local p = game.Players.LocalPlayer
local Humanoid = p.Character:WaitForChild("Humanoid")

-- Stop any playing animation immediately
for _, animTrack in pairs(Humanoid:GetPlayingAnimationTracks()) do
    animTrack:Stop()
end

-- Create and load new animation
local AnimAnim = Instance.new("Animation")
AnimAnim.AnimationId = "rbxassetid://15957376722" -- Your animation ID

local Anim = Humanoid:LoadAnimation(AnimAnim)

-- Play the animation
Anim:Play()

-- Adjust the speed
Anim:AdjustSpeed(0.1) -- Initial speed (does nothing because of the next line)
Anim.TimePosition = 0 -- Start time at 0
Anim:AdjustSpeed(0.9) -- Adjust speed again (this is effective)

getgenv().Time = 0.5

getgenv().Head = {
    91010921135712,  -- Accessory IDs for the head
    91010921135712,
    91010921135712,
}

getgenv().Torso = {
    4340259867,  -- Accessory IDs for the torso
    11925342157,
}

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Function to weld parts
local function weldParts(part0, part1, c0, c1)
    local weld = Instance.new("Weld")
    weld.Part0 = part0
    weld.Part1 = part1
    weld.C0 = c0
    weld.C1 = c1
    weld.Parent = part0
    return weld
end

-- Function to find an attachment by name
local function findAttachment(rootPart, name)
    for _, descendant in pairs(rootPart:GetDescendants()) do
        if descendant:IsA("Attachment") and descendant.Name == name then
            return descendant
        end
    end
end

-- Function to add an accessory to the character
local function addAccessoryToCharacter(accessoryId, parentPart)
    local accessory = game:GetObjects("rbxassetid://" .. tostring(accessoryId))[1]
    local character = player.Character

    -- Parent the accessory to Workspace first
    accessory.Parent = game.Workspace

    local handle = accessory:FindFirstChild("Handle")
    if handle then
        handle.CanCollide = false
        local attachment = handle:FindFirstChildOfClass("Attachment")
        if attachment then
            local parentAttachment = findAttachment(parentPart, attachment.Name)
            if parentAttachment then
                weldParts(parentPart, handle, parentAttachment.CFrame, attachment.CFrame)
            end
        else
            local parent = character:FindFirstChild(parentPart.Name)
            if parent then
                local attachmentPoint = accessory.AttachmentPoint
                weldParts(parent, handle, CFrame.new(0, 0.5, 0), attachmentPoint.CFrame)
            end
        end
    end

    -- Parent the accessory to the character
    accessory.Parent = character
end

-- Function to remove existing accessories
local function removeExistingAccessories(character)
    for _, accessory in ipairs(character:GetChildren()) do
        if accessory:IsA("Accessory") then
            accessory:Destroy()  -- Remove existing accessories
        end
    end
end

-- Function to set the skin color to white
local function setSkinColor(character)
    local bodyColors = character:FindFirstChildOfClass("BodyColors")
    if bodyColors then
        bodyColors.HeadColor = BrickColor.White()
        bodyColors.TorsoColor = BrickColor.White()
        bodyColors.LeftArmColor = BrickColor.White()
        bodyColors.RightArmColor = BrickColor.White()
        bodyColors.LeftLegColor = BrickColor.White()
        bodyColors.RightLegColor = BrickColor.White()
    end
end

-- Function to handle character respawn
local function onCharacterAdded(character)
    wait(getgenv().Time)

    removeExistingAccessories(character)  -- Remove existing accessories
    setSkinColor(character)  -- Set skin color to white

    for _, accessoryId in ipairs(getgenv().Head) do
        addAccessoryToCharacter(accessoryId, character.Head)
    end

    for _, accessoryId in ipairs(getgenv().Torso) do
        addAccessoryToCharacter(accessoryId, character:FindFirstChild("UpperTorso") or character:FindFirstChild("Torso"))
    end
end

-- Function to handle player death
local function onCharacterDied()
    local character = player.Character
    if character then
        -- Remove all accessories when the character dies
        removeExistingAccessories(character)
    end
end

-- Connect the death event to handle when the player dies
local function onCharacterAddedDeath(character)
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.Died:Connect(onCharacterDied)
end

-- Initial connection for when the script runs
if player.Character then
    onCharacterAdded(player.Character)
    onCharacterAddedDeath(player.Character)
end

player.CharacterAdded:Connect(function(character)
    onCharacterAdded(character)
    onCharacterAddedDeath(character)
end)

-- Permanently disable the script on player death
local char = game.Players.LocalPlayer.Character or game.Players.LocalPlayer.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")

humanoid.Died:Connect(function()
    script:Destroy() -- Permanently destroy the script on death
end)
