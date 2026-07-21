# demo-R6-face-roblox

```luau
local Players = game:GetService("Players")

-- ID de votre compte Roblox pour copier votre skin
local YOUR_USER_ID = 8273523301 -- ⚠️ Remplacez 1 par VOTRE UserId Roblox

-- Texture du visage classique que vous souhaitez appliquer
local FACE_TEXTURE_ID = "rbxassetid://22972635"

local function spawnR6ClassicNPC(userId, spawnPosition)
    -- 1. Récupérer la description du skin du joueur
    local success, description = pcall(function()
        return Players:GetHumanoidDescriptionFromUserId(userId)
    end)

    if not success or not description then
        warn("Impossible de charger le skin pour l'UserId: " .. tostring(userId))
        return
    end

    -- 2. Configuration pour forcer la tête classique R6
    description.Head = 0 -- Annule la tête 3D / Dynamic Head
    description.Face = 0 -- Annule le visage du Marketplace
    description.StaticFacialAnimation = true

    -- 3. Générer le modèle de personnage en R6
    local model = Players:CreateHumanoidModelFromDescription(description, Enum.HumanoidRigType.R6)
    model.Name = "ClassicR6NPC"

    -- Positionner le mannequin dans la carte
    model:PivotTo(CFrame.new(spawnPosition))

    -- 4. Nettoyer les éventuels Decals restants et appliquer votre texture sur la tête
    local head = model:WaitForChild("Head", 5)
    if head then
        for _, child in ipairs(head:GetChildren()) do
            if child:IsA("Decal") then
                child:Destroy()
            end
        end

        local faceDecal = Instance.new("Decal")
        faceDecal.Name = "face"
        faceDecal.Face = Enum.NormalId.Front
        faceDecal.Texture = FACE_TEXTURE_ID
        faceDecal.Parent = head
    end

    -- Placer le mannequin dans la Workspace
    model.Parent = workspace
end

-- Exemple d'apparition du mannequin au centre de la carte (0, 5, 0)
task.wait(2) -- Légère pause pour s'assurer que la partie est prête
spawnR6ClassicNPC(YOUR_USER_ID, Vector3.new(0, 5, 0))
```
