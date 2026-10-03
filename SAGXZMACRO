local replicated_storage = cloneref(game:GetService('ReplicatedStorage'))
local workspace = cloneref(game:GetService('Workspace'))

local _token
for _, Function in getgc(true) do
    if type(Function) ~= 'function' or not debug.info(Function, 's'):find('PRY', 1, true) then
        continue
    end
    for _, value in debug.getupvalues(Function) do
        if type(value) == 'function' then
            _token = value
            break
        end
    end
    if _token then break end
end

function _tokenize(_remote_uid)
    local time = tostring(math.floor(workspace:GetServerTimeNow() * 100))
    local key = _token(_remote_uid, 'TIME')
    local characters = table.create(#time)
    for index = 1, #time do
        characters[index] = string.char(bit32.bxor(
            (string.byte(time, index) + index) % 256,
            string.byte(key, (index - 1) % #key + 1)
        ))
    end
    return table.concat(characters)
end

local _reverted = {}
local _original = {}
local _captured = nil

function _is_valid(args)
    return #args == 8 and type(args[2]) == "string" and type(args[3]) == "string"
        and type(args[4]) == "number" and typeof(args[5]) == "CFrame"
        and type(args[6]) == "table" and type(args[7]) == "table" and type(args[8]) == "boolean"
end

function _hook(remote)
    if not _reverted[remote] then
        if not _original[getrawmetatable(remote)] then
            _original[getrawmetatable(remote)] = true
            local _meta = getrawmetatable(remote)
            setreadonly(_meta, false)
            local _old = _meta.__index
            _meta.__index = function(self, key)
                if (key == 'FireServer' and self:IsA('RemoteEvent'))
                    or (key == 'InvokeServer' and self:IsA('RemoteFunction')) then
                    return function(_, ...)
                        local _arguments = {...}
                        if _is_valid(_arguments) then
                            if not _reverted[self] then
                                _reverted[self] = _arguments
                                _captured = {remote = self, args = _arguments}
                            end
                        end
                        return _old(self, key)(_, unpack(_arguments))
                    end
                end
                return _old(self, key)
            end
            setreadonly(_meta, true)
        end
    end
end

for _, _remote in pairs(replicated_storage:GetDescendants()) do
    if _remote:IsA('RemoteEvent') or _remote:IsA('RemoteFunction') then
        _hook(_remote)
    end
end

task.wait(5)

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer

local parentGui
pcall(function() parentGui = gethui and gethui() or CoreGui end)
if not parentGui then parentGui = LocalPlayer:WaitForChild("PlayerGui") end

if parentGui:FindFirstChild("SpooferUI") then parentGui.SpooferUI:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpooferUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = parentGui

local THEME = {
    Background = Color3.fromRGB(28, 30, 42),
    CardBg     = Color3.fromRGB(45, 48, 64),
    CardHover  = Color3.fromRGB(62, 66, 86),
    Accent     = Color3.fromRGB(0, 220, 255),
    TextMain   = Color3.fromRGB(255, 255, 255),
    TextDim    = Color3.fromRGB(190, 195, 215),
    Active     = Color3.fromRGB(80, 230, 130),
    Inactive   = Color3.fromRGB(255, 80, 100),
    NotifyBg   = Color3.fromRGB(32, 35, 50),
    NotifyLine = Color3.fromRGB(58, 62, 84),
    KeybindOn  = Color3.fromRGB(255, 200, 60),
    KeybindSet = Color3.fromRGB(80, 230, 130),
    Hold       = Color3.fromRGB(255, 160, 60),
}

local Frame = Instance.new("CanvasGroup")
Frame.Size = UDim2.new(0, 270, 0, 610)
Frame.Position = UDim2.new(0, 20, 0, 20)
Frame.BackgroundColor3 = THEME.Background
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true
Frame.ClipsDescendants = false
Frame.Parent = ScreenGui
Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 12)

local stroke = Instance.new("UIStroke", Frame)
stroke.Color = Color3.fromRGB(75, 78, 95)
stroke.Thickness = 1.5
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local Header = Instance.new("Frame")
Header.Size = UDim2.new(1, 0, 0, 40)
Header.BackgroundColor3 = Color3.fromRGB(42, 46, 64)
Header.BorderSizePixel = 0
Header.Parent = Frame
Instance.new("UICorner", Header).CornerRadius = UDim.new(0, 12)
local headerFix = Instance.new("Frame")
headerFix.Size = UDim2.new(1, 0, 0, 12)
headerFix.Position = UDim2.new(0, 0, 1, -12)
headerFix.BackgroundColor3 = Color3.fromRGB(42, 46, 64)
headerFix.BorderSizePixel = 0
headerFix.Parent = Header

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -20, 1, 0)
Title.Position = UDim2.new(0, 14, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "S4G4XZ ☣️"
Title.TextColor3 = THEME.TextMain
Title.TextSize = 15
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.TextStrokeTransparency = 0.4
Title.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
Title.Parent = Header

local keybinds = {}
local keybindBtns = {}
local choosingKeybind = nil
local _suppressKeybindExec = false

local function keyName(k)
    if k == nil then return "None" end
    return k.Name
end

local function updateKeybindBtn(action)
    local entry = keybindBtns[action]
    if not entry then return end
    local k = keybinds[action]
    entry.Text = keyName(k)
    if choosingKeybind == action then
        entry.TextColor3 = THEME.KeybindOn
    elseif k then
        entry.TextColor3 = THEME.KeybindSet
    else
        entry.TextColor3 = THEME.TextDim
    end
end

local function makeBtn(text, yPos)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -20, 0, 26)
    btn.Position = UDim2.new(0, 10, 0, yPos)
    btn.BackgroundColor3 = THEME.CardBg
    btn.Text = text
    btn.TextColor3 = THEME.TextMain
    btn.TextSize = 12
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    btn.TextStrokeTransparency = 0.5
    btn.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    btn.Parent = Frame
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)

    local bs = Instance.new("UIStroke", btn)
    bs.Color = Color3.fromRGB(75, 78, 95)
    bs.Thickness = 1

    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = THEME.CardHover}):Play()
        TweenService:Create(bs, TweenInfo.new(0.15), {Color = THEME.Accent}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = THEME.CardBg}):Play()
        TweenService:Create(bs, TweenInfo.new(0.15), {Color = Color3.fromRGB(75, 78, 95)}):Play()
    end)

    return btn, bs
