-- [[ MÈO CHUỐI HUB - PHIÊN BẢN CHỦ TỊCH TOÀN NĂNG ]] --
-- ĐỘC QUYỀN: ĐẠI GIA MÈO CHUỐI

local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({
    Name = "Mèo Chuối Hub 🐱🍌 [FULL OPTION]", 
    HidePremium = false, 
    SaveConfig = true, 
    ConfigFolder = "MeoChuoiFinal",
    IntroText = "CHÀO MỪNG CHỦ TỊCH MÈO CHUỐI!" 
})

local CorrectKey = "meOcHuoibeo2014"

-- [[ HÀM MỞ TOÀN BỘ TÍNH NĂNG ]] --
function OpenFullMenu()
    -- 1. THÔNG BÁO ONLINE
    game:GetService("ReplicatedStorage").DefaultChatSystemChatEvents.SayMessageRequest:FireServer("✨ ĐẠI GIA MÈO CHUỐI ĐÃ ONLINE! ✨", "All")

    -- 2. TAB CHIẾN ĐẤU (AUTO EQUIP & CLICK)
    local CombatTab = Window:MakeTab({Name = "Chiến Đấu", Icon = "rbxassetid://4483362458"})
    
    CombatTab:AddButton({
        Name = "Lấy Đồ Mạnh Nhất (Auto Equip)",
        Callback = function()
            local backpack = game.Players.LocalPlayer.Backpack
            local character = game.Players.LocalPlayer.Character
            if character and character:FindFirstChild("Humanoid") then
                for _, tool in pairs(backpack:GetChildren()) do
                    if tool:IsA("Tool") then
                        tool.Parent = character -- Lôi đồ ra tay
                    end
                end
                OrionLib:MakeNotification({Name = "Mèo Chuối", Content = "Đã lấy toàn bộ vũ khí!", Time = 2})
            end
        end
    })

    CombatTab:AddToggle({
        Name = "Tự Động Đánh (Auto Click)",
        Default = false,
        Callback = function(v)
            getgenv().AutoClick = v
            spawn(function()
                while getgenv().AutoClick do
                    task.wait(0.1)
                    local VIM = game:GetService('VirtualInputManager')
                    VIM:SendMouseButtonEvent(0, 0, 0, true, game, 1)
                    VIM:SendMouseButtonEvent(0, 0, 0, false, game, 1)
                end
            end)
        end
    })

    -- 3. TAB NHÂN VẬT (SPEED & JUMP)
    local PlayerTab = Window:MakeTab({Name = "Nhân Vật", Icon = "rbxassetid://4483345998"})
    
    PlayerTab:AddSlider({
        Name = "Tốc Độ Chạy",
        Min = 16, Max = 1000, Default = 16,
        Callback = function(v) game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v end
    })

    PlayerTab:AddToggle({
        Name = "Vô Hạn Nhảy (Infinite Jump)",
        Default = false,
        Callback = function(v) getgenv().InfJump = v end
    })

    -- 4. TAB DỊCH CHUYỂN & TIỆN ÍCH
    local MiscTab = Window:MakeTab({Name = "Tiện Ích", Icon = "rbxassetid://4483345998"})
    
    MiscTab:AddButton({
        Name = "Dọn Rác Server (Giảm Lag)",
        Callback = function()
            for _,v in pairs(game.Workspace:GetChildren()) do
                if v:IsA("BasePart") and v.Transparency == 1 then v:Destroy() end
            end
            OrionLib:MakeNotification({Name = "Mèo Chuối", Content = "Server đã mượt hơn rồi đó!", Time = 2})
        end
    })

    MiscTab:AddButton({
        Name = "Đóng Script",
        Callback = function() OrionLib:Destroy() end
    })
end

-- [[ GIAO DIỆN NHẬP KEY ]] --
local KeyTab = Window:MakeTab({Name = "Xác Thực Key", Icon = "rbxassetid://4483345998"})
KeyTab:AddTextbox({
    Name = "Nhập Key Premium:",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        if Value == CorrectKey then
            OrionLib:MakeNotification({Name = "VIP", Content = "Mở khóa thành công!", Time = 3})
            OpenFullMenu()
        else
            OrionLib:MakeNotification({Name = "SAI KEY", Content = "Nhập lại đi Đại gia ơi!", Time = 3})
        end
    end      
})

-- Xử lý Vô hạn nhảy (Gắn ngoài hàm để hoạt động ổn định)
game:GetService("UserInputService").JumpRequest:Connect(function()
    if getgenv().InfJump then
        local h = game.Players.LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
        if h then h:ChangeState("Jumping") end
    end
end)

OrionLib:Init()
