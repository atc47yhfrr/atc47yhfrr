- 👋 Hi, I’m @atc47yhfrr
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
atc47yhfrr/atc47yhfrr is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
-- FPSCamera (LocalScript in StarterPlayerScripts)
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rootPart = character:WaitForChild("HumanoidRootPart")
local head = character:WaitForChild("Head")

local camera = workspace.CurrentCamera
camera.FieldOfView = 70 -- Adjust for a more immersive FPS feel

-- Lock the camera to first-person
local function updateCamera()
    if character and humanoid and humanoid.Health > 0 then
        camera.CameraType = Enum.CameraType.Custom
        camera.CameraSubject = head
        camera.CFrame = CFrame.new(head.Position) * CFrame.Angles(0, 0, 0)
        UserInputService.MouseBehavior = Enum.MouseBehavior.LockCenter
    end
end

-- Update the camera every frame
RunService.RenderStepped:Connect(updateCamera)

-- Handle character respawn
player.CharacterAdded:Connect(function(newCharacter)
    character = newCharacter
    humanoid = character:WaitForChild("Humanoid")
    rootPart = character:WaitForChild("HumanoidRootPart")
    head = character:WaitForChild("Head")
end)