end

local function makeActionRow(text, yPos, action)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -108, 0, 26)
    btn.Position = UDim2.new(0, 10, 0, yPos)
    btn.BackgroundColor3 = THEME.CardBg
    btn.Text = text
    btn.TextColor3 = THEME.TextMain
    btn.TextSize = 12
    btn.Font = Enum.Font.GothamBold
    btn.BorderSizePixel = 0
    btn.AutoButtonColor = false
    btn.TextStrokeTransparency = 0.5
    btn.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    btn.Parent = Frame
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
    local bs = Instance.new("UIStroke", btn)
    bs.Color = Color3.fromRGB(75, 78, 95)
    bs.Thickness = 1

    btn.MouseEnter:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = THEME.CardHover}):Play()
        TweenService:Create(bs, TweenInfo.new(0.15), {Color = THEME.Accent}):Play()
    end)
    btn.MouseLeave:Connect(function()
        TweenService:Create(btn, TweenInfo.new(0.15), {BackgroundColor3 = THEME.CardBg}):Play()
        TweenService:Create(bs, TweenInfo.new(0.15), {Color = Color3.fromRGB(75, 78, 95)}):Play()
    end)

    local kb = Instance.new("TextButton")
    kb.Size = UDim2.new(0, 88, 0, 26)
    kb.Position = UDim2.new(1, -98, 0, yPos)
    kb.BackgroundColor3 = Color3.fromRGB(36, 39, 54)
    kb.Text = "None"
    kb.TextColor3 = THEME.TextDim
    kb.TextSize = 11
    kb.Font = Enum.Font.GothamBold
    kb.BorderSizePixel = 0
    kb.AutoButtonColor = false
    kb.Parent = Frame
    Instance.new("UICorner", kb).CornerRadius = UDim.new(0, 6)
    local ks = Instance.new("UIStroke", kb)
    ks.Color = Color3.fromRGB(75, 78, 95)
    ks.Thickness = 1

    kb.MouseEnter:Connect(function()
        TweenService:Create(kb, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(50, 54, 72)}):Play()
        TweenService:Create(ks, TweenInfo.new(0.15), {Color = THEME.Accent}):Play()
    end)
    kb.MouseLeave:Connect(function()
        TweenService:Create(kb, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(36, 39, 54)}):Play()
        TweenService:Create(ks, TweenInfo.new(0.15), {Color = Color3.fromRGB(75, 78, 95)}):Play()
    end)

    keybindBtns[action] = kb

    kb.MouseButton1Click:Connect(function()
        if choosingKeybind == action then
            choosingKeybind = nil
        else
            choosingKeybind = action
        end
        for a, _ in pairs(keybindBtns) do
            updateKeybindBtn(a)
        end
        HintLabel.Text = choosingKeybind and ("Aguardando tecla p/ " .. action .. " (Backspace = limpar)") or ""
    end)

    return btn, kb
end

local function makeDivider(text, yPos)
    local d = Instance.new("TextLabel")
    d.Size = UDim2.new(1, -20, 0, 20)
    d.Position = UDim2.new(0, 10, 0, yPos)
    d.BackgroundTransparency = 1
    d.Text = "◆ " .. text .. " ◆"
    d.TextColor3 = THEME.TextDim
    d.TextSize = 11
    d.Font = Enum.Font.GothamBold
    d.TextStrokeTransparency = 0.5
    d.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    d.Parent = Frame
    return d
end

local function makeTextBox(placeholder, yPos)
    local box = Instance.new("TextBox")
    box.Size = UDim2.new(1, -20, 0, 26)
    box.Position = UDim2.new(0, 10, 0, yPos)
    box.BackgroundColor3 = Color3.fromRGB(36, 39, 54)
    box.Text = ""
    box.PlaceholderText = placeholder
    box.PlaceholderColor3 = THEME.TextDim
    box.TextColor3 = THEME.TextMain
    box.TextSize = 11
    box.Font = Enum.Font.Gotham
    box.ClearTextOnFocus = false
    box.BorderSizePixel = 0
    box.Parent = Frame
    Instance.new("UICorner", box).CornerRadius = UDim.new(0, 6)
    local s = Instance.new("UIStroke", box)
    s.Color = Color3.fromRGB(75, 78, 95)
    s.Thickness = 1
    return box, s
end

makeDivider("COMBATE", 52)
local SpamManualBtn = makeActionRow("Spam Manual: OFF", 76, "spam_manual")
local SpamModeBtn = makeBtn("Spam Mode: Toggle", 106)
local SpamAnimFixBtn = makeBtn("Spam Anim Fix: OFF", 136)

