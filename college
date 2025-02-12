_G.AutoCash = true

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

setsimulationradius(1000, 1000)
local success, err = pcall(function()
    sethiddenproperty(player, "SimulationRadius", math.huge)
end)
if not success then
    warn("ETN:", err)
end

local SafePos = CFrame.new(-85.02771, 6.04272652, 89.258606, 0.315776497, 7.4798713e-08, 0.948833585, 6.21933283e-10, 1, -7.90392605e-08, -0.948833585, 2.55488519e-08, 0.315776497)

local function isSafePosition(position)
    local ray = Ray.new(position + Vector3.new(0, 5, 0), Vector3.new(0, -10, 0))
    local hit = workspace:FindPartOnRay(ray)
    return hit ~= nil
end

local function teleportSafely(target)
    if target and target:IsA("BasePart") then
        local safePos = target.Position + Vector3.new(0, 5, 0)
        if isSafePosition(safePos) then
            humanoidRootPart.CFrame = CFrame.new(safePos)
        else
            humanoidRootPart.CFrame = CFrame.new(target.Position + Vector3.new(0, 10, 0))
        end
    end
end

local function firePrompt(prompt)
    if prompt then
        for _ = 1, 3 do
            fireproximityprompt(prompt)
            task.wait(0.05)
        end
    end
end

local function getMoney()
    for _, obj in ipairs(workspace:GetChildren()) do
        if obj.Name:match("กูรู้นะ(%d+)") then
            return obj
        end
    end
    return nil
end

local function getAvailableObjectFarm()
    for _, obj in ipairs(workspace:GetChildren()) do
        if (obj.Name == "ATM" or obj.Name == "Phone Booth") and obj:FindFirstChild("Value") and obj.Value.Value < 3 then
            return obj
        end
    end
    return nil
end

local targetObjectFarm = nil

task.spawn(function()
    while _G.AutoCash do
        pcall(function()
            task.wait()

            local money = getMoney()

            if money then
                teleportSafely(money)
                firePrompt(money:FindFirstChild("ProximityPrompt"))
            else
                if not targetObjectFarm or targetObjectFarm.Value.Value >= 3 then
                    targetObjectFarm = getAvailableObjectFarm()
                end

                if not targetObjectFarm then
                    humanoidRootPart.CFrame = SafePos
                    while not targetObjectFarm do
                        task.wait(1)
                        targetObjectFarm = getAvailableObjectFarm()
                    end
                else
                    teleportSafely(targetObjectFarm)
                    firePrompt(targetObjectFarm:FindFirstChild("ProximityPrompt"))
                end
            end
        end)
    end
end)
