-- GunServer (Script in ServerScriptService)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local shootEvent = ReplicatedStorage:WaitForChild("ShootEvent")

-- Handle the shooting event from the client
shootEvent.OnServerEvent:Connect(function(player, targetPoint)
    local character = player.Character
    if not character then return end

    local humanoid = character:FindFirstChild("Humanoid")
    if not humanoid or humanoid.Health <= 0 then return end

    local gun = character:FindFirstChild("Gun")
    if not gun then return end

    local barrel = gun:FindFirstChild("Barrel")
    if not barrel then return end

    -- Create a bullet (a simple part)
    local bullet = Instance.new("Part")
    bullet.Size = Vector3.new(0.2, 0.2, 1)
    bullet.BrickColor = BrickColor.new("Bright yellow")
    bullet.Position = barrel.Position
    bullet.CanCollide = false
    bullet.Parent = workspace

    -- Calculate direction to the target
    local direction = (targetPoint - barrel.Position).Unit * 100
    bullet.CFrame = CFrame.new(barrel.Position, barrel.Position + direction)

    -- Add velocity to the bullet
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    bodyVelocity.Velocity = direction
    bodyVelocity.Parent = bullet

    -- Destroy the bullet after 2 seconds
    game:GetService("Debris"):AddItem(bullet, 2)

    -- Raycast to detect hits
    local rayOrigin = barrel.Position
    local rayDirection = direction
    local raycastParams = RaycastParams.new()
    raycastParams.FilterDescendantsInstances = {character}
    raycastParams.FilterType = Enum.RaycastFilterType.Blacklist

    local raycastResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
    if raycastResult then
        local hitPart = raycastResult.Instance
        local hitHumanoid = hitPart.Parent:FindFirstChild("Humanoid")
        if hitHumanoid then
            -- Deal damage to the hit player
            hitHumanoid:TakeDamage(10) -- Adjust damage as needed
        end
    end
end)