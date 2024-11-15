-- Accès à CoreGui et RunService
local CoreGui = game:GetService("CoreGui")
local RunService = game:GetService("RunService")  -- Pour la vérification périodique

-- Fonction pour ajuster la taille de menuSection en fonction du nombre d'enfants dans panelContainer
local function updateMenuSectionSize(panelContainer, menuSection)
    if not panelContainer or not menuSection then return end  -- Vérification des instances

    local numberOfChildren = #panelContainer:GetChildren()
    local correctWidth = numberOfChildren * 44
    if menuSection.Size.X.Offset ~= correctWidth then
        menuSection.Size = UDim2.new(0, correctWidth, 0, 44)
    end
end

-- Fonction pour surveiller périodiquement la taille de menuSection
local function monitorMenuSectionSize(panelContainer, menuSection)
    RunService.Heartbeat:Connect(function()
        if not panelContainer or not menuSection then
            return
        end
        updateMenuSectionSize(panelContainer, menuSection)
    end)
end

-- Fonction pour créer un bouton avec ID et icône spécifiques
local function createButton(uniqueId, iconAssetId)
    local topBarApp = CoreGui:FindFirstChild("TopBarApp", true)
    if not topBarApp then
        warn("TopBarApp introuvable dans CoreGui.")
        return
    end

    local unibarLeftFrame = topBarApp:FindFirstChild("UnibarLeftFrame", true)
    if not unibarLeftFrame then
        warn("UnibarLeftFrame introuvable.")
        return
    end

    local unibarMenu = unibarLeftFrame:FindFirstChild("UnibarMenu", true)
    if not unibarMenu then
        warn("UnibarMenu introuvable.")
        return
    end

    local menuSection = unibarMenu:FindFirstChild("2", true)
    if not menuSection then
        warn("Section '2' introuvable dans UnibarMenu.")
        return
    end

    local panelContainer = menuSection:FindFirstChild("3", true)
    if not panelContainer then
        warn("Section '3' introuvable dans Section '2'.")
        return
    end

    -- Création du bouton avec l'ID unique
    local button = Instance.new("TextButton")
    button.Name = uniqueId .. "Panel"  -- Nom basé sur l'ID
    button.Position = UDim2.new(0, #panelContainer:GetChildren() * 44, 0, 0)
    button.Size = UDim2.new(0, 44, 0, 44)
    button.Selectable = true
    button.Text = ""  -- Pas de texte
    button.Visible = true
    button.BackgroundTransparency = 1
    button.Parent = panelContainer

    button.Selected = false

    -- Création de l'élément "Highlight"
    local highlight = Instance.new("Frame")
    highlight.Name = "Highlight"
    highlight.BackgroundColor3 = Color3.fromRGB(163, 162, 165)
    highlight.BorderColor3 = Color3.fromRGB(27, 42, 53)
    highlight.Size = UDim2.new(0, 36, 0, 36)
    highlight.Position = UDim2.new(0.5, 0, 0.5, 0)
    highlight.BackgroundTransparency = 0.9
    highlight.AnchorPoint = Vector2.new(0.5, 0.5)
    highlight.Visible = false
    highlight.Parent = button

    local uiCorner = Instance.new("UICorner")
    uiCorner.CornerRadius = UDim.new(0.5, 0)
    uiCorner.Parent = highlight

    -- Création de l'élément "Icon"
    local icon = Instance.new("ImageLabel")
    icon.Name = "Icon"
    icon.Image = iconAssetId  -- Utilise l'icône spécifique
    icon.Size = UDim2.new(0, 22, 0, 22)
    icon.Position = UDim2.new(0.5, -11, 0.5, -11)
    icon.BackgroundTransparency = 1
    icon.Visible = true
    icon.Parent = button

    -- Comportement de survol et de sélection
    button.MouseEnter:Connect(function()
        highlight.Visible = true
    end)
    button.MouseLeave:Connect(function()
        highlight.Visible = false
    end)
    button.MouseButton1Click:Connect(function()
        if button.Selected then
            icon.ImageColor3 = Color3.fromRGB(255, 255, 0)
        else
            icon.ImageColor3 = Color3.fromRGB(255, 255, 255)
        end
        button.Selected = not button.Selected
    end)

    updateMenuSectionSize(panelContainer, menuSection)
    monitorMenuSectionSize(panelContainer, menuSection)
end

-- Fonction pour supprimer le bouton avec un ID spécifique
local function deleteButton(uniqueId)
    local topBarApp = CoreGui:FindFirstChild("TopBarApp", true)
    if topBarApp then
        local unibarLeftFrame = topBarApp:FindFirstChild("UnibarLeftFrame", true)
        if unibarLeftFrame then
            local stackedElements = unibarLeftFrame:FindFirstChild("StackedElements", true)
            if stackedElements then
                local button = stackedElements:FindFirstChild(uniqueId .. "Panel", true)
                if button then
                    button:Destroy()
                    print(uniqueId .. " panel a été détruit.")
                end
            end
        end
    end
end

-- Exécuter en fonction de l'état du pin
return function(uniqueId, iconAssetId, isPinned)
    if isPinned then
        createButton(uniqueId, iconAssetId)  -- Crée le bouton avec l'icône et l'ID spécifiés
    else
        deleteButton(uniqueId)  -- Supprime le bouton correspondant
    end
end
