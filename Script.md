local Players, RService, RStorage, Uis, TweenService, TPService = game:GetService("Players"), game:GetService("RunService"), game:GetService("ReplicatedStorage"), game:GetService("UserInputService"), game:GetService("TweenService"), game:GetService("TeleportService");
local LocalP, Mouse = Players.LocalPlayer, Players.LocalPlayer:GetMouse()
local Chat, Mute, UD, UD2, Vec3, Vec2, INew, CordCamera, CF = RStorage.DefaultChatSystemChatEvents.SayMessageRequest, RStorage.DefaultChatSystemChatEvents.MutePlayerRequest, UDim.new, UDim2.new, Vector3.new, Vector2.new, Instance.new, workspace.CurrentCamera.CoordinateFrame, CFrame.new
local Teleporting, Flying, FirstFly, Airwalk, Noclip, Animating, SuperSpeed, AntiAim, Aimbot, ItemEsp, GuardOn = false, false, true, false, false, false, false, false, false, false, false, false
local GetJumpAnim, GetIdleAnim, GetWalkAnim = LocalP.Character.Animate.jump:GetChildren(), LocalP.Character.Animate.idle:GetChildren(), LocalP.Character.Animate.walk:GetChildren();
local FSpeed, TPSpeed, WSpeed, AV = 8, 4, 150, 4
local AVDKey, AVUKey, FlyKey = "l", "k", "f";
local GuardV2 = {"die", "my mom is better then ur mom"}
local Seats = {}
local Keys = {
    ["W"] = false;["A"] = false;
    ["S"] = false;["D"] = false;
}
Uis.InputBegan:Connect(function(Key)
    if Key.KeyCode == Enum.KeyCode.W then 
        Keys["W"] = true 
    end
    if Key.KeyCode == Enum.KeyCode.A then 
        Keys["A"] = true 
    end
    if Key.KeyCode == Enum.KeyCode.S then 
        Keys["S"] = true 
    end
    if Key.KeyCode == Enum.KeyCode.D then 
        Keys["D"] = true 
    end
end)

Uis.InputEnded:Connect(function(Key)
    if Key.KeyCode == Enum.KeyCode.W then 
        Keys["W"] = false 
    end
    if Key.KeyCode == Enum.KeyCode.A then 
        Keys["A"] = false 
    end
    if Key.KeyCode == Enum.KeyCode.S then 
        Keys["S"] = false 
    end
    if Key.KeyCode == Enum.KeyCode.D then 
        Keys["D"] = false 
    end
end)
-- // Guis
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local ZV8_Header = Instance.new("TextLabel")
local Combat_Header = Instance.new("TextLabel")
local Mobility_Header = Instance.new("TextLabel")
local Animations_Header = Instance.new("TextLabel")
local Noclip_Box = Instance.new("TextButton")
local PriestANIM_Box = Instance.new("TextButton")
local GunANIM_Box = Instance.new("TextButton")
local Fly_Box = Instance.new("TextButton")
local DJumpANIM_Box = Instance.new("TextButton")
local Aimbot_Box = Instance.new("TextButton")
local Airwalk_Box = Instance.new("TextButton")
local AntiAim_Box = Instance.new("TextButton")
local Teleports_Header = Instance.new("TextLabel")
local UziTP_Box = Instance.new("TextButton")
local GlockTP_Box = Instance.new("TextButton")
local SawTP_Box = Instance.new("TextButton")
local SawTP_Box_2 = Instance.new("TextButton")
local DogANIM_Box = Instance.new("TextButton")
local GuardV2_Box = Instance.new("TextButton")
local ItemEsp_Box = Instance.new("TextButton")
local Noclip_Box_2 = Instance.new("TextButton")
-- //local functions:
local function Notify(title, text, icon, time)
    game.StarterGui:SetCore("SendNotification",{
        Title = title;
        Text = text;
        Icon = icon;
        Duration = time;
    })
end

local function ChatSpy()
    local ChatSpyFrame = LocalP.PlayerGui.Chat.Frame
    ChatSpyFrame.ChatChannelParentFrame.Visible = true
    ChatSpyFrame.ChatBarParentFrame.Position = ChatSpyFrame.ChatChannelParentFrame.Position + UD2(UD(), ChatSpyFrame.ChatChannelParentFrame.Size.Y)
end
ChatSpy()

--[[local function togglefly()
    Flying = not Flying
    local currenttext = ""
    if Flying == true then
        currenttext = "Fly: true"
        for i,v in pairs(Seats) do
            if v.Parent ~= nil then
                v.Parent = RStorage
            end
        end
    else
        currenttext = "Fly: false"
        for i,v in pairs(Seats) do
            if v.Parent ~= nil then
                v.Parent = game.Workspace
            end
        end
    end
    Notify("PAX", currenttext, "", 3)
    local T = LocalP.Character.HumanoidRootPart or LocalP.Character:FindFirstChild("Torso")
    if Flying and LocalP.Character:FindFirstChild("Humanoid") then
        local Float = INew("Part", LocalP.Character)
        Float.Name = "Float"
        Float.Transparency = 1
        Float.Size = Vector3.new(6,1,6)
        Float.Anchored = true
        LocalP.Character.HumanoidRootPart.Anchored = true
        local CONTROL = {F = 0, B = 0, L = 0, R = 0}
        local lCONTROL = {F = 0, B = 0, L = 0, R = 0}
        local SPEED = 0
            local function FLY()
                FLYING = true
                local BG = INew("BodyGyro", T)
                local BV = INew("BodyVelocity", T)
	            BG.P = 9e2
                BG.maxTorque = Vec3(9e9, math.huge, 9e9)
                BG.cframe = T.CFrame
                BV.maxForce = Vec3(math.huge, 9e9, math.huge)
                    coroutine.resume(coroutine.create(function()
                    wait()
                        repeat RService.Heartbeat:Wait()
                        if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 then
                            SPEED = 50
                        elseif not (CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0) and SPEED ~= 0 then
                            SPEED = 0
                        end
                        if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 then
                            BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                            lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
                        elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and SPEED ~= 0 then
                            BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                        else
                            BV.velocity = Vec3(0, 0, 0)
                        end
                        BG.cframe = workspace.CurrentCamera.CoordinateFrame
                        until not FLYING
                        CONTROL = {F = 0, B = 0, L = 0, R = 0}
                        lCONTROL = {F = 0, B = 0, L = 0, R = 0}
                        SPEED = 0
                        BG:destroy()
                        BV:destroy()
                    end))
                end
                Mouse.KeyDown:connect(function(KEY)
                    if KEY:lower() == 'w' then
                        CONTROL.F = FSpeed
                    elseif KEY:lower() == 's' then
                        CONTROL.B = -FSpeed
                    elseif KEY:lower() == 'a' then
                        CONTROL.L = -FSpeed
                    elseif KEY:lower() == 'd' then 
                        CONTROL.R = FSpeed
                    end
                end)
                Mouse.KeyUp:connect(function(KEY)
                    if KEY:lower() == 'w' then
                        CONTROL.F = 0
                    elseif KEY:lower() == 's' then
                        CONTROL.B = 0
                    elseif KEY:lower() == 'a' then
                        CONTROL.L = 0
                    elseif KEY:lower() == 'd' then
                        CONTROL.R = 0
                    end
                end)
            FLY()
            LocalP.Character.HumanoidRootPart.Anchored = false
        else
        if LocalP.Character and LocalP.Character:FindFirstChild("Float") then
            LocalP.Character:FindFirstChild("Float"):Destroy()
        end
        local AnimationTracks = LocalP.Character.Humanoid:GetPlayingAnimationTracks()
        for i, track in pairs (AnimationTracks) do
            if track.Name ~= "WalkAnim" or track.Name == "Walk" or track.Name == "Fires" then
                track:Stop()
            end
        end
        FLYING = false
    end
end]]--

local function togglefly()
    Flying = not Flying
    local currenttext = ""
    if Flying == true then
        currenttext = "Fly: true"
        for i,v in pairs(Seats) do
            if v.Parent ~= nil then
                v.Parent = RStorage
            end
        end
    else
        currenttext = "Fly: false"
        for i,v in pairs(Seats) do
            if v.Parent ~= nil then
                v.Parent = game.Workspace
            end
        end
    end
    Notify("Citizen", currenttext, "", 3)
    local T = LocalP.Character.HumanoidRootPart or LocalP.Character:FindFirstChild("Torso")
    if Flying and LocalP.Character:FindFirstChild("Humanoid") then
        local Float = INew("Part", LocalP.Character)
        Float.Name = "Float"
        Float.Transparency = 1
        Float.Size = Vector3.new(6,1,6)
        Float.Anchored = true
        LocalP.Character.HumanoidRootPart.Anchored = true
        local CONTROL = {F = 0, B = 0, L = 0, R = 0}
        local lCONTROL = {F = 0, B = 0, L = 0, R = 0}
        local SPEED = 0
            local function FLY()
                FLYING = true
                local BG = INew("BodyGyro", T)
                local BV = INew("BodyVelocity", T)
	            BG.P = 8e8
                BG.maxTorque = Vec3(9e9, math.huge, 9e9)
                BG.cframe = T.CFrame
                BV.maxForce = Vec3(math.huge, 9e9, math.huge)
                    coroutine.resume(coroutine.create(function()
                    wait()
                        repeat RService.Heartbeat:Wait()
                        if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 then
                            SPEED = 50
                        elseif not (CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0) and SPEED ~= 0 then
                            SPEED = 0
                        end
                        if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 then
                            BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                            lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
                        elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and SPEED ~= 0 then
                            BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                        else
                            BV.velocity = Vec3(0, 0, 0)
                        end
                        BG.cframe = workspace.CurrentCamera.CoordinateFrame
                        until not FLYING
                        CONTROL = {F = 0, B = 0, L = 0, R = 0}
                        lCONTROL = {F = 0, B = 0, L = 0, R = 0}
                        SPEED = 0
                        BG:destroy()
                        BV:destroy()
                    end))
                end
                Mouse.KeyDown:connect(function(KEY)
                    if KEY:lower() == 'w' then
                        CONTROL.F = FSpeed
                    elseif KEY:lower() == 's' then
                        CONTROL.B = -FSpeed
                    elseif KEY:lower() == 'a' then
                        CONTROL.L = -FSpeed
                    elseif KEY:lower() == 'd' then 
                        CONTROL.R = FSpeed
                    end
                end)
                Mouse.KeyUp:connect(function(KEY)
                    if KEY:lower() == 'w' then
                        CONTROL.F = 0
                    elseif KEY:lower() == 's' then
                        CONTROL.B = 0
                    elseif KEY:lower() == 'a' then
                        CONTROL.L = 0
                    elseif KEY:lower() == 'd' then
                        CONTROL.R = 0
                    end
                end)
            FLY()
            LocalP.Character.HumanoidRootPart.Anchored = false
        else
        if LocalP.Character and LocalP.Character:FindFirstChild("Float") then
            LocalP.Character:FindFirstChild("Float"):Destroy()
        end
        local AnimationTracks = LocalP.Character.Humanoid:GetPlayingAnimationTracks()
        for i, track in pairs (AnimationTracks) do
            if track.Name ~= "WalkAnim" or track.Name == "Walk" or track.Name == "Fires" then
                track:Stop()
            end
        end
        FLYING = false
    end
end




local function gnpt()
    local plrs = {} 
    local holding = {}
    local distance = {}
    for i, p in pairs(game.Players:GetPlayers()) do
        if p ~= LocalP then
            table.insert(plrs, p) 
        end
    end
    for i, plr in pairs(plrs) do
        if plr.Character ~= nil then
        local foundhead = plr.Character:FindFirstChild("Head") 
        if foundhead ~= nil then 
        local founddistance = (plr.Character:FindFirstChild("Head").Position - workspace.CurrentCamera.CFrame.p).magnitude
        local raycast = Ray.new(workspace.CurrentCamera.CFrame.p, (Mouse.Hit.p - workspace.CurrentCamera.CFrame.p).unit * founddistance)
        local foundmouse, mspos = workspace:FindPartOnRay(raycast, workspace)
        local difference = math.floor((mspos - foundhead.Position).magnitude)
        holding[plr.Name .. i] = {}
        holding[plr.Name .. i].dist = founddistance
        holding[plr.Name .. i].plr = plr
        holding[plr.Name .. i].diff = difference
        table.insert(distance, difference)
        end
    end
end

if unpack(distance) == nil then
    return nil
end

local leftdistance = math.floor(math.min(unpack(distance)))
if leftdistance > 15 then
    return nil
end

for i, h in pairs(holding) do
    if h.diff == leftdistance then
        return h.plr
    end
end
    return nil
end

-- The Streets Bypass
local rm = getrawmetatable(game)
local caller = checkcaller or is_protosmasher_caller
local rindex = rm.__index
local nindex = rm.__newindex
local ncall = rm.__namecall
setreadonly(rm, false)

rm.__newindex = newcclosure(function(self, Meme, Val)
   if not caller() then
   if tostring(self) == "HumanoidRootPart" or tostring(self) == "Torso" then
       if Meme == "CFrame" then
           return true
       end
   end -- NoClip bypass
end
return nindex(self, Meme, Val)
end)

rm.__namecall = newcclosure(function(self, ...)
   local Method = getnamecallmethod()
   local Beans = {...}
   
   if Method == "FireServer" and Beans[1] == "WalkSpeed" then
       return nil
   end 
   if Method == "FireServer" and Beans[1] == "JumpPower" then
       return nil
   end
   if Method == "FireServer" and Beans[1] == "HipHeight" then
       return nil
   end
   if tostring(self) == "Input" then 
   if Beans[1] == "bv" or Beans[1] == "hb" or Beans[1] == "ws" then 
   return wait(9e9)
   end
   end
   return ncall(self, ...)
end)
setreadonly(rm, true)
-- //RunServices:
RService.Stepped:Connect(function()
    if Noclip == true then
        LocalP.Character.Head.CanCollide = false
        LocalP.Character.Torso.CanCollide = false
    end
    if Flying and not JumpingStateDebounce then
        JumpingStateDebounce = true
        if LocalP.Character and LocalP.Character:FindFirstChild("Humanoid") then
            wait()
            LocalP.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Running)
        end
       JumpingStateDebounce = false
	end
    if Flying and LocalP.Character then
        LocalP.Character.Humanoid.PlatformStand = false;LocalP.Character.Humanoid.Sit = false
        LocalP.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Running)
    end
    if LocalP.Character:FindFirstChild("Ap") then
        LocalP.Character:FindFirstChild("Ap").CFrame = LocalP.Character.HumanoidRootPart.CFrame * CFrame.new(0,-3.5,0)
    end
end)

-- //CharacterAdded + HumanoidDied:
LocalP.Character.Humanoid.Died:Connect(function()
    if Flying then
        togglefly()
    end
end)

LocalP.CharacterAdded:Connect(function(char)
    repeat wait() 
    until LocalP.Character:FindFirstChild("Humanoid")
    LocalP.Character.Humanoid.Died:Connect(function()
        if Flying then
            togglefly()
        end
    end)
end)

for i,v in pairs(game.Workspace:GetDescendants()) do
    if v:IsA("Seat") then
        table.insert(Seats, v)
    end
end

coroutine.resume(coroutine.create(function()
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.new(0.7, 0.7, 0.7)
Frame.BorderColor3 = Color3.new(0.9, 0.9, 0.9)
Frame.BorderSizePixel = 3
Frame.Position = UDim2.new(0.0859607384, 0, 0.271775454, 0)
Frame.Size = UDim2.new(0, 446, 0, 229)
Frame.Draggable = true
Frame.Selectable = true
Frame.Active = true

ZV8_Header.Name = "ZV8_Header"
ZV8_Header.Parent = Frame
ZV8_Header.BackgroundColor3 = Color3.new(0.8, 0.8, 0.8)
ZV8_Header.BorderColor3 = Color3.new(0.9, 0.9, 0.9)
ZV8_Header.BorderSizePixel = 3
ZV8_Header.Position = UDim2.new(0.00224215258, 0, 0, 0)
ZV8_Header.Size = UDim2.new(0, 445, 0, 50)
ZV8_Header.Font = Enum.Font.SourceSans
ZV8_Header.Text = "PAX GUI - DC: el paxoi#9457"
ZV8_Header.TextColor3 = Color3.new(0, 0, 0)
ZV8_Header.TextSize = 20
ZV8_Header.TextStrokeColor3 = Color3.new(0.976471, 0.976471, 0.976471)

Combat_Header.Name = "Combat_Header"
Combat_Header.Parent = Frame
Combat_Header.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
Combat_Header.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
Combat_Header.BorderSizePixel = 3
Combat_Header.Position = UDim2.new(0.0515695103, 0, 0.256587863, 0)
Combat_Header.Size = UDim2.new(0, 78, 0, 32)
Combat_Header.Font = Enum.Font.SourceSans
Combat_Header.Text = "Combat"
Combat_Header.TextColor3 = Color3.new(0, 0, 0)
Combat_Header.TextSize = 20
Combat_Header.TextStrokeColor3 = Color3.new(0.976471, 0.976471, 0.976471)

Mobility_Header.Name = "Mobility_Header"
Mobility_Header.Parent = Frame
Mobility_Header.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
Mobility_Header.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
Mobility_Header.BorderSizePixel = 3
Mobility_Header.Position = UDim2.new(0.302749783, 0, 0.256587863, 0)
Mobility_Header.Size = UDim2.new(0, 78, 0, 32)
Mobility_Header.Font = Enum.Font.SourceSans
Mobility_Header.Text = "Mobility"
Mobility_Header.TextColor3 = Color3.new(0, 0, 0)
Mobility_Header.TextSize = 20
Mobility_Header.TextStrokeColor3 = Color3.new(0.976471, 0.976471, 0.976471)

Animations_Header.Name = "Animations_Header"
Animations_Header.Parent = Frame
Animations_Header.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
Animations_Header.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
Animations_Header.BorderSizePixel = 3
Animations_Header.Position = UDim2.new(0.545087218, 0, 0.256587863, 0)
Animations_Header.Size = UDim2.new(0, 78, 0, 32)
Animations_Header.Font = Enum.Font.SourceSans
Animations_Header.Text = "Anims"
Animations_Header.TextColor3 = Color3.new(0, 0, 0)
Animations_Header.TextSize = 20
Animations_Header.TextStrokeColor3 = Color3.new(0.976471, 0.976471, 0.976471)

Noclip_Box.Name = "Noclip_Box"
Noclip_Box.Parent = Frame
Noclip_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
Noclip_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
Noclip_Box.BorderSizePixel = 2
Noclip_Box.Position = UDim2.new(0.30179736, 0, 0.697341502, 0)
Noclip_Box.Size = UDim2.new(0, 78, 0, 30)
Noclip_Box.Font = Enum.Font.SourceSans
Noclip_Box.Text = "Noclip"
Noclip_Box.TextColor3 = Color3.new(0, 0, 0)
Noclip_Box.TextSize = 14

PriestANIM_Box.Name = "PriestANIM_Box"
PriestANIM_Box.Parent = Frame
PriestANIM_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
PriestANIM_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
PriestANIM_Box.BorderSizePixel = 2
PriestANIM_Box.Position = UDim2.new(0.543958306, 0, 0.701600254, 0)
PriestANIM_Box.Size = UDim2.new(0, 78, 0, 30)
PriestANIM_Box.Font = Enum.Font.SourceSans
PriestANIM_Box.Text = "Priest Anim"
PriestANIM_Box.TextColor3 = Color3.new(0, 0, 0)
PriestANIM_Box.TextSize = 14

GunANIM_Box.Name = "GunANIM_Box"
GunANIM_Box.Parent = Frame
GunANIM_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
GunANIM_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
GunANIM_Box.BorderSizePixel = 2
GunANIM_Box.Position = UDim2.new(0.543958306, 0, 0.434235752, 0)
GunANIM_Box.Size = UDim2.new(0, 78, 0, 30)
GunANIM_Box.Font = Enum.Font.SourceSans
GunANIM_Box.Text = "Gun Anim"
GunANIM_Box.TextColor3 = Color3.new(0, 0, 0)
GunANIM_Box.TextSize = 14

Fly_Box.Name = "Fly_Box"
Fly_Box.Parent = Frame
Fly_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
Fly_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
Fly_Box.BorderSizePixel = 2
Fly_Box.Position = UDim2.new(0.30179733, 0, 0.562010407, 0)
Fly_Box.Size = UDim2.new(0, 78, 0, 30)
Fly_Box.Font = Enum.Font.SourceSans
Fly_Box.Text = "Fly"
Fly_Box.TextColor3 = Color3.new(0, 0, 0)
Fly_Box.TextSize = 14

DJumpANIM_Box.Name = "DJumpANIM_Box"
DJumpANIM_Box.Parent = Frame
DJumpANIM_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
DJumpANIM_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
DJumpANIM_Box.BorderSizePixel = 2
DJumpANIM_Box.Position = UDim2.new(0.543958306, 0, 0.561674178, 0)
DJumpANIM_Box.Size = UDim2.new(0, 78, 0, 30)
DJumpANIM_Box.Font = Enum.Font.SourceSans
DJumpANIM_Box.Text = "DJump Anim"
DJumpANIM_Box.TextColor3 = Color3.new(0, 0, 0)
DJumpANIM_Box.TextSize = 14

Aimbot_Box.Name = "Aimbot_Box"
Aimbot_Box.Parent = Frame
Aimbot_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
Aimbot_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
Aimbot_Box.BorderSizePixel = 2
Aimbot_Box.Position = UDim2.new(0.0518557951, 0, 0.429868907, 0)
Aimbot_Box.Size = UDim2.new(0, 78, 0, 30)
Aimbot_Box.Font = Enum.Font.SourceSans
Aimbot_Box.Text = "Aimbot"
Aimbot_Box.TextColor3 = Color3.new(0, 0, 0)
Aimbot_Box.TextSize = 14

Airwalk_Box.Name = "Airwalk_Box"
Airwalk_Box.Parent = Frame
Airwalk_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
Airwalk_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
Airwalk_Box.BorderSizePixel = 2
Airwalk_Box.Position = UDim2.new(0.30179736, 0, 0.429868877, 0)
Airwalk_Box.Size = UDim2.new(0, 78, 0, 29)
Airwalk_Box.Font = Enum.Font.SourceSans
Airwalk_Box.Text = "Airwalk"
Airwalk_Box.TextColor3 = Color3.new(0, 0, 0)
Airwalk_Box.TextSize = 14

AntiAim_Box.Name = "AntiAim_Box"
AntiAim_Box.Parent = Frame
AntiAim_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
AntiAim_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
AntiAim_Box.BorderSizePixel = 2
AntiAim_Box.Position = UDim2.new(0.0518557951, 0, 0.557495415, 0)
AntiAim_Box.Size = UDim2.new(0, 78, 0, 30)
AntiAim_Box.Font = Enum.Font.SourceSans
AntiAim_Box.Text = "AntiAim"
AntiAim_Box.TextColor3 = Color3.new(0, 0, 0)
AntiAim_Box.TextSize = 14

Teleports_Header.Name = "Teleports_Header"
Teleports_Header.Parent = Frame
Teleports_Header.BackgroundColor3 = Color3.new(0.4, 0.4, 0.4)
Teleports_Header.BorderColor3 = Color3.new(0.5, 0.5, 0.5)
Teleports_Header.BorderSizePixel = 3
Teleports_Header.Position = UDim2.new(0.778271079, 0, 0.256587833, 0)
Teleports_Header.Size = UDim2.new(0, 78, 0, 32)
Teleports_Header.Font = Enum.Font.SourceSans
Teleports_Header.Text = "Teleports"
Teleports_Header.TextColor3 = Color3.new(0, 0, 0)
Teleports_Header.TextSize = 20
Teleports_Header.TextStrokeColor3 = Color3.new(0.976471, 0.976471, 0.976471)

UziTP_Box.Name = "UziTP_Box"
UziTP_Box.Parent = Frame
UziTP_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
UziTP_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
UziTP_Box.BorderSizePixel = 2
UziTP_Box.Position = UDim2.new(0.777142167, 0, 0.434235752, 0)
UziTP_Box.Size = UDim2.new(0, 78, 0, 30)
UziTP_Box.Font = Enum.Font.SourceSans
UziTP_Box.Text = "Uzi"
UziTP_Box.TextColor3 = Color3.new(0, 0, 0)
UziTP_Box.TextSize = 14

GlockTP_Box.Name = "GlockTP_Box"
GlockTP_Box.Parent = Frame
GlockTP_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
GlockTP_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
GlockTP_Box.BorderSizePixel = 2
GlockTP_Box.Position = UDim2.new(0.777142107, 0, 0.570407808, 0)
GlockTP_Box.Size = UDim2.new(0, 78, 0, 30)
GlockTP_Box.Font = Enum.Font.SourceSans
GlockTP_Box.Text = "Glock"
GlockTP_Box.TextColor3 = Color3.new(0, 0, 0)
GlockTP_Box.TextSize = 14

SawTP_Box.Name = "SawTP_Box"
SawTP_Box.Parent = Frame
SawTP_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
SawTP_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
SawTP_Box.BorderSizePixel = 2
SawTP_Box.Position = UDim2.new(0.777142167, 0, 0.701412141, 0)
SawTP_Box.Size = UDim2.new(0, 78, 0, 30)
SawTP_Box.Font = Enum.Font.SourceSans
SawTP_Box.Text = "Sawed Off"
SawTP_Box.TextColor3 = Color3.new(0, 0, 0)
SawTP_Box.TextSize = 14

SawTP_Box_2.Name = "SawTP_Box"
SawTP_Box_2.Parent = Frame
SawTP_Box_2.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
SawTP_Box_2.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
SawTP_Box_2.BorderSizePixel = 2
SawTP_Box_2.Position = UDim2.new(0.777142167, 0, 0.832416475, 0)
SawTP_Box_2.Size = UDim2.new(0, 78, 0, 30)
SawTP_Box_2.Font = Enum.Font.SourceSans
SawTP_Box_2.Text = "Rejoin"
SawTP_Box_2.TextColor3 = Color3.new(0, 0, 0)
SawTP_Box_2.TextSize = 14

DogANIM_Box.Name = "DogANIM_Box"
DogANIM_Box.Parent = Frame
DogANIM_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
DogANIM_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
DogANIM_Box.BorderSizePixel = 2
DogANIM_Box.Position = UDim2.new(0.543958306, 0, 0.828237832, 0)
DogANIM_Box.Size = UDim2.new(0, 78, 0, 30)
DogANIM_Box.Font = Enum.Font.SourceSans
DogANIM_Box.Text = "Dog Anim"
DogANIM_Box.TextColor3 = Color3.new(0, 0, 0)
DogANIM_Box.TextSize = 14

GuardV2_Box.Name = "GuardV2_Box"
GuardV2_Box.Parent = Frame
GuardV2_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
GuardV2_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
GuardV2_Box.BorderSizePixel = 2
GuardV2_Box.Position = UDim2.new(0.0518557951, 0, 0.688499808, 0)
GuardV2_Box.Size = UDim2.new(0, 78, 0, 30)
GuardV2_Box.Font = Enum.Font.SourceSans
GuardV2_Box.Text = "Guard V2"
GuardV2_Box.TextColor3 = Color3.new(0, 0, 0)
GuardV2_Box.TextSize = 14

ItemEsp_Box.Name = "ItemEsp_Box"
ItemEsp_Box.Parent = Frame
ItemEsp_Box.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
ItemEsp_Box.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
ItemEsp_Box.BorderSizePixel = 2
ItemEsp_Box.Position = UDim2.new(0.0518558025, 0, 0.819612324, 0)
ItemEsp_Box.Size = UDim2.new(0, 78, 0, 29)
ItemEsp_Box.Font = Enum.Font.SourceSans
ItemEsp_Box.Text = "Item Esp"
ItemEsp_Box.TextColor3 = Color3.new(0, 0, 0)
ItemEsp_Box.TextSize = 14

Noclip_Box_2.Name = "Noclip_Box"
Noclip_Box_2.Parent = Frame
Noclip_Box_2.BackgroundColor3 = Color3.new(0.6, 0.6, 0.6)
Noclip_Box_2.BorderColor3 = Color3.new(0.7, 0.7, 0.7)
Noclip_Box_2.BorderSizePixel = 2
Noclip_Box_2.Position = UDim2.new(0.30179736, 0, 0.828345895, 0)
Noclip_Box_2.Size = UDim2.new(0, 78, 0, 30)
Noclip_Box_2.Font = Enum.Font.SourceSans
Noclip_Box_2.Text = "Speed+"
Noclip_Box_2.TextColor3 = Color3.new(0, 0, 0)
Noclip_Box_2.TextSize = 14
end))
-- //Mouse.KeyDown(Button1Down) + Gui Button Functions 
Mouse.KeyDown:Connect(function(Key)
    if Key == FlyKey then
        if FirstFly then
            Notify("Citizen", "You can now fly, like a bird.")
            FirstFly = false
        end
        togglefly()
    end
end)

