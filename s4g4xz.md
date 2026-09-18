--// SERVIÇOS
local Players          = game:GetService("Players")
local TweenService     = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService       = game:GetService("RunService")

local player    = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

--// REMOVER ANTIGO
local antigo = playerGui:FindFirstChild("AegisMenu")
if antigo then antigo:Destroy() end

--// CONFIG
local COR_FUNDO     = Color3.fromRGB(14, 12, 22)
local COR_FUNDO_2   = Color3.fromRGB(26, 18, 48)
local COR_TOPO      = Color3.fromRGB(24, 18, 44)
local COR_ACENTO    = Color3.fromRGB(160, 100, 255)
local COR_ACENTO_2  = Color3.fromRGB(90, 190, 255)
local COR_ACENTO_3  = Color3.fromRGB(255, 100, 200)
local COR_TEXTO     = Color3.fromRGB(245, 240, 255)

--// LINK DO TIKTOK
local TIKTOK_URL = "https://www.tiktok.com/@_s4gxztrash"

--// SCREENGUI
local gui = Instance.new("ScreenGui")
gui.Name = "AegisMenu"
gui.ResetOnSpawn = false
gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
gui.IgnoreGuiInset = true
gui.Parent = playerGui

--// SOMBRA EXTERNA
local shadow = Instance.new("ImageLabel")
shadow.Name = "Shadow"
shadow.AnchorPoint = Vector2.new(0.5, 0.5)
shadow.Size = UDim2.new(0, 980, 0, 680)
shadow.Position = UDim2.new(0.5, 0, 0.5, 14)
shadow.BackgroundTransparency = 1
shadow.Image = "rbxassetid://5028857084"
shadow.ImageColor3 = COR_ACENTO
shadow.ImageTransparency = 0.55
shadow.ZIndex = 0
shadow.Parent = gui

--// FRAME PRINCIPAL
local main = Instance.new("Frame")
main.Name = "Main"
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.Size = UDim2.new(0, 900, 0, 600)
main.Position = UDim2.new(0.5, 0, 0.5, 0)
main.BackgroundColor3 = COR_FUNDO
main.BorderSizePixel = 0
main.Active = true
main.ClipsDescendants = true
main.ZIndex = 2
main.Parent = gui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 28)
mainCorner.Parent = main

local bgGrad = Instance.new("UIGradient")
bgGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, COR_FUNDO_2),
	ColorSequenceKeypoint.new(0.5, COR_FUNDO),
	ColorSequenceKeypoint.new(1, COR_FUNDO_2),
})
bgGrad.Rotation = 135
bgGrad.Parent = main

local mainStroke = Instance.new("UIStroke")
mainStroke.Thickness = 3
mainStroke.Transparency = 0.1
mainStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
mainStroke.Parent = main

local strokeGrad = Instance.new("UIGradient")
strokeGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0,   COR_ACENTO),
	ColorSequenceKeypoint.new(0.3, COR_ACENTO_2),
	ColorSequenceKeypoint.new(0.6, COR_ACENTO_3),
	ColorSequenceKeypoint.new(1,   COR_ACENTO),
})
strokeGrad.Parent = mainStroke

--// TOPBAR
local topbar = Instance.new("Frame")
topbar.Name = "Topbar"
topbar.Size = UDim2.new(1, 0, 0, 74)
topbar.BackgroundColor3 = COR_TOPO
topbar.BackgroundTransparency = 0.1
topbar.BorderSizePixel = 0
topbar.ZIndex = 4
topbar.Parent = main

local topbarCorner = Instance.new("UICorner")
topbarCorner.CornerRadius = UDim.new(0, 28)
topbarCorner.Parent = topbar

local topbarFix = Instance.new("Frame")
topbarFix.Size = UDim2.new(1, 0, 0, 28)
topbarFix.Position = UDim2.new(0, 0, 1, -28)
topbarFix.BackgroundColor3 = COR_TOPO
topbarFix.BackgroundTransparency = 0.1
topbarFix.BorderSizePixel = 0
topbarFix.ZIndex = 4
topbarFix.Parent = topbar

local topGrad = Instance.new("UIGradient")
topGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 25, 75)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 15, 40)),
})
topGrad.Rotation = 90
topGrad.Parent = topbar

local topReflex = Instance.new("Frame")
topReflex.Size = UDim2.new(1, -40, 0, 1)
topReflex.Position = UDim2.new(0, 20, 0, 1)
topReflex.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
topReflex.BackgroundTransparency = 0.6
topReflex.BorderSizePixel = 0
topReflex.ZIndex = 6
topReflex.Parent = topbar

local reflexGrad = Instance.new("UIGradient")
reflexGrad.Transparency = NumberSequence.new({
	NumberSequenceKeypoint.new(0, 1),
	NumberSequenceKeypoint.new(0.5, 0.1),
	NumberSequenceKeypoint.new(1, 1),
})
reflexGrad.Parent = topReflex

local function criarDot(x, cor, corGlow)
	local wrapper = Instance.new("Frame")
	wrapper.Size = UDim2.new(0, 18, 0, 18)
	wrapper.Position = UDim2.new(0, x, 0.5, -9)
	wrapper.BackgroundTransparency = 1
	wrapper.ZIndex = 6
	wrapper.Parent = topbar

	local glow = Instance.new("Frame")
	glow.Size = UDim2.new(1, 8, 1, 8)
	glow.Position = UDim2.new(0.5, -13, 0.5, -13)
	glow.BackgroundColor3 = corGlow
	glow.BackgroundTransparency = 0.7
	glow.BorderSizePixel = 0
	glow.ZIndex = 6
	glow.Parent = wrapper
	Instance.new("UICorner", glow).CornerRadius = UDim.new(1, 0)

	local d = Instance.new("Frame")
	d.Size = UDim2.new(1, 0, 1, 0)
	d.BackgroundColor3 = cor
	d.BorderSizePixel = 0
	d.ZIndex = 7
	d.Parent = wrapper
	Instance.new("UICorner", d).CornerRadius = UDim.new(1, 0)

	return d
end

criarDot(24, Color3.fromRGB(255, 95, 87),  Color3.fromRGB(255, 95, 87))
criarDot(50, Color3.fromRGB(255, 189, 46), Color3.fromRGB(255, 189, 46))
criarDot(76, Color3.fromRGB(39, 201, 63),  Color3.fromRGB(39, 201, 63))

local logoWrapper = Instance.new("Frame")
logoWrapper.Size = UDim2.new(0, 40, 0, 40)
logoWrapper.Position = UDim2.new(0, 116, 0.5, -20)
logoWrapper.BackgroundTransparency = 1
logoWrapper.ZIndex = 6
logoWrapper.Parent = topbar

local logoGlow = Instance.new("Frame")
logoGlow.Size = UDim2.new(1, 12, 1, 12)
logoGlow.Position = UDim2.new(0.5, -26, 0.5, -26)
logoGlow.BackgroundColor3 = COR_ACENTO
logoGlow.BackgroundTransparency = 0.6
logoGlow.BorderSizePixel = 0
logoGlow.ZIndex = 6
logoGlow.Parent = logoWrapper
Instance.new("UICorner", logoGlow).CornerRadius = UDim.new(1, 0)

local logoBox = Instance.new("Frame")
logoBox.Size = UDim2.new(1, 0, 1, 0)
logoBox.BackgroundColor3 = Color3.fromRGB(50, 30, 100)
logoBox.BorderSizePixel = 0
logoBox.ZIndex = 7
logoBox.Parent = logoWrapper
Instance.new("UICorner", logoBox).CornerRadius = UDim.new(0, 12)

local logoStroke = Instance.new("UIStroke")
logoStroke.Color = COR_ACENTO_2
logoStroke.Thickness = 1.5
logoStroke.Transparency = 0.3
logoStroke.Parent = logoBox

local logo = Instance.new("TextLabel")
logo.Size = UDim2.new(1, 0, 1, 0)
logo.BackgroundTransparency = 1
logo.Text = "⚡"
logo.TextColor3 = Color3.fromRGB(255, 255, 255)
logo.TextSize = 24
logo.Font = Enum.Font.GothamBold
logo.ZIndex = 8
logo.Parent = logoBox

--// TÍTULO (agora é S4G4XZ MENU)
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -280, 0, 26)
title.Position = UDim2.new(0, 168, 0, 16)
title.BackgroundTransparency = 1
title.Text = "S4G4XZ MENU"
title.TextColor3 = COR_TEXTO
title.TextSize = 24
title.Font = Enum.Font.GothamBlack
title.TextXAlignment = Enum.TextXAlignment.Left
title.ZIndex = 6
title.Parent = topbar

local titleGrad = Instance.new("UIGradient")
titleGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(200, 160, 255)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(120, 200, 255)),
})
titleGrad.Parent = title

local subtitle = Instance.new("TextLabel")
subtitle.Size = UDim2.new(1, -280, 0, 18)
subtitle.Position = UDim2.new(0, 168, 0, 42)
subtitle.BackgroundTransparency = 1
subtitle.Text = "PREMIUM   •   v4   •   EDITION"
subtitle.TextColor3 = Color3.fromRGB(170, 160, 210)
subtitle.TextSize = 12
subtitle.Font = Enum.Font.GothamBold
subtitle.TextXAlignment = Enum.TextXAlignment.Left
subtitle.ZIndex = 6
subtitle.Parent = topbar

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 44, 0, 44)
closeBtn.Position = UDim2.new(1, -60, 0.5, -22)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 70, 90)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 20
closeBtn.Font = Enum.Font.GothamBold
closeBtn.BorderSizePixel = 0
closeBtn.AutoButtonColor = false
closeBtn.ZIndex = 7
closeBtn.Parent = topbar
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 12)

local closeStroke = Instance.new("UIStroke")
closeStroke.Color = Color3.fromRGB(255, 150, 170)
closeStroke.Thickness = 1.5
closeStroke.Transparency = 0.4
closeStroke.Parent = closeBtn

local closeGlow = Instance.new("Frame")
closeGlow.Size = UDim2.new(1, 10, 1, 10)
closeGlow.Position = UDim2.new(0.5, -27, 0.5, -27)
closeGlow.BackgroundColor3 = Color3.fromRGB(255, 70, 90)
closeGlow.BackgroundTransparency = 0.75
closeGlow.BorderSizePixel = 0
closeGlow.ZIndex = 6
closeGlow.Parent = closeBtn
Instance.new("UICorner", closeGlow).CornerRadius = UDim.new(1, 0)

closeBtn.MouseEnter:Connect(function()
	TweenService:Create(closeBtn, TweenInfo.new(0.15), {
		BackgroundColor3 = Color3.fromRGB(255, 120, 140),
		Size = UDim2.new(0, 46, 0, 46),
		Position = UDim2.new(1, -61, 0.5, -23)
	}):Play()
end)
closeBtn.MouseLeave:Connect(function()
	TweenService:Create(closeBtn, TweenInfo.new(0.15), {
		BackgroundColor3 = Color3.fromRGB(255, 70, 90),
		Size = UDim2.new(0, 44, 0, 44),
		Position = UDim2.new(1, -60, 0.5, -22)
	}):Play()
end)
closeBtn.MouseButton1Click:Connect(function()
	local t1 = TweenService:Create(main, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
		Size = UDim2.new(0, 0, 0, 0),
		BackgroundTransparency = 1
	})
	local t2 = TweenService:Create(shadow, TweenInfo.new(0.3), {ImageTransparency = 1})
	t1:Play(); t2:Play()
	t1.Completed:Connect(function() gui:Destroy() end)
end)

local infoPanel = Instance.new("Frame")
infoPanel.Name = "InfoPanel"
infoPanel.Size = UDim2.new(1, -70, 1, -140)
infoPanel.Position = UDim2.new(0, 35, 0, 96)
infoPanel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
infoPanel.BorderSizePixel = 0
infoPanel.ZIndex = 4
infoPanel.ClipsDescendants = true
infoPanel.Parent = main

local infoCorner = Instance.new("UICorner")
infoCorner.CornerRadius = UDim.new(0, 22)
infoCorner.Parent = infoPanel

local infoStroke = Instance.new("UIStroke")
infoStroke.Color = COR_ACENTO
infoStroke.Thickness = 2
infoStroke.Transparency = 0.4
infoStroke.Parent = infoPanel

local infoBgGrad = Instance.new("UIGradient")
infoBgGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(245, 240, 255)),
})
infoBgGrad.Rotation = 90
infoBgGrad.Parent = infoPanel

local infoReflex = Instance.new("Frame")
infoReflex.Size = UDim2.new(1, -60, 0, 1)
infoReflex.Position = UDim2.new(0, 30, 0, 1)
infoReflex.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
infoReflex.BackgroundTransparency = 0.2
infoReflex.BorderSizePixel = 0
infoReflex.ZIndex = 6
infoReflex.Parent = infoPanel

local sidebarAccent = Instance.new("Frame")
sidebarAccent.Size = UDim2.new(0, 6, 1, -40)
sidebarAccent.Position = UDim2.new(0, 0, 0, 20)
sidebarAccent.BackgroundColor3 = COR_ACENTO
sidebarAccent.BorderSizePixel = 0
sidebarAccent.ZIndex = 6
sidebarAccent.Parent = infoPanel

local accentCorner = Instance.new("UICorner")
accentCorner.CornerRadius = UDim.new(1, 0)
accentCorner.Parent = sidebarAccent

local accentGrad = Instance.new("UIGradient")
accentGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0,   COR_ACENTO),
	ColorSequenceKeypoint.new(0.5, COR_ACENTO_2),
	ColorSequenceKeypoint.new(1,   COR_ACENTO_3),
})
accentGrad.Rotation = 90
accentGrad.Parent = sidebarAccent

local header = Instance.new("TextLabel")
header.Size = UDim2.new(1, -80, 0, 40)
header.Position = UDim2.new(0, 40, 0, 22)
header.BackgroundTransparency = 1
header.Text = "📌  INFORMAÇÕES"
header.TextColor3 = Color3.fromRGB(50, 30, 90)
header.TextSize = 26
header.Font = Enum.Font.GothamBlack
header.TextXAlignment = Enum.TextXAlignment.Left
header.ZIndex = 7
header.Parent = infoPanel

local headerGrad = Instance.new("UIGradient")
headerGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(120, 60, 220)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(60, 150, 240)),
})
headerGrad.Parent = header

local headerLine = Instance.new("Frame")
headerLine.Size = UDim2.new(1, -80, 0, 2)
headerLine.Position = UDim2.new(0, 40, 0, 64)
headerLine.BackgroundColor3 = Color3.fromRGB(220, 210, 255)
headerLine.BorderSizePixel = 0
headerLine.ZIndex = 7
headerLine.Parent = infoPanel

local lineGrad = Instance.new("UIGradient")
lineGrad.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, COR_ACENTO),
	ColorSequenceKeypoint.new(0.5, COR_ACENTO_2),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 255, 255)),
})
lineGrad.Parent = headerLine

