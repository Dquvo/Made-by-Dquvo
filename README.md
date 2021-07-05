# Made-by-Dquvo
-- So this my axe scripts (this not for full game)

Axe local script:
local tool = script.Parent
local player = game.Players
local plr = player.LocalPlayer
local mouse = plr:GetMouse()
local firstattack = true
local camerause = false
local AxeDamage = tool:WaitForChild("AxeDetails").AxeDamage
local TreeAttackSizePlus = 1 / AxeDamage.Value
local mainrangefromtrees = 5
local secondfirsttree = false
local debounce = false
local secondtree
local CrackingTheTreePart
local oldtree
tool.Equipped:Connect(function()
mainrangefromtrees = 5
mouse.Button1Down:Connect(function()
if firstattack == true then	
CrackingTheTreePart = Instance.new("Part",game.ReplicatedStorage.CrackingPart)
end
local Area1Trees = game.Workspace.Area1Trees
coroutine.wrap(function()
for a,b in pairs(Area1Trees:GetDescendants()) do
if b:IsA("BasePart") then
local disttree = b
if mainrangefromtrees > (disttree.Position - tool.Handle.Position).magnitude then
if mouse.Target == b then						
if firstattack == true	then
oldtree = b
if camerause == false then
game.ReplicatedStorage.UseCamera:FireServer(game.Players.LocalPlayer,b)
camerause = true
end
CrackingTheTreePart.Parent = oldtree
CrackingTheTreePart.Anchored = true
CrackingTheTreePart.CFrame = oldtree.CFrame * CFrame.new(0,-mouse.Hit.Position.Y/mouse.Hit.Position.Y,0)
CrackingTheTreePart.Size = Vector3.new(.1, .2, 1.4)
firstattack = false
secondfirsttree = true
end
if secondfirsttree == true then
if mouse.Target ~= oldtree and mouse.Target == b then
CrackingTheTreePart.Parent = b
CrackingTheTreePart.Anchored = true
CrackingTheTreePart.CFrame = b.CFrame * CFrame.new(0,-mouse.Hit.Position.Y/mouse.Hit.Position.Y,0)
CrackingTheTreePart.Size = Vector3.new(.1, .2, 1.4)
firstattack = true
secondfirsttree = false
end
end
if firstattack == false then
if CrackingTheTreePart.Size.X < 1.6 or CrackingTheTreePart.Size.X > 0 then
if debounce == false then
CrackingTheTreePart.Size = CrackingTheTreePart.Size + Vector3.new(TreeAttackSizePlus,0,0)
game.ReplicatedStorage.WaitEvent:FireServer(plr)
debounce = true
wait(1)
debounce = false
end
if firstattack == false and camerause == true then
warn("this player already on the camera")								
end
if firstattack == false and camerause == false then
game.ReplicatedStorage.UseCamera:FireServer(game.Players.LocalPlayer,b)
end
if CrackingTheTreePart.Size.X > 1.5 then
local debounce = false
if debounce == false then
CrackingTheTreePart.Parent = game.ReplicatedStorage.CrackingPart
CrackingTheTreePart.Size = Vector3.new(.1, .2, 1.4)
game.ReplicatedStorage.DamageToTree:FireServer(oldtree,game.Players.LocalPlayer)
firstattack = true
debounce = true
wait(1)
debounce = false
end
end
if secondfirsttree == true and firstattack == false then
local debounce = false
if CrackingTheTreePart.Size.X > 1.6 then
if debounce == false then
CrackingTheTreePart.Parent = game.ReplicatedStorage.CrackingPart
CrackingTheTreePart.Size = Vector3.new(.1, .2, 1.4)
game.ReplicatedStorage.DamageToTree:FireServer(b,game.Players.LocalPlayer)
firstattack = true
debounce = true			
wait(.1)
debounce = false
end
end
end
end	
end
end
end
end
end
end)()
end)
end)
tool.Unequipped:Connect(function()
mainrangefromtrees = 0
end)
game.ReplicatedStorage.LeaveFromTree.OnClientEvent:Connect(function(x,plr)
camerause = false
end)

--Axe ServerScript:
game.Players.PlayerAdded:Connect(function(plr)
	local leaderstats = Instance.new("Folder",plr)
	leaderstats.Name = "leaderstats"
	local plrsettings = Instance.new("Folder",plr)
	plrsettings.Name = "plrsettings"
	local Wood = Instance.new("IntValue",plr.leaderstats)
	Wood.Name = "Wood"
	local TMoney = Instance.new("IntValue",plr.leaderstats)
	TMoney.Name = "TMoney"
	local Multiplier = Instance.new("NumberValue",plr.plrsettings)
	Multiplier.Name = "Multiplier"
	Multiplier.Value = 1.2 -- this is starting Multiplier 1.2
	local textmultiplier = Multiplier.Value
	local MultiplierTextValue = Instance.new("NumberValue",plr.plrsettings)
	MultiplierTextValue.Name = "MultiplierTextValue"
	MultiplierTextValue.Value = textmultiplier-0.2 -- this is Multiplier 1
	local Backpack = Instance.new("IntValue",plr.plrsettings)
	Backpack.Name = "Backpack"
	Backpack.Value = 1 
	
	local Area2Trees = game.Workspace.Area1Trees
	for a,b in pairs(Area2Trees:GetChildren()) do

	end
end)