Mouse.KeyDown:Connect(function(Key)
    local NoclipKey = "x";
    if Key == NoclipKey then 
        Noclip = not Noclip 
        Notify("PAX", "Noclip: "..tostring(Noclip), "", 3)
    end
end)

Mouse.KeyDown:Connect(function(Key)
    local AVDKey = "l"
    if Key == AVDKey then 
        AV = AV - 1 
        Notify("PAX", "Aimvelocity: "..tonumber(AV), "", 3)
    end
end)

Mouse.KeyDown:Connect(function(Key)
    local AVUKey = "l"
    if Key == AVDKey then 
        AV = AV + 1 
        Notify("PAX", "Aimvelocity: "..tonumber(AV), "", 3)
    end
end)

ItemEsp_Box.MouseButton1Down:Connect(function()
function esp(instance)
    local types = ""
    local EspText = ""
    local EspFontSize = "Size14" -- Uzi has larger text
    local EspColor = BrickColor.new("Bright green") -- Temporary placeholder
    local EspPart = instance
    for i,v in pairs(instance:GetDescendants()) do
        if v:IsA("MeshPart") and v.Name == "Blade" and string.find(tostring(v.MeshId), tostring(12177251)) then
            types = "katana"
            EspText = "Katana"
            EspColor = BrickColor.new("Bright blue")
            EspPart = v:FindFirstAncestorWhichIsA("BasePart")
        elseif v:IsA("MeshPart") and v.MeshId == "rbxassetid://511726060" then
            types = "cash"
            EspText = "Cash"
            EspColor = BrickColor.new("Bright blue")
            EspPart = v
        elseif v:IsA("Sound") and v.Name == "Fire" and string.find(tostring(v.SoundId), tostring(328964620)) then
            types = "uzi"
            EspText = "Uzi"
            EspFontSize = "Size18"
            EspColor = BrickColor.new("Bright blue")
            EspPart = v:FindFirstAncestorWhichIsA("BasePart")
        elseif v:IsA("Sound") and v.Name == "Fire" and string.find(tostring(v.SoundId), tostring(142383762)) then
            types = "shotty"
            EspText = "Shotty"
            EspColor = BrickColor.new("Bright blue")
            EspPart = v:FindFirstAncestorWhichIsA("BasePart")
        end
    end


    local TracerPart = Instance.new("Part")
    TracerPart.Parent = EspPart
    TracerPart.Name = "TracerPart"
    TracerPart.CFrame = EspPart.CFrame
    TracerPart.Size = Vector3.new(0.2,0.2,0.2)
    TracerPart.Anchored = true
    TracerPart.Transparency = 1

    local billgui = Instance.new('BillboardGui', TracerPart)
    local textlab = Instance.new('TextLabel', billgui)

    billgui.Name = "ESPBillboard"
    billgui.Adornee  = TracerPart
    billgui.AlwaysOnTop = true
    billgui.ExtentsOffset = Vector3.new(0, 1, 0)
    billgui.Size = UDim2.new(0, 5, 0, 5)
	
    textlab.Name = "ESPLabel"
    textlab.BackgroundColor3 = Color3.new(255, 255, 255)
    textlab.BackgroundTransparency = 1
    textlab.BorderSizePixel = 0
    textlab.Position = UDim2.new(0, 0, 0, -40)
    textlab.Size = UDim2.new(1, 0, 10, 0)
    textlab.Visible = true
    textlab.ZIndex = 10
    textlab.Font = 'ArialBold'
    textlab.FontSize = EspFontSize
    textlab.Text = EspText
    textlab.TextColor = EspColor
    textlab.TextStrokeColor3 = Color3.fromRGB(0,0,0)
    textlab.TextStrokeTransparency = 0.6
end

for i,v in pairs(game.Workspace:GetChildren()) do
    if v.Name == "RandomSpawner" then
        esp(v)
    end
end

for i,v in pairs(game.Workspace:GetChildren()) do
    if v.Name == "RandomSpawner" then
        if v:FindFirstChild("Model") then
            v.Model.ChildAdded:Connect(function(child)
                esp(child)
            end)
        end
        v.ChildAdded:Connect(function(child)
            esp(child)
        end)
    end
end

game.Workspace.ChildAdded:Connect(function(child)
    if child.Name == "RandomSpawner" then
        repeat wait() until child.Model
        esp(child)
    end
end)
end)

