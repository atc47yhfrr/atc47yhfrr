- 👋 Hi, I’m @atc47yhfrr
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
atc47yhfrr/atc47yhfrr is a ✨ special ✨ repository because its `READ-- GunClient (LocalScript inside the Gun Tool)
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local tool = script.Parent
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Create a RemoteEvent to communicate with the server
local shootEvent = Instance.new("RemoteEvent")
shootEvent.Name = "ShootEvent"
shootEvent.Parent = ReplicatedStorage

-- Handle shooting when the player clicks
tool.Equipped:Connect(function()
    UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
        if input.UserInputType == Enum.UserInputType.MouseButton1 and not gameProcessedEvent then
            -- Get the mouse's target position
            local targetPoint = mouse.Hit.Position
            -- Fire the RemoteEvent to the server
            shootEvent:FireServer(targetPoint)
        end
    end)
end)