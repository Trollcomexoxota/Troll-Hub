if game.PlaceId == 8540346411 then
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
    local Window = OrionLib:MakeWindow({Name = "Troll Hub", HidePremium = false, IntroEnabled = false, IntroText = "Troll Hub", SaveConfig = true, ConfigFolder = "OrionTest"})

--Values
_G.autoTap = true
_G.autoRebirth = true

--Functions

function autoTap()
    while _G.autoTap == true do
        game:GetService("ReplicatedStorage").Events.Click4:FireServer()
        wait(.00001)
     end
    end

function autoRebirth()
    while _G.autoRebirth == true do
        game:GetService("ReplicatedStorage").Events.Rebirth:FireServer()
    end
end





-- Tabs
local FarmTab = Window:MakeTab({
	Name = "Auto Farm",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})

--Toggles
FarmTab:AddToggle({
	Name = "Auto Clicker",
	Default = false,
	Callback = function(Value)
        _G.autoTap = Value
        autoTap()
	end    
})

FarmTab:AddToggle({
	Name = "Auto Rebirth",
	Default = false,
	Callback = function(Value)
        _G.autoRebirth = Value
        autoRebirth()
	end    
})


end
    OrionLib:Init()