makeDivider("INTERFACE", 170)
local UIKeyBtn = makeActionRow("Toggle UI", 194, "toggle_ui")
local NotifyToggleBtn = makeActionRow("Notifier: ON", 224, "notifier")

makeDivider("SKIN CHANGER", 258)
local SkinToggleBtn = makeBtn("Skin Changer: OFF", 282)
local SwordNameBox, sBoxStroke = makeTextBox("Nome da Espada...", 312)

makeDivider("APARÊNCIA", 346)
local HeadlessBtn = makeBtn("Headless: OFF", 370)
local KorbloxBtn = makeBtn("Korblox: OFF", 400)

local HintLabel = Instance.new("TextLabel")
HintLabel.Size = UDim2.new(1, -20, 0, 16)
HintLabel.Position = UDim2.new(0, 10, 0, 434)
HintLabel.BackgroundTransparency = 1
HintLabel.Text = ""
HintLabel.TextColor3 = Color3.fromRGB(255, 210, 80)
HintLabel.TextSize = 10
HintLabel.Font = Enum.Font.Gotham
HintLabel.TextStrokeTransparency = 0.4
HintLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
HintLabel.Parent = Frame

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 24, 0, 24)
CloseBtn.Position = UDim2.new(1, -30, 1, -30)
CloseBtn.BackgroundColor3 = THEME.Inactive
CloseBtn.Text = "×"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 16
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
CloseBtn.AutoButtonColor = false
CloseBtn.Parent = Frame
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 6)
local closeStroke = Instance.new("UIStroke", CloseBtn)
closeStroke.Color = Color3.fromRGB(255, 130, 150)
closeStroke.Thickness = 1
CloseBtn.MouseEnter:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(255, 70, 90)}):Play()
end)
CloseBtn.MouseLeave:Connect(function()
    TweenService:Create(CloseBtn, TweenInfo.new(0.15), {BackgroundColor3 = THEME.Inactive}):Play()
end)

local TikTokBtn = Instance.new("TextButton")
TikTokBtn.Size = UDim2.new(1, -20, 0, 22)
TikTokBtn.Position = UDim2.new(0, 10, 1, -54)
TikTokBtn.BackgroundColor3 = Color3.fromRGB(28, 30, 42)
TikTokBtn.Text = "Siga-me no ttk @_s4gxztrash"
TikTokBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
TikTokBtn.TextSize = 11
TikTokBtn.Font = Enum.Font.GothamBold
TikTokBtn.BorderSizePixel = 0
TikTokBtn.AutoButtonColor = false
TikTokBtn.Parent = Frame
Instance.new("UICorner", TikTokBtn).CornerRadius = UDim.new(0, 6)
local ttStroke = Instance.new("UIStroke", TikTokBtn)
ttStroke.Color = Color3.fromRGB(75, 78, 95)
ttStroke.Thickness = 1

TikTokBtn.MouseEnter:Connect(function()
    TweenService:Create(TikTokBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(40, 44, 62), TextColor3 = THEME.Accent}):Play()
    TweenService:Create(ttStroke, TweenInfo.new(0.15), {Color = THEME.Accent}):Play()
end)
TikTokBtn.MouseLeave:Connect(function()
    TweenService:Create(TikTokBtn, TweenInfo.new(0.15), {BackgroundColor3 = Color3.fromRGB(28, 30, 42), TextColor3 = Color3.fromRGB(255, 255, 255)}):Play()
    TweenService:Create(ttStroke, TweenInfo.new(0.15), {Color = Color3.fromRGB(75, 78, 95)}):Play()
end)

TikTokBtn.MouseButton1Click:Connect(function()
    if setclipboard then
        pcall(setclipboard, "https://www.tiktok.com/@_s4gxztrash")
    end
    pcall(function()
        game:GetService("GuiService"):OpenBrowserWindow("https://www.tiktok.com/@_s4gxztrash")
    end)
end)

local notificationsEnabled = true
local NotifyHolder = Instance.new("Frame")
NotifyHolder.Name = "NotifyHolder"
NotifyHolder.Size = UDim2.new(0, 300, 1, -40)
NotifyHolder.Position = UDim2.new(1, -320, 0, 20)
NotifyHolder.BackgroundTransparency = 1
NotifyHolder.Parent = ScreenGui

local NotifyList = Instance.new("UIListLayout", NotifyHolder)
NotifyList.VerticalAlignment = Enum.VerticalAlignment.Bottom
NotifyList.HorizontalAlignment = Enum.HorizontalAlignment.Right
NotifyList.Padding = UDim.new(0, 8)
NotifyList.SortOrder = Enum.SortOrder.LayoutOrder