local infoLabel = Instance.new("TextLabel")
infoLabel.Name = "InfoLabel"
infoLabel.Size = UDim2.new(1, -70, 1, -100)
infoLabel.Position = UDim2.new(0, 40, 0, 82)
infoLabel.BackgroundTransparency = 1
infoLabel.Text = ""
infoLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
infoLabel.TextSize = 24
infoLabel.Font = Enum.Font.GothamBold
infoLabel.TextXAlignment = Enum.TextXAlignment.Left
infoLabel.TextYAlignment = Enum.TextYAlignment.Top
infoLabel.TextWrapped = true
infoLabel.RichText = true
infoLabel.ZIndex = 7
infoLabel.Parent = infoPanel

local ttkBtn = Instance.new("TextButton")
ttkBtn.Name = "TikTokLink"
ttkBtn.BackgroundTransparency = 1
ttkBtn.Text = "TKK :  @_s4gxztrash"
ttkBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ttkBtn.TextSize = 24
ttkBtn.Font = Enum.Font.GothamBold
ttkBtn.TextXAlignment = Enum.TextXAlignment.Left
ttkBtn.TextYAlignment = Enum.TextYAlignment.Top
ttkBtn.AutoButtonColor = false
ttkBtn.ZIndex = 8
ttkBtn.Size = UDim2.new(0, 320, 0, 32)
ttkBtn.Position = UDim2.new(0, 40, 0, 315)
ttkBtn.Parent = infoPanel

ttkBtn.MouseEnter:Connect(function()
	TweenService:Create(ttkBtn, TweenInfo.new(0.15), {TextTransparency = 0.15}):Play()
end)
ttkBtn.MouseLeave:Connect(function()
	TweenService:Create(ttkBtn, TweenInfo.new(0.15), {TextTransparency = 0}):Play()
end)

ttkBtn.MouseButton1Click:Connect(function()
	local ok = pcall(function()
		game:GetService("GuiService"):OpenBrowserWindow(TIKTOK_URL)
	end)
	if not ok then
		pcall(function()
			if setclipboard then setclipboard(TIKTOK_URL) end
		end)
	end
end)

local function criarBotaoContorno(texto, corBase)
	local container = Instance.new("Frame")
	container.BackgroundTransparency = 1
	container.Size = UDim2.new(0, 110, 0, 36)
	container.ZIndex = 9
	container.Parent = infoPanel

	local fundo = Instance.new("Frame")
	fundo.Size = UDim2.new(1, 0, 1, 0)
	fundo.BackgroundTransparency = 1
	fundo.BorderSizePixel = 0
	fundo.ZIndex = 9
	fundo.Parent = container

	local fundoCorner = Instance.new("UICorner")
	fundoCorner.CornerRadius = UDim.new(1, 0)
	fundoCorner.Parent = fundo

	local borda = Instance.new("UIStroke")
	borda.Thickness = 2.5
	borda.Transparency = 0
	borda.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
	borda.Parent = fundo

	local bordaGrad = Instance.new("UIGradient")
	bordaGrad.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0,    corBase),
		ColorSequenceKeypoint.new(0.25, COR_ACENTO),
		ColorSequenceKeypoint.new(0.5,  COR_ACENTO_2),
		ColorSequenceKeypoint.new(0.75, COR_ACENTO_3),
		ColorSequenceKeypoint.new(1,    corBase),
	})
	bordaGrad.Parent = borda

	local btn = Instance.new("TextButton")
	btn.BackgroundTransparency = 1
	btn.Size = UDim2.new(1, 0, 1, 0)
	btn.Text = texto
	btn.TextColor3 = corBase
	btn.TextSize = 22
	btn.Font = Enum.Font.GothamBold
	btn.TextXAlignment = Enum.TextXAlignment.Center
	btn.TextYAlignment = Enum.TextYAlignment.Center
	btn.AutoButtonColor = false
	btn.ZIndex = 10
	btn.Parent = container

	local txtGrad = Instance.new("UIGradient")
	txtGrad.Color = ColorSequence.new({
		ColorSequenceKeypoint.new(0, corBase),
		ColorSequenceKeypoint.new(1, corBase),
	})
	txtGrad.Parent = btn

	return btn, txtGrad, corBase, container, borda, bordaGrad
end

local btnSair, txtGradSair, corSair, contSair, bordaSair, bordaGradSair =
	criarBotaoContorno("Sair", Color3.fromRGB(255, 60, 60))
contSair.Position = UDim2.new(0, 370, 0, 315)

local btnEntrar, txtGradEntrar, corEntrar, contEntrar, bordaEntrar, bordaGradEntrar =
	criarBotaoContorno("Entrar", Color3.fromRGB(60, 220, 90))
contEntrar.Position = UDim2.new(0, 500, 0, 315)

local tGrad = 0
local gradConn = RunService.RenderStepped:Connect(function(dt)
	tGrad = (tGrad + dt * 60) % 360
	bordaGradSair.Rotation   = tGrad
	bordaGradEntrar.Rotation = tGrad + 90
end)