GuardV2_Box.MouseButton1Down:Connect(function()
    GuardOn = true
    for _, c in pairs(LocalP.Character:GetChildren()) do 
        c.ChildAdded:Connect(function(BulletParent)
            if BulletParent.Name == "creator" and GuardOn == true then
                Chat:FireServer(GuardV2[math.random(1, #GuardV2)], "All")
            end
        end)
    end
end)

UziTP_Box.MouseButton1Down:Connect(function()
    if not Flying then 
        togglefly()
    end
    if not Noclip then 
        Noclip = true 
    end
    if workspace:FindFirstChild("Uzi | $150") then 
        local UziRoot = workspace:FindFirstChild("Uzi | $150").Head.CFrame
        local TPAction = TweenService:Create(LocalP.Character.HumanoidRootPart, TweenInfo.new(TPSpeed + .1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {CFrame = UziRoot})
        TPAction:Play()
    end
end)

GlockTP_Box.MouseButton1Down:Connect(function()
    if not Flying then 
        togglefly()
    end
    if not Noclip then 
        Noclip = true 
    end
    if workspace:FindFirstChild("Glock | $200") then 
        local GlockRoot = workspace:FindFirstChild("Glock | $200").Head.CFrame
        local TPAction = TweenService:Create(LocalP.Character.HumanoidRootPart, TweenInfo.new(TPSpeed + .1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {CFrame = GlockRoot})
        TPAction:Play()
    end
end)

SawTP_Box.MouseButton1Down:Connect(function()
    if not Flying then 
        togglefly()
    end
    if not Noclip then 
        Noclip = true 
    end
    if workspace:FindFirstChild("Sawed Off | $150") then 
        local SawRoot = workspace:FindFirstChild("Sawed Off | $150").Head.CFrame
        local TPAction = TweenService:Create(LocalP.Character.HumanoidRootPart, TweenInfo.new(TPSpeed + .1, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {CFrame = SawRoot})
        TPAction:Play()
    end
end)

Aimbot_Box.MouseButton1Down:Connect(function()
if game.PlaceId ~= 4669040 then
Notify("PAX", "Aimlock, Press E by someone and it shoots them until they die", "", 3)
local workspace = game:GetService("Workspace")
local mouse = LocalP:GetMouse()
local rclickyes = false
local rdone = false
local AV = 5

mouse.KeyDown:Connect(function(Key)
    if Key == "e" then
        rclickyes = true
        rclickdone = true
        Aimlock = true
        if LocalP and LocalP.Character:FindFirstChildWhichIsA("Tool") then
                gunyes = LocalP.Character:FindFirstChildWhichIsA("Tool")
                local otarget = gnpt()
                if otarget ~= nil and otarget.Character:FindFirstChild("Torso") and rclickyes ~= false then
                    repeat wait()
                        LocalP.Backpack.Input:FireServer("m1",{["mousehit"] = otarget.Character.Torso.CFrame + otarget.Character.Torso.Velocity/AV;})
                    until Aimlock == false or otarget.Character:FindFirstChild("Bone", true) or LocalP.Character.Humanoid.Health == 0
                end
            end
        end
    end)
end 
if game.PlaceId == 4669040 then 
Notify("PAX", "Aimlock, Press E by someone and it shoots them until they die", "", 3)
local workspace = game:GetService("Workspace")
local mouse = LocalP:GetMouse()
local rclickyes = false
local rdone = false
local AV = 5

mouse.KeyDown:Connect(function(Key)
    if Key == "e" then
        rclickyes = true
        rclickdone = true
        Aimlock = true
        if LocalP and LocalP.Character:FindFirstChildWhichIsA("Tool") then
                gunyes = LocalP.Character:FindFirstChildWhichIsA("Tool")
                local otarget = gnpt()
                if otarget ~= nil and otarget.Character:FindFirstChild("Torso") and rclickyes ~= false then
                    repeat wait()
                        gunyes.Fire:FireServer(otarget.Character.Torso.CFrame + otarget.Character.Torso.Velocity/AV)
                    until Aimlock == false or otarget.Character:FindFirstChild("KO") or LocalP.Character.Humanoid.Health == 0
                    end
                end
            end
        end)
    end
end)

SawTP_Box_2.MouseButton1Down:Connect(function()
    TPService:Teleport(game.PlaceId, LocalP)
end)

Noclip_Box.MouseButton1Down:Connect(function()
    Noclip = not Noclip 
    Notify("PAX", "Noclip: "..tostring(Noclip), "", 4)
end)

Noclip_Box_2.MouseButton1Down:Connect(function()
    if LocalP.Character:FindFirstChild("Humanoid") then 
        LocalP.Character.Humanoid.WalkSpeed = WSpeed 
    end
    LocalP.CharacterAdded:Connect(function(char)
        repeat wait()
        until LocalP.Character:FindFirstChild("Humanoid")
        LocalP.Character.Humanoid.WalkSpeed = WSpeed 
    end)
end)

Fly_Box.MouseButton1Down:Connect(function()
    togglefly()
end)

DJumpANIM_Box.MouseButton1Down:Connect(function()
    for _, a in pairs(GetJumpAnim) do 
        a.AnimationId = "rbxassetid://538512152"
    end
end)

GunANIM_Box.MouseButton1Down:Connect(function()
    for _, a in pairs(GetIdleAnim) do 
        a.AnimationId = "rbxassetid://889994935"
    end
end)

PriestANIM_Box.MouseButton1Down:Connect(function()
    for _, a in pairs(GetIdleAnim) do 
        a.AnimationId = "rbxassetid://933475808"
    end
    for _, a in pairs(GetJumpAnim) do 
        a.AnimationId = "rbxassetid://447278169"
    end
end)

DogANIM_Box.MouseButton1Down:Connect(function()
    for _, a in pairs(GetIdleAnim) do 
        a.AnimationId = "rbxassetid://948442744"
    end
    for _, a in pairs(GetWalkAnim) do 
        a.AnimationId = "rbxassetid://948442744"
    end
end)

Chat:FireServer("[PAX] HACK SCRIPT!", "All")
wait(4)
Chat:FireServer("ITS VERY COOL SCRIPT 👌", "All")
wait(2)
Chat:FireServer("[!] Press F9 for Keybinds [!]", "All")
wait(2)
Chat:FireServer("Dc: el paxoi# 9328", "All")
print([[
[PAX] Hack Keybinds --(BY Paxoi)-- 

Press F to Fly 
Press X to Noclip
Press K to Increase your Aimvelocity By 1
Press L to Decrease your aimvelocity By 1
Press E to Repeat Shoot someone until they die (Press the "Aimbot" box first)
]])