local _notifyOrder = 0
local function Notify(title, text, color)
    if not notificationsEnabled then return end
    _notifyOrder = _notifyOrder + 1
    color = color or THEME.Accent

    local wrap = Instance.new("Frame")
    wrap.Size = UDim2.new(0, 290, 0, 62)
    wrap.BackgroundTransparency = 1
    wrap.LayoutOrder = _notifyOrder
    wrap.Parent = NotifyHolder

    local card = Instance.new("Frame")
    card.Size = UDim2.new(1, 0, 1, 0)
    card.BackgroundColor3 = THEME.NotifyBg
    card.BorderSizePixel = 0
    card.Position = UDim2.new(1.2, 0, 0, 0)
    card.ClipsDescendants = true
    card.Parent = wrap
    Instance.new("UICorner", card).CornerRadius = UDim.new(0, 10)

    local cStroke = Instance.new("UIStroke", card)
    cStroke.Color = THEME.NotifyLine
    cStroke.Thickness = 1

    local sideBar = Instance.new("Frame")
    sideBar.Size = UDim2.new(0, 3, 1, -14)
    sideBar.Position = UDim2.new(0, 0, 0.5, 0)
    sideBar.AnchorPoint = Vector2.new(0, 0.5)
    sideBar.BackgroundColor3 = color
    sideBar.BorderSizePixel = 0
    sideBar.Parent = card
    Instance.new("UICorner", sideBar).CornerRadius = UDim.new(1, 0)

    local tLbl = Instance.new("TextLabel")
    tLbl.BackgroundTransparency = 1
    tLbl.Position = UDim2.new(0, 14, 0, 8)
    tLbl.Size = UDim2.new(1, -22, 0, 16)
    tLbl.Text = title or "Aviso"
    tLbl.Font = Enum.Font.GothamBold
    tLbl.TextSize = 12
    tLbl.TextColor3 = color
    tLbl.TextXAlignment = Enum.TextXAlignment.Left
    tLbl.Parent = card

    local dLbl = Instance.new("TextLabel")
    dLbl.BackgroundTransparency = 1
    dLbl.Position = UDim2.new(0, 14, 0, 26)
    dLbl.Size = UDim2.new(1, -22, 0, 24)
    dLbl.Text = text or ""
    dLbl.Font = Enum.Font.Gotham
    dLbl.TextSize = 10
    dLbl.TextColor3 = THEME.TextDim
    dLbl.TextXAlignment = Enum.TextXAlignment.Left
    dLbl.TextYAlignment = Enum.TextYAlignment.Top
    dLbl.TextWrapped = true
    dLbl.Parent = card

    local progBg = Instance.new("Frame")
    progBg.Size = UDim2.new(1, -20, 0, 2)
    progBg.Position = UDim2.new(0, 10, 1, -5)
    progBg.AnchorPoint = Vector2.new(0, 1)
    progBg.BackgroundColor3 = Color3.fromRGB(55, 60, 80)
    progBg.BorderSizePixel = 0
    progBg.Parent = card
    Instance.new("UICorner", progBg).CornerRadius = UDim.new(1, 0)

    local progFill = Instance.new("Frame")
    progFill.Size = UDim2.new(1, 0, 1, 0)
    progFill.BackgroundColor3 = color
    progFill.BorderSizePixel = 0
    progFill.Parent = progBg
    Instance.new("UICorner", progFill).CornerRadius = UDim.new(1, 0)

    TweenService:Create(card, TweenInfo.new(0.45, Enum.EasingStyle.Quint, Enum.EasingDirection.Out),
        {Position = UDim2.new(0, 0, 0, 0)}):Play()
    TweenService:Create(progFill, TweenInfo.new(3.6, Enum.EasingStyle.Linear),
        {Size = UDim2.new(0, 0, 1, 0)}):Play()

    task.delay(3.6, function()
        TweenService:Create(card, TweenInfo.new(0.35, Enum.EasingStyle.Quint, Enum.EasingDirection.In),
            {Position = UDim2.new(1.2, 0, 0, 0)}):Play()
        task.delay(0.4, function()
            if wrap and wrap.Parent then wrap:Destroy() end
        end)
    end)
end

local spamManualEnabled  = false
local spamAnimFixEnabled = false
local skinChangerEnabled = false
local headlessEnabled    = false
local korbloxEnabled     = false
local spamMode           = "Toggle"

local uiVisible = true
local destroyed = false
local isAnimating = false

local function updateButtons()
    SpamManualBtn.Text = "Spam Manual: " .. (spamManualEnabled and "ON" or "OFF")
    SpamManualBtn.TextColor3 = spamManualEnabled and THEME.Active or THEME.TextMain

    SpamModeBtn.Text = "Spam Mode: " .. spamMode
    SpamModeBtn.TextColor3 = (spamMode == "Hold") and THEME.Hold or THEME.Accent

    SpamAnimFixBtn.Text = "Spam Anim Fix: " .. (spamAnimFixEnabled and "ON" or "OFF")
    SpamAnimFixBtn.TextColor3 = spamAnimFixEnabled and THEME.Active or THEME.TextMain

    NotifyToggleBtn.Text = "Notifier: " .. (notificationsEnabled and "ON" or "OFF")
    NotifyToggleBtn.TextColor3 = notificationsEnabled and THEME.Active or THEME.TextMain

    SkinToggleBtn.Text = "Skin Changer: " .. (skinChangerEnabled and "ON" or "OFF")
    SkinToggleBtn.TextColor3 = skinChangerEnabled and THEME.Active or THEME.TextMain

    HeadlessBtn.Text = "Headless: " .. (headlessEnabled and "ON" or "OFF")
    HeadlessBtn.TextColor3 = headlessEnabled and THEME.Active or THEME.TextMain

    KorbloxBtn.Text = "Korblox: " .. (korbloxEnabled and "ON" or "OFF")
    KorbloxBtn.TextColor3 = korbloxEnabled and THEME.Active or THEME.TextMain

    for action, _ in pairs(keybindBtns) do
        updateKeybindBtn(action)
    end

    if not choosingKeybind then
        HintLabel.Text = ""
    end
end

updateButtons()

local _vim
pcall(function()
    _vim = Instance.new("VirtualInputManager")
end)
if not _vim then
    pcall(function() _vim = game:GetService("VirtualInputManager") end)
end

local _isMobile = UserInputService.TouchEnabled and not UserInputService.MouseEnabled

local function fireAnimFixInput()
    if not _vim then return end
    pcall(function()
        if _isMobile then
            _vim:SendMouseButtonEvent(0, 0, 0, true, game, 0)
            _vim:SendMouseButtonEvent(0, 0, 0, false, game, 0)
        else
            _vim:SendKeyEvent(true, Enum.KeyCode.F, false, game)
            _vim:SendKeyEvent(false, Enum.KeyCode.F, false, game)
        end
    end)
end

local function fireParryInput()
    if mouse1press and mouse1release then
        pcall(function()
            mouse1press(); task.wait(0.03); mouse1release()
        end)
        return
    end
    if mouse1click then
        pcall(function() mouse1click() end)
        return
    end
    fireAnimFixInput()
end

local function getTargetPlayer()
    local alive = workspace:FindFirstChild("Alive")
    local myChar = LocalPlayer.Character
    local cam = workspace.CurrentCamera
    if not alive or not myChar or not cam then return nil end

    local viewport = cam.ViewportSize
    local screenCenter = Vector2.new(viewport.X * 0.5, viewport.Y * 0.5)
    local MAX_SCREEN_DISTANCE = math.min(viewport.X, viewport.Y) * 0.45

    local bestTarget = nil
    local bestScreenDistance = math.huge
    local fallbackTarget = nil
    local shortestWorldDist = math.huge

    for _, target in ipairs(alive:GetChildren()) do
        if target:IsA("Model") and target ~= myChar then
            local hrp = target:FindFirstChild("HumanoidRootPart")
            if hrp then
                local worldDist = (myChar.HumanoidRootPart.Position - hrp.Position).Magnitude
                local screenPos = cam:WorldToViewportPoint(hrp.Position)
                if screenPos.Z > 0 then
                    local screenDistance = (Vector2.new(screenPos.X, screenPos.Y) - screenCenter).Magnitude
                    if screenDistance <= MAX_SCREEN_DISTANCE and screenDistance < bestScreenDistance then
                        bestScreenDistance = screenDistance
                        bestTarget = target
                    end
                end
                if worldDist < shortestWorldDist then
                    shortestWorldDist = worldDist
                    fallbackTarget = target
                end
            end
        end
    end
    return bestTarget or fallbackTarget
end

local function fireParryRemote()
    if next(_reverted) == nil then
        fireParryInput()
        return
    end
    local cam = workspace.CurrentCamera
    if not cam then return end
    local finalCamCFrame = cam.CFrame
    local activeTargetPlayer = getTargetPlayer()
    if activeTargetPlayer and activeTargetPlayer.PrimaryPart then
        finalCamCFrame = CFrame.lookAt(cam.CFrame.Position, activeTargetPlayer.PrimaryPart.Position)
    end

    local gamePositionsSnapshot = {}
    local aliveFolder = workspace:FindFirstChild("Alive")
    if aliveFolder then
        for _, aliveChar in ipairs(aliveFolder:GetChildren()) do
            if aliveChar:IsA("Model") and aliveChar:FindFirstChild("HumanoidRootPart") then
                local screenPos = cam:WorldToScreenPoint(aliveChar.HumanoidRootPart.Position)
                gamePositionsSnapshot[aliveChar.Name] = screenPos
            end
        end
    end

    local mouseLocation = UserInputService:GetMouseLocation()
    local finalCoordinates = {mouseLocation.X, mouseLocation.Y}

    for _remote, _originalArgs in pairs(_reverted) do
        if type(_originalArgs) == "table" and _originalArgs ~= nil then
            task.spawn(function()
                pcall(function()
                    if _remote:IsA("RemoteEvent") then
                        _remote:FireServer(
                            _originalArgs[1],
                            _originalArgs[2],
                            _tokenize(_originalArgs[2]),
                            0,
                            finalCamCFrame,
                            gamePositionsSnapshot,
                            finalCoordinates,
                            false
                        )
                    end
                end)
            end)
        end
    end
end

local spamManualConn = nil
local spamHolding = false

local function spamManualStep()
    if not spamManualEnabled then return end
    local char = LocalPlayer.Character
    if not char then return end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum or hum.Health <= 0 then return end
    if not char:FindFirstChild("HumanoidRootPart") then return end
    local alive = workspace:FindFirstChild("Alive")
    if not alive or not alive:FindFirstChild(LocalPlayer.Name) then return end

    if spamAnimFixEnabled then
        fireAnimFixInput()
    end

    if next(_reverted) ~= nil then
        task.spawn(function() pcall(fireParryRemote) end)
    else
        fireParryInput()
    end
end

local function startSpamManual()
    if spamManualConn then return end
    spamManualConn = RunService.Heartbeat:Connect(function()
        if not spamManualEnabled then return end
        spamManualStep()
    end)
end

local function stopSpamManual()
    if spamManualConn then spamManualConn:Disconnect(); spamManualConn = nil end
end

getgenv().skinChanger = false
getgenv().swordModel = ""
getgenv().swordAnimations = ""
getgenv().swordFX = ""

task.spawn(function()
    local rs = replicated_storage
    local Shared = rs:WaitForChild("Shared", 30); if not Shared then return end
    local ReplicatedInstances = Shared:WaitForChild("ReplicatedInstances", 30); if not ReplicatedInstances then return end
    local swordInstancesInstance = ReplicatedInstances:WaitForChild("Swords", 30); if not swordInstancesInstance then return end

    local ok, swordInstances = pcall(require, swordInstancesInstance)
    if not ok or type(swordInstances) ~= "table" then return end

    local swordsController
    task.spawn(function()
        while task.wait(0.5) and not swordsController do
            local ok2, conns = pcall(getconnections, rs.Remotes.FireSwordInfo.OnClientEvent)
            if ok2 and conns then
                for _, v in ipairs(conns) do
                    if v.Function and islclosure and islclosure(v.Function) then
                        local ok3, up = pcall(getupvalues, v.Function)
                        if ok3 and #up == 1 and type(up[1]) == "table" then
                            swordsController = up[1]
                            break
                        end
                    end
                end
            end
        end
    end)

    local function getSlashName(swordName)
        local ok4, sln = pcall(function() return swordInstances:GetSword(swordName) end)
        return (ok4 and sln and sln.SlashName) or "SlashEffect"
    end

    local function refreshSlashName()
        local name = getgenv().swordModel
        if name ~= "" then
            getgenv().slashName = getSlashName(name)
        else
            getgenv().slashName = "SlashEffect"
        end
    end

    local function setSword()
        if not getgenv().skinChanger then return end
        if not LocalPlayer.Character then return end
        local targetName = getgenv().swordModel
        if targetName == "" then return end

        pcall(function()
            local f = rawget(swordInstances, "EquipSwordTo")
            if type(f) == "function" then
                local ups = getupvalues(f)
                for i = 1, #ups do
                    if type(ups[i]) == "boolean" then
                        setupvalue(f, i, false)
                        break
                    end
                end
            end
        end)
        pcall(function()
            swordInstances:EquipSwordTo(LocalPlayer.Character, targetName)
        end)
        task.spawn(function()
            local attempts = 0
            while not swordsController and attempts < 20 do
                task.wait(0.5); attempts = attempts + 1
            end
            if not swordsController then return end
            pcall(function()
                if swordsController.SetSword then
                    swordsController:SetSword(targetName)
                end
            end)
            pcall(function()
                if rs.Remotes:FindFirstChild("FireSwordInfo") then
                    rs.Remotes.FireSwordInfo:FireServer(targetName)
                end
                if swordsController.currentSword ~= nil then swordsController.currentSword = targetName end
                if swordsController.SwordFX ~= nil then swordsController.SwordFX = targetName end
            end)
        end)
    end

    local hookedFuncs = {}
    task.spawn(function()
        while task.wait(1) do
            local ok5, conns = pcall(getconnections, rs.Remotes.ParrySuccessAll.OnClientEvent)
            if ok5 and type(conns) == "table" then
                for _, v in ipairs(conns) do
                    local func = v.Function
                    if func and not hookedFuncs[func] then
                        if isourclosure and isourclosure(func) then
                            hookedFuncs[func] = true
                        else
                            hookedFuncs[func] = true
                            v:Disable()
                            local targetFunc = func
                            local ourFunc
                            ourFunc = function(...)
                                local args = {...}
                                if tostring(args[4]) == LocalPlayer.Name and getgenv().skinChanger then
                                    local name = getgenv().swordModel
                                    refreshSlashName()
                                    args[1] = getgenv().slashName
                                    args[3] = name
                                end
                                if setthreadidentity then pcall(setthreadidentity, 2) end
                                pcall(targetFunc, unpack(args))
                            end
                            hookedFuncs[ourFunc] = true
                            rs.Remotes.ParrySuccessAll.OnClientEvent:Connect(ourFunc)
                        end
                    end
                end
            end
        end
    end)

    getgenv().updateSword = function()
        refreshSlashName()
        setSword()
    end

    task.spawn(function()
        while task.wait(1) do
            if getgenv().skinChanger and getgenv().swordModel ~= "" then
                local char = LocalPlayer.Character
                if char then
                    if LocalPlayer:GetAttribute("CurrentlyEquippedSword") ~= getgenv().swordModel then setSword() end
                    if not char:FindFirstChild(getgenv().swordModel) then setSword() end
                    for _, v in char:GetChildren() do
                        if v:IsA("Model") and v.Name ~= getgenv().swordModel then
                            v:Destroy()
                        end
                        task.wait()
                    end
                end
            end
        end
    end)

    LocalPlayer.CharacterAdded:Connect(function()
        if getgenv().skinChanger then
            getgenv().skinChanger = false
            task.wait(1.5)
            getgenv().skinChanger = true
            task.wait(0.5)
            pcall(function() getgenv().updateSword() end)
        end
    end)
end)

local headlessConn, korbloxConn
local headlessWatcher, korbloxWatcher

local function applyHeadlessTo(char)
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end
    head.Transparency = 1
    local dec = head:FindFirstChildOfClass("Decal")
    if dec then dec:Destroy() end
end

local function removeHeadlessFrom(char)
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end
    head.Transparency = 0
end