btnSair.MouseEnter:Connect(function()
	TweenService:Create(btnSair, TweenInfo.new(0.15), {TextTransparency = 0.15}):Play()
	TweenService:Create(bordaSair, TweenInfo.new(0.15), {Thickness = 3.5}):Play()
end)
btnSair.MouseLeave:Connect(function()
	TweenService:Create(btnSair, TweenInfo.new(0.15), {TextTransparency = 0}):Play()
	TweenService:Create(bordaSair, TweenInfo.new(0.15), {Thickness = 2.5}):Play()
end)

btnEntrar.MouseEnter:Connect(function()
	TweenService:Create(btnEntrar, TweenInfo.new(0.15), {TextTransparency = 0.15}):Play()
	TweenService:Create(bordaEntrar, TweenInfo.new(0.15), {Thickness = 3.5}):Play()
end)
btnEntrar.MouseLeave:Connect(function()
	TweenService:Create(btnEntrar, TweenInfo.new(0.15), {TextTransparency = 0}):Play()
	TweenService:Create(bordaEntrar, TweenInfo.new(0.15), {Thickness = 2.5}):Play()
end)

-- =========================================================
-- TELA "PAINEL ACTIVADO" — VERSÃO ANTERIOR (rainbow colorido + ondas) mais lento
-- =========================================================
local function mostrarTelaAtivado()
	local tela = Instance.new("ScreenGui")
	tela.Name = "AegisAtivado"
	tela.ResetOnSpawn = false
	tela.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	tela.IgnoreGuiInset = true
	tela.DisplayOrder = 999
	tela.Parent = playerGui

	local fundo = Instance.new("Frame")
	fundo.Size = UDim2.new(1, 0, 1, 0)
	fundo.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
	fundo.BackgroundTransparency = 1
	fundo.BorderSizePixel = 0
	fundo.ZIndex = 1
	fundo.Parent = tela

	TweenService:Create(fundo, TweenInfo.new(0.4), {BackgroundTransparency = 0.25}):Play()

	local centro = Instance.new("Frame")
	centro.AnchorPoint = Vector2.new(0.5, 0.5)
	centro.Size = UDim2.new(0, 900, 0, 240)
	centro.Position = UDim2.new(0.5, 0, 0.5, 0)
	centro.BackgroundTransparency = 1
	centro.ZIndex = 5
	centro.Parent = fundo

	local LINHA1 = "PAINEL ACTIVADO..."
	local LINHA2 = "TIKTOK : @_s4gxztrash"

	local letras1 = {}
	local letras2 = {}

	local function criarLinha(texto, yPos, tamanho, storeTable)
		local charW = tamanho * 0.65
		local totalW = charW * #texto
		local startX = (900 - totalW) / 2

		for i = 1, #texto do
			local ch = texto:sub(i, i)
			local lbl = Instance.new("TextLabel")
			lbl.Size = UDim2.new(0, charW + 6, 0, tamanho + 14)
			lbl.Position = UDim2.new(0, startX + (i - 1) * charW, 0, yPos)
			lbl.BackgroundTransparency = 1
			lbl.Text = ch
			lbl.TextColor3 = Color3.fromRGB(255, 255, 255)
			lbl.TextSize = tamanho
			lbl.Font = Enum.Font.GothamBold
			lbl.TextXAlignment = Enum.TextXAlignment.Center
			lbl.TextYAlignment = Enum.TextYAlignment.Center
			lbl.ZIndex = 6

			local stroke = Instance.new("UIStroke")
			stroke.Thickness = 3
			stroke.Color = Color3.fromRGB(0, 0, 0)
			stroke.Transparency = 0.3
			stroke.Parent = lbl

			lbl.Parent = centro
			table.insert(storeTable, { label = lbl, index = i, total = #texto, baseY = yPos })
		end
	end

	criarLinha(LINHA1, 20, 52, letras1)
	criarLinha(LINHA2, 130, 42, letras2)

	-- FADE IN suave
	for _, l in ipairs(letras1) do
		l.label.TextTransparency = 1
		TweenService:Create(l.label, TweenInfo.new(0.4), {TextTransparency = 0}):Play()
	end
	for _, l in ipairs(letras2) do
		l.label.TextTransparency = 1
		TweenService:Create(l.label, TweenInfo.new(0.5), {TextTransparency = 0}):Play()
	end

	-- 🌈 RAINBOW COLORIDO (volta ao anterior, um pouco mais lento)
	task.spawn(function()
		local hue = 0
		while centro.Parent do
			hue = (hue + 0.009) % 1  -- mais lento que 0.015
			for _, l in ipairs(letras1) do
				if l.label.Parent then
					local h = (hue + l.index * 0.04) % 1
					l.label.TextColor3 = Color3.fromHSV(h, 0.9, 1)
				end
			end
			for _, l in ipairs(letras2) do
				if l.label.Parent then
					local h = (hue + 0.35 + l.index * 0.04) % 1
					l.label.TextColor3 = Color3.fromHSV(h, 0.9, 1)
				end
			end
			RunService.RenderStepped:Wait()
		end
	end)

	-- 🌊 ONDAS (mais suaves/devagar)
	task.spawn(function()
		local t = 0
		while centro.Parent do
			t = t + 0.025  -- era 0.05, agora 0.025 (metade da velocidade)
			for _, l in ipairs(letras1) do
				if l.label.Parent then
					local yOff = math.sin(t * 1.5 + l.index * 0.5) * 12
					l.label.Position = UDim2.new(
						l.label.Position.X.Scale,
						l.label.Position.X.Offset,
						0,
						l.baseY + yOff
					)
				end
			end
			for _, l in ipairs(letras2) do
				if l.label.Parent then
					local yOff = math.sin(t * 1.5 + l.index * 0.5 + 1) * 10
					l.label.Position = UDim2.new(
						l.label.Position.X.Scale,
						l.label.Position.X.Offset,
						0,
						l.baseY + yOff
					)
				end
			end
			RunService.RenderStepped:Wait()
		end
	end)

	-- APÓS 2 SEGUNDOS: fade out
	task.delay(2, function()
		TweenService:Create(fundo, TweenInfo.new(0.5), {BackgroundTransparency = 1}):Play()
		for _, l in ipairs(letras1) do
			TweenService:Create(l.label, TweenInfo.new(0.5), {TextTransparency = 1}):Play()
		end
		for _, l in ipairs(letras2) do
			TweenService:Create(l.label, TweenInfo.new(0.5), {TextTransparency = 1}):Play()
		end
		task.wait(0.55)
		tela:Destroy()
	end)
end

-- =========================================================
-- SCRIPT INTERNO
-- =========================================================
local scriptInternoAtivo = false
local conexoesInternas = {}
local scriptAtivoRef = { valor = true }

local function iniciarScriptInterno()
	if scriptInternoAtivo then return end
	scriptInternoAtivo = true

	local TeleportService = game:GetService("TeleportService")
	local plr = player

	local HEADLESS_MESH_ID    = "rbxassetid://1095708"
	local KORBLOX_MESH_ID     = "rbxassetid://101851696"
	local KORBLOX_TEXTURE_ID  = "rbxassetid://101851254"

	local headlessAtivo = false
	local korbloxAtivo  = false
	local headBackup    = nil
	local backupFeito   = false
	local scriptAtivo   = true
	scriptAtivoRef.valor = true

	local function fazerBackupCabeca(character)
		if backupFeito then return end
		local head = character:FindFirstChild("Head")
		if not head then return end
		headBackup = {
			transparency = head.Transparency,
			localTransparency = head.LocalTransparencyModifier,
			canCollide = head.CanCollide,
			decals = {},
			meshes = {},
		}
		for _, v in ipairs(head:GetChildren()) do
			if v:IsA("Decal") then
				table.insert(headBackup.decals, {
					name = v.Name, texture = v.Texture, face = v.Face,
					transparency = v.Transparency, color3 = v.Color3,
				})
			elseif v:IsA("SpecialMesh") then
				table.insert(headBackup.meshes, {
					name = v.Name, meshType = v.MeshType, meshId = v.MeshId,
					textureId = v.TextureId, scale = v.Scale, offset = v.Offset,
				})
			end
		end
		backupFeito = true
	end

	local function applyHeadless(character)
		local head = character:FindFirstChild("Head")
		if not head then return end
		fazerBackupCabeca(character)
		local antiga = head:FindFirstChild("HeadlessMesh")
		if antiga then antiga:Destroy() end
		for _, v in ipairs(head:GetChildren()) do
			if v:IsA("SpecialMesh") and v.Name ~= "HeadlessMesh" then
				v.Scale = Vector3.new(0.001, 0.001, 0.001)
			end
		end
		local mesh = Instance.new("SpecialMesh")
		mesh.Name = "HeadlessMesh"
		mesh.MeshType = Enum.MeshType.FileMesh
		mesh.MeshId = HEADLESS_MESH_ID
		mesh.Scale = Vector3.new(0.001, 0.001, 0.001)
		mesh.Parent = head
	end

	local function removeHeadless(character)
		local head = character:FindFirstChild("Head")
		if not head then return end
		local mesh = head:FindFirstChild("HeadlessMesh")
		if mesh then mesh:Destroy() end
		if headBackup then
			for _, v in ipairs(head:GetChildren()) do
				if v:IsA("SpecialMesh") then
					for _, m in ipairs(headBackup.meshes) do
						if v.Name == m.name then
							v.Scale = m.scale
							v.MeshId = m.meshId
							v.TextureId = m.textureId
							v.MeshType = m.meshType
							v.Offset = m.offset
						end
					end
				end
			end
			head.Transparency = headBackup.transparency
			head.LocalTransparencyModifier = headBackup.localTransparency
			head.CanCollide = headBackup.canCollide
			for _, v in ipairs(head:GetChildren()) do
				if v:IsA("Decal") then
					for _, d in ipairs(headBackup.decals) do
						if v.Name == d.name then
							v.Texture = d.texture
							v.Face = d.face
							v.Transparency = d.transparency
							v.Color3 = d.color3
						end
					end
				end
			end
		end
	end

	local function applyKorblox(character)
		local rightLeg = character:FindFirstChild("Right Leg")
		if not rightLeg then return end
		local antiga = rightLeg:FindFirstChild("KorbloxMesh")
		if antiga then antiga:Destroy() end
		for _, v in ipairs(rightLeg:GetChildren()) do
			if v:IsA("SpecialMesh") or v:IsA("CharacterMesh") then
				v:Destroy()
			end
		end
		local mesh = Instance.new("SpecialMesh")
		mesh.Name = "KorbloxMesh"
		mesh.MeshType = Enum.MeshType.FileMesh
		mesh.MeshId = KORBLOX_MESH_ID
		mesh.TextureId = KORBLOX_TEXTURE_ID
		mesh.Scale = Vector3.new(1, 1, 1)
		mesh.Parent = rightLeg
	end

	local function removeKorblox(character)
		local rightLeg = character:FindFirstChild("Right Leg")
		if not rightLeg then return end
		local mesh = rightLeg:FindFirstChild("KorbloxMesh")
		if mesh then mesh:Destroy() end
	end

	local function rejoin()
		local placeId = game.PlaceId
		local jobId   = game.JobId
		if jobId and jobId ~= "" then
			TeleportService:TeleportToPlaceInstance(placeId, jobId, plr)
		else
			TeleportService:Teleport(placeId, plr)
		end
	end

	local connInput = UserInputService.InputBegan:Connect(function(input, gameProcessed)
		if not scriptAtivo then return end
		local ctrlDown = UserInputService:IsKeyDown(Enum.KeyCode.LeftControl)
			or UserInputService:IsKeyDown(Enum.KeyCode.RightControl)
		local altDown = UserInputService:IsKeyDown(Enum.KeyCode.LeftAlt)
			or UserInputService:IsKeyDown(Enum.KeyCode.RightAlt)

		if input.KeyCode == Enum.KeyCode.F and ctrlDown then
			scriptAtivo = false
			scriptAtivoRef.valor = false
			scriptInternoAtivo = false
			local char = plr.Character
			if char then
				if headlessAtivo then removeHeadless(char) end
				if korbloxAtivo then removeKorblox(char) end
			end
			for _, c in ipairs(conexoesInternas) do
				if c and c.Disconnect then c:Disconnect() end
			end
			return
		end

		if gameProcessed then return end

		if input.KeyCode == Enum.KeyCode.R and ctrlDown and altDown then
			rejoin()
			return
		end

		if not ctrlDown then return end

		if input.KeyCode == Enum.KeyCode.H then
			headlessAtivo = not headlessAtivo
			if plr.Character then
				if headlessAtivo then applyHeadless(plr.Character)
				else removeHeadless(plr.Character) end
			end
		end

		if input.KeyCode == Enum.KeyCode.K then
			korbloxAtivo = not korbloxAtivo
			if plr.Character then
				if korbloxAtivo then applyKorblox(plr.Character)
				else removeKorblox(plr.Character) end
			end
		end
	end)
	table.insert(conexoesInternas, connInput)

	local connRespawn = plr.CharacterAdded:Connect(function(character)
		task.wait(1)
		headBackup = nil
		backupFeito = false
		if not scriptAtivo then return end
		if headlessAtivo then applyHeadless(character) end
		if korbloxAtivo then applyKorblox(character) end
	end)
	table.insert(conexoesInternas, connRespawn)

	print("[S4G4XZ] Script interno iniciado (Ctrl+H / Ctrl+K / Ctrl+Alt+R / Ctrl+F)")
end

-- =========================================================
-- AÇÕES DOS BOTÕES
-- =========================================================
local function fecharTudo()
	scriptAtivoRef.valor = false
	scriptInternoAtivo = false
	for _, c in ipairs(conexoesInternas) do
		if c and c.Disconnect then c:Disconnect() end
	end
	conexoesInternas = {}

	local t1 = TweenService:Create(main, TweenInfo.new(0.3, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
		Size = UDim2.new(0, 0, 0, 0),
		BackgroundTransparency = 1
	})
	local t2 = TweenService:Create(shadow, TweenInfo.new(0.3), {ImageTransparency = 1})
	t1:Play(); t2:Play()
	t1.Completed:Connect(function() gui:Destroy() end)
end

btnEntrar.MouseButton1Click:Connect(function()
	iniciarScriptInterno()
	local t1 = TweenService:Create(main, TweenInfo.new(0.25, Enum.EasingStyle.Back, Enum.EasingDirection.In), {
		Size = UDim2.new(0, 0, 0, 0),
		BackgroundTransparency = 1
	})
	local t2 = TweenService:Create(shadow, TweenInfo.new(0.25), {ImageTransparency = 1})
	t1:Play(); t2:Play()
	t1.Completed:Connect(function()
		gui:Destroy()
		mostrarTelaAtivado()
	end)
end)

btnSair.MouseButton1Click:Connect(function()
	print("[S4G4XZ MENU] Sair clicado — encerrando script e fechando menu")
	fecharTudo()
end)

--// DADOS
local linhas = {
	{ "Ctrl + H",       "Liga/desliga Headless" },
	{ "Ctrl + K",       "Liga/desliga Korblox" },
	{ "Ctrl + Alt + R", "Rejoin no mesmo servidor" },
	{ "Ctrl + F",       "Destruir script + restaurar personagem" },
}

--// RAINBOW ANIMADO + COR DO BOTÃO TIKTOK
local hue = 0
local rainbowConn = RunService.RenderStepped:Connect(function(dt)
	hue = (hue + dt * 0.13) % 1
	local function hex(c)
		return string.format("%02X%02X%02X",
			math.floor(c.R * 255), math.floor(c.G * 255), math.floor(c.B * 255))
	end
	local out = {}
	local function linhaColorida(i, esquerda, direita)
		local h1 = (hue + i * 0.07) % 1
		local h2 = (hue + i * 0.07 + 0.45) % 1
		local h3 = (hue + i * 0.07 + 0.75) % 1
		local c1 = Color3.fromHSV(h1, 1, 0.85)
		local c2 = Color3.fromHSV(h2, 1, 0.85)
		local c3 = Color3.fromHSV(h3, 1, 0.85)
		return string.format(
			'<font color="#%s">%s</font>   <font color="#%s">→</font>   <font color="#%s">%s</font>',
			hex(c1), esquerda, hex(c2), hex(c3), direita
		)
	end
	out[#out + 1] = linhaColorida(0, "Tecla", "Ação")
	out[#out + 1] = ""
	for i, l in ipairs(linhas) do
		out[#out + 1] = linhaColorida(i + 1, l[1], l[2])
	end
	infoLabel.Text = table.concat(out, "<br/>")
	ttkBtn.TextColor3 = Color3.fromHSV((hue + 0.55) % 1, 1, 0.85)
end)

--// RODAPÉ (agora S4G4XZ)
local footer = Instance.new("TextLabel")
footer.Size = UDim2.new(1, -70, 0, 22)
footer.Position = UDim2.new(0, 35, 1, -38)
footer.BackgroundTransparency = 1
footer.Text = "⚡  S4G4XZ MENU  •  desenvolvido por _s4gxztrash  •  todos os direitos reservados"
footer.TextColor3 = Color3.fromRGB(140, 140, 180)
footer.TextSize = 12
footer.Font = Enum.Font.GothamBold
footer.TextXAlignment = Enum.TextXAlignment.Left
footer.ZIndex = 6
footer.Parent = main

--// PARTÍCULAS FLUTUANTES
local function criarParticula()
	local p = Instance.new("Frame")
	p.Size = UDim2.new(0, math.random(3, 6), 0, math.random(3, 6))
	p.Position = UDim2.new(math.random(), 0, 1.05, 0)
	p.BackgroundColor3 = Color3.fromHSV(math.random(), 0.7, 1)
	p.BackgroundTransparency = 0.3
	p.BorderSizePixel = 0
	p.ZIndex = 3
	p.Parent = main
	Instance.new("UICorner", p).CornerRadius = UDim.new(1, 0)
	local duracao = math.random(4, 8)
	local tween = TweenService:Create(p, TweenInfo.new(duracao, Enum.EasingStyle.Linear), {
		Position = UDim2.new(math.random(), 0, -0.05, 0),
		BackgroundTransparency = 1
	})
	tween:Play()
	tween.Completed:Connect(function() p:Destroy() end)
end

local spawnerAtivo = true
task.spawn(function()
	while spawnerAtivo and main.Parent do
		criarParticula()
		task.wait(math.random(30, 80) / 100)
	end
end)

--// ANIMAÇÃO DE ENTRADA
main.Size = UDim2.new(0, 0, 0, 0)
main.BackgroundTransparency = 1
shadow.ImageTransparency = 1

TweenService:Create(main, TweenInfo.new(0.6, Enum.EasingStyle.Back, Enum.EasingDirection.Out), {
	Size = UDim2.new(0, 900, 0, 600),
	BackgroundTransparency = 0
}):Play()
TweenService:Create(shadow, TweenInfo.new(0.7), {ImageTransparency = 0.5}):Play()

--// PULSO NA BORDA DO MENU
task.spawn(function()
	while main.Parent do
		TweenService:Create(mainStroke, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
			Transparency = 0.5
		}):Play()
		task.wait(1.5)
		TweenService:Create(mainStroke, TweenInfo.new(1.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
			Transparency = 0.1
		}):Play()
		task.wait(1.5)
	end
end)

--// ROTAÇÃO DO GRADIENTE DE FUNDO
task.spawn(function()
	while main.Parent do
		for i = 0, 360, 2 do
			if not main.Parent then break end
			bgGrad.Rotation = i
			task.wait(0.05)
		end
	end
end)

--// SISTEMA DE ARRASTAR
local dragging, dragStart, startPos

topbar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
	or input.UserInputType == Enum.UserInputType.Touch then
		dragging   = true
		dragStart  = input.Position
		startPos   = main.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement
	or input.UserInputType == Enum.UserInputType.Touch) then
		local delta = input.Position - dragStart
		main.Position = UDim2.new(
			startPos.X.Scale, startPos.X.Offset + delta.X,
			startPos.Y.Scale, startPos.Y.Offset + delta.Y
		)
	end
end)

--// LIMPEZA
gui.Destroying:Connect(function()
	spawnerAtivo = false
	if rainbowConn then rainbowConn:Disconnect() end
	if gradConn then gradConn:Disconnect() end
end)

print("[S4G4XZ MENU] ULTRA carregado ✅")
