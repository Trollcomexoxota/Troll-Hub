if game.PlaceId == 10033487652 then
    local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
    local Window = OrionLib:MakeWindow({Name = "Troll Hub", HidePremium = false, IntroText = "Troll Hub", SaveConfig = true, ConfigFolder = "OrionTest"})

--Values
_G.autoTap = true

--Functions

function autoTap()
    while _G.autoTap == true do
        game:GetService("ReplicatedStorage").Remotes.Tap:FireServer()
        wait(.0001)
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

end
    OrionLib:Init()