local function applyKorbloxTo(char)
    if not char then return end
    local leg = char:FindFirstChild("RightUpperLeg") or char:FindFirstChild("Right Leg") or char:FindFirstChild("RightLeg")
    if not leg then return end
    if char:FindFirstChild("Right Leg") then
        local rl = char:FindFirstChild("Right Leg")
        for _, c in ipairs(rl:GetChildren()) do
            if c:IsA("SpecialMesh") or c:IsA("Mesh") then c:Destroy() end
        end
        local m = Instance.new("SpecialMesh")
        m.MeshId = "rbxassetid://101851696"
        m.TextureId = "rbxassetid://115727863"
        m.Scale = Vector3.new(1, 1, 1)
        m.Parent = rl
        return
    end
    for _, n in ipairs({"RightUpperLeg", "RightLowerLeg", "RightFoot"}) do
        local p = char:FindFirstChild(n)
        if p then p.Transparency = 1 end
    end
    local upper = char:FindFirstChild("RightUpperLeg")
    if upper and not upper:FindFirstChild("KorbloxLeg") then
        local part = Instance.new("Part")
        part.Name = "KorbloxLeg"
        part.CanCollide = false
        part.Massless = true
        part.Size = Vector3.new(1, 2, 1)
        local mesh = Instance.new("SpecialMesh")
        mesh.MeshId = "rbxassetid://101851696"
        mesh.TextureId = "rbxassetid://115727863"
        mesh.Scale = Vector3.new(1, 1, 1)
        mesh.Parent = part
        local weld = Instance.new("Weld")
        weld.Part0 = upper
        weld.Part1 = part
        weld.C0 = CFrame.new(0, -0.5, 0)
        weld.Parent = part
        part.Parent = upper
    end
end

local function removeKorbloxFrom(char)
    if not char then return end
    for _, n in ipairs({"RightUpperLeg", "RightLowerLeg", "RightFoot"}) do
        local p = char:FindFirstChild(n)
        if p then p.Transparency = 0 end
    end
    local upper = char:FindFirstChild("RightUpperLeg")
    if upper then
        local k = upper:FindFirstChild("KorbloxLeg")
        if k then k:Destroy() end
    end
    local rl = char:FindFirstChild("Right Leg")
    if rl then
        for _, c in ipairs(rl:GetChildren()) do
            if c:IsA("SpecialMesh") and c.MeshId == "rbxassetid://101851696" then
                c:Destroy()
            end
        end
    end
end

local function startHeadless()
    if headlessConn then return end
    local function applyAll()
        local char = LocalPlayer.Character
        if char then applyHeadlessTo(char) end
    end
    applyAll()
    headlessConn = LocalPlayer.CharacterAdded:Connect(function(c)
        task.wait(0.5)
        if headlessEnabled then applyHeadlessTo(c) end
    end)
    headlessWatcher = RunService.Heartbeat:Connect(function()
        if headlessEnabled then
            local c = LocalPlayer.Character
            if c and c:FindFirstChild("Head") and c.Head.Transparency ~= 1 then
                applyHeadlessTo(c)
            end
        end
    end)
end

local function stopHeadless()
    if headlessConn then headlessConn:Disconnect(); headlessConn = nil end
    if headlessWatcher then headlessWatcher:Disconnect(); headlessWatcher = nil end
    removeHeadlessFrom(LocalPlayer.Character)
end

local function startKorblox()
    if korbloxConn then return end
    local function applyAll()
        local char = LocalPlayer.Character
        if char then applyKorbloxTo(char) end
    end
    applyAll()
    korbloxConn = LocalPlayer.CharacterAdded:Connect(function(c)
        task.wait(0.5)
        if korbloxEnabled then applyKorbloxTo(c) end
    end)
    korbloxWatcher = RunService.Heartbeat:Connect(function()
        if korbloxEnabled then
            local c = LocalPlayer.Character
            if c then
                local rl = c:FindFirstChild("Right Leg")
                local upper = c:FindFirstChild("RightUpperLeg")
                local hasMesh = (rl and rl:FindFirstChildOfClass("SpecialMesh"))
                    or (upper and upper:FindFirstChild("KorbloxLeg"))
                if not hasMesh then applyKorbloxTo(c) end
            end
        end
    end)
end

local function stopKorblox()
    if korbloxConn then korbloxConn:Disconnect(); korbloxConn = nil end
    if korbloxWatcher then korbloxWatcher:Disconnect(); korbloxWatcher = nil end
    removeKorbloxFrom(LocalPlayer.Character)
end

local function toggleUI()
    if isAnimating then return end
    isAnimating = true
    uiVisible = not uiVisible

    if not uiVisible then
        TweenService:Create(Frame, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.In), {
            Size = UDim2.new(0, 0, 0, 0),
            Position = UDim2.new(Frame.Position.X.Scale, Frame.Position.X.Offset + 135, Frame.Position.Y.Scale, Frame.Position.Y.Offset + 305),
            GroupTransparency = 1
        }):Play()
        task.wait(0.25)
        Frame.Visible = false
        isAnimating = false
    else
        Frame.Visible = true
        Frame.Size = UDim2.new(0, 0, 0, 0)
        Frame.Position = UDim2.new(0, 20, 0, 20)
        Frame.GroupTransparency = 1
        TweenService:Create(Frame, TweenInfo.new(0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
            Size = UDim2.new(0, 270, 0, 610),
            Position = UDim2.new(0, 20, 0, 20),
            GroupTransparency = 0
        }):Play()
        task.wait(0.35)
        isAnimating = false
    end
end

local function destroyAll()
    if destroyed then return end
    destroyed = true
    spamManualEnabled = false
    getgenv().skinChanger = false
    stopSpamManual()
    stopHeadless()
    stopKorblox()
    pcall(function() ScreenGui:Destroy() end)
end

local function toggleSpamManual()
    spamManualEnabled = not spamManualEnabled
    if spamManualEnabled then startSpamManual() else stopSpamManual() end
    updateButtons()
    Notify("Spam Manual", spamManualEnabled and "Ativado ✓" or "Desativado ✗",
        spamManualEnabled and THEME.Active or THEME.Inactive)
end

local function toggleSpamAnimFix()
    spamAnimFixEnabled = not spamAnimFixEnabled
    updateButtons()
    Notify("Spam Anim Fix", spamAnimFixEnabled and "Ativado ✓" or "Desativado ✗",
        spamAnimFixEnabled and THEME.Active or THEME.Inactive)