game.ReplicatedStorage.DamageToTree.OnServerEvent:Connect(function(x,tree,plr)
	local Area1TreeWood = game.Workspace.Area1Trees.NormalTree.TreeSettings.Wood
	local Area1PalmTree = game.Workspace.Area1Trees.PalmTrees.TreeSettings.Wood
	plr.Character.HumanoidRootPart.Anchored = false
	if plr.PlayerGui:FindFirstChild("LeaveFromTree") then
		plr.PlayerGui:FindFirstChild("LeaveFromTree"):Destroy()
	end
	game.ReplicatedStorage.LeaveFromTree:FireAllClients(plr,tree)
	if tree.Name == "Tree" then
		local debounce = false
		if debounce == false then
			tree.Parent = nil
			plr.leaderstats.Wood.Value = plr.leaderstats.Wood.Value + Area1TreeWood.Value
			debounce = true
			wait(.5)
			debounce = false
		end
	end
	if tree.Name == "PalmTree" then
		local debounce = false
		if debounce == false then
			tree.Parent = nil
			plr.leaderstats.Wood.Value = plr.leaderstats.Wood.Value + Area1PalmTree.Value
			debounce = true
			wait(.5)
			debounce = false
		end
	end

end)
game.ReplicatedStorage.WaitEvent.OnServerEvent:Connect(function(x,plr)
local waitgui = game.ServerStorage.GameGuis.WaitGui:Clone()
waitgui.Parent = plr.PlayerGui
game.ReplicatedStorage.WaitEvent:FireAllClients(plr)
end)
game.ReplicatedStorage.FullBackpack.OnServerEvent:Connect(function(x,plr,numberback)
	if numberback == 1 then
		plr.leaderstats.Wood.Value = 15
	end
end)

game.ReplicatedStorage.UseCamera.OnServerEvent:Connect(function(x,plr,tree)
local leavefromtreegui = game.ServerStorage.GameGuis.LeaveFromTree:Clone()
leavefromtreegui.Parent = plr.PlayerGui	
warn("used copy")
plr.Character.HumanoidRootPart.Anchored = true
plr.Character.HumanoidRootPart.CFrame = tree.BodyLock.CFrame
game.ReplicatedStorage.UseCamera:FireAllClients(plr,tree)
end)

game.ReplicatedStorage.FullBackpack.OnServerEvent:Connect(function(x,plr,numberback)
		if numberback == 1 then
			plr.leaderstats.Wood.Value = 15
		end
end)

game.ReplicatedStorage.LeaveFromTree.OnServerEvent:Connect(function(x,plr)
	plr.Character.HumanoidRootPart.Anchored = false
	game.ReplicatedStorage.LeaveFromTree:FireAllClients(plr)
end)

--Camera Settings

game.ReplicatedStorage.UseCamera.OnServerEvent:Connect(function(x,plr,tree)
	local camera = script.Parent
	camera.CFrame = tree.CFrame * CFrame.new(4,2.5,8)
	camera.CFrame = camera.CFrame * CFrame.fromOrientation(0,360,0)
end)

-- Sell

local SellPart = script.Parent

SellPart.Touched:Connect(function(hit)
local plr = game.Players:GetPlayerFromCharacter(hit:FindFirstAncestorOfClass("Model"))
local Wood = plr.leaderstats:FindFirstChild("Wood")
local leaderstats = plr:FindFirstChild("leaderstats")
if hit.Parent:IsA("Model") then
	if plr:FindFirstChild("leaderstats") then
			if plr.leaderstats:FindFirstChild("Wood") then
				local plrmultiplier = plr.plrsettings.Multiplier
				local plrwoods = plr.leaderstats.Wood
				local plrcash = plr.leaderstats.TMoney
				plrcash.Value = plrcash.Value + plrwoods.Value * plrmultiplier.Value
				wait()
				plrwoods.Value = 0
		end
	  end
	end
end)

game.ReplicatedStorage.PlayerDetect.OnServerEvent:Connect(function(x,plr)
	local plrmultiplier = plr.plrsettings.MultiplierTextValue
	script.Parent.BillboardGui.TextLabel.Text = "Your Multiplier Is "..plrmultiplier.Value.."x"
end)

-- On this video showcase: https://www.youtube.com/watch?v=Z9_-BhzNyi4&t=0s