end

local function toggleNotifications()
    notificationsEnabled = not notificationsEnabled
    updateButtons()
    if notificationsEnabled then
        Notify("Notifier", "Notificações ativadas ✓", THEME.Active)
    else
        Notify("Notifier", "Notificações desativadas ✗", THEME.Inactive)
    end
end

local keybindActions = {
    spam_manual = toggleSpamManual,
    toggle_ui   = toggleUI,
    notifier    = toggleNotifications,
}

UserInputService.InputBegan:Connect(function(input, gp)
    if destroyed then return end

    if choosingKeybind then
        if input.UserInputType ~= Enum.UserInputType.Keyboard then return end
        if input.KeyCode == Enum.KeyCode.Unknown then return end

        if input.KeyCode == Enum.KeyCode.Backspace then
            keybinds[choosingKeybind] = nil
        else
            keybinds[choosingKeybind] = input.KeyCode
        end

        local prev = choosingKeybind
        choosingKeybind = nil
        updateKeybindBtn(prev)
        HintLabel.Text = ""

        _suppressKeybindExec = true
        task.defer(function()
            _suppressKeybindExec = false
        end)
        return
    end

    if _suppressKeybindExec then return end
    if gp then return end
    if input.UserInputType ~= Enum.UserInputType.Keyboard then return end

    for action, key in pairs(keybinds) do
        if input.KeyCode == key and keybindActions[action] then
            if action == "spam_manual" and spamMode == "Hold" then
                if not spamHolding then
                    spamHolding = true
                    if not spamManualEnabled then
                        spamManualEnabled = true
                        startSpamManual()
                        updateButtons()
                    end
                end
            else
                keybindActions[action]()
            end
        end
    end
end)

UserInputService.InputEnded:Connect(function(input, gp)
    if destroyed then return end
    if input.UserInputType ~= Enum.UserInputType.Keyboard then return end

    if spamMode == "Hold" then
        local key = keybinds["spam_manual"]
        if key and input.KeyCode == key and spamHolding then
            spamHolding = false
            if spamManualEnabled then
                spamManualEnabled = false
                stopSpamManual()
                updateButtons()
            end
        end
    end
end)

SpamManualBtn.MouseButton1Click:Connect(function()
    if spamMode == "Hold" then
        Notify("Spam Manual", "Modo Hold: use apenas a keybind!", THEME.Inactive)
        return
    end
    toggleSpamManual()
end)

SpamModeBtn.MouseButton1Click:Connect(function()
    spamMode = (spamMode == "Toggle") and "Hold" or "Toggle"
    updateButtons()

    if spamMode == "Toggle" then
        spamHolding = false
    end

    Notify("Spam Mode", "Modo: " .. spamMode, (spamMode == "Hold") and THEME.Hold or THEME.Accent)
end)

SpamAnimFixBtn.MouseButton1Click:Connect(toggleSpamAnimFix)
NotifyToggleBtn.MouseButton1Click:Connect(toggleNotifications)
UIKeyBtn.MouseButton1Click:Connect(toggleUI)

SkinToggleBtn.MouseButton1Click:Connect(function()
    skinChangerEnabled = not skinChangerEnabled
    getgenv().skinChanger = skinChangerEnabled
    updateButtons()

    if skinChangerEnabled then
        if getgenv().swordModel == "" then
            Notify("Skin Changer", "Digite o nome da espada primeiro!", THEME.Inactive)
            SkinToggleBtn.Text = "Skin Changer: OFF"
            SkinToggleBtn.TextColor3 = THEME.TextMain
            skinChangerEnabled = false
            getgenv().skinChanger = false
            updateButtons()
            return
        end
        pcall(function() getgenv().updateSword() end)
        Notify("Skin Changer", "Ativado ✓", THEME.Active)
    else
        Notify("Skin Changer", "Desativado ✗", THEME.Inactive)
    end
end)

SwordNameBox.Focused:Connect(function() sBoxStroke.Color = THEME.Accent end)
SwordNameBox.FocusLost:Connect(function()
    sBoxStroke.Color = Color3.fromRGB(75, 78, 95)
    local name = SwordNameBox.Text
    getgenv().swordModel = name
    getgenv().swordAnimations = name
    getgenv().swordFX = name
    if getgenv().skinChanger then
        pcall(function() getgenv().updateSword() end)
    end
end)

HeadlessBtn.MouseButton1Click:Connect(function()
    headlessEnabled = not headlessEnabled
    if headlessEnabled then startHeadless() else stopHeadless() end
    updateButtons()
    Notify("Headless", headlessEnabled and "Ativado ✓" or "Desativado ✗",
        headlessEnabled and THEME.Active or THEME.Inactive)
end)

KorbloxBtn.MouseButton1Click:Connect(function()
    korbloxEnabled = not korbloxEnabled
    if korbloxEnabled then startKorblox() else stopKorblox() end
    updateButtons()
    Notify("Korblox", korbloxEnabled and "Ativado ✓" or "Desativado ✗",
        korbloxEnabled and THEME.Active or THEME.Inactive)
end)

CloseBtn.MouseButton1Click:Connect(destroyAll)

task.spawn(function()
    task.wait(0.6)
    Notify("S4G4XZ", "Menu carregado com sucesso.", THEME.Accent)
    task.wait(0.3)
    Notify("Dica", "Clique no 'None' para definir keybind.", THEME.Accent)
end)
