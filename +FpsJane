--============================================================--
--        BLOX FRUITS - MINECRAFT MODE 2.0                  --
--        ULTRA LOW GRAPHICS / LOW CPU OVERHEAD              --
--        LOCAL CLIENT ONLY                                  --
--============================================================--

local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local Players = game:GetService("Players")
local Terrain = Workspace:FindFirstChildOfClass("Terrain")

--============================================================--
-- CONFIGURAÇÃO EXTREMA
--============================================================--

local REMOVE_TEXTURES = true
local REMOVE_DECALS = true
local REMOVE_PARTICLES = true
local REMOVE_BEAMS = true
local REMOVE_TRAILS = true
local REMOVE_LIGHTS = true
local REMOVE_HIGHLIGHTS = true
local REMOVE_POSTFX = true
local REMOVE_SHADOWS = true
local SIMPLE_MATERIAL = true

--============================================================--
-- LIGHTING
--============================================================--

pcall(function()
    Lighting.GlobalShadows = false
    Lighting.Brightness = 0
    Lighting.EnvironmentDiffuseScale = 0
    Lighting.EnvironmentSpecularScale = 0
    Lighting.FogEnd = 1000000
end)

-- Remove pós processamento
for _, obj in ipairs(Lighting:GetChildren()) do
    if obj:IsA("PostEffect") then
        pcall(function()
            obj.Enabled = false
        end)
    end
end

-- Pega efeitos adicionados posteriormente
Lighting.ChildAdded:Connect(function(obj)
    if obj:IsA("PostEffect") then
        pcall(function()
            obj.Enabled = false
        end)
    end
end)

--============================================================--
-- TERRAIN
--============================================================--

if Terrain then
    pcall(function()
        Terrain.WaterWaveSize = 0
        Terrain.WaterWaveSpeed = 0
        Terrain.WaterReflectance = 0
        Terrain.WaterTransparency = 1
    end)
end

--============================================================--
-- OTIMIZAÇÃO INDIVIDUAL
--============================================================--

local function optimize(obj)

    -- TEXTURAS
    if REMOVE_TEXTURES and obj:IsA("Texture") then
        pcall(function()
            obj.Transparency = 1
        end)
        return
    end

    -- DECALS
    if REMOVE_DECALS and obj:IsA("Decal") then
        pcall(function()
            obj.Transparency = 1
        end)
        return
    end

    -- PARTICLES
    if REMOVE_PARTICLES and obj:IsA("ParticleEmitter") then
        pcall(function()
            obj.Enabled = false
            obj.Rate = 0
        end)
        return
    end

    -- TRAILS
    if REMOVE_TRAILS and obj:IsA("Trail") then
        pcall(function()
            obj.Enabled = false
        end)
        return
    end

    -- BEAMS
    if REMOVE_BEAMS and obj:IsA("Beam") then
        pcall(function()
            obj.Enabled = false
        end)
        return
    end

    -- LIGHTS
    if REMOVE_LIGHTS then
        if obj:IsA("PointLight")
        or obj:IsA("SpotLight")
        or obj:IsA("SurfaceLight") then

            pcall(function()
                obj.Enabled = false
                obj.Shadows = false
            end)

            return
        end
    end

    -- HIGHLIGHTS
    if REMOVE_HIGHLIGHTS and obj:IsA("Highlight") then
        pcall(function()
            obj.Enabled = false
        end)
        return
    end

    -- PARTES
    if obj:IsA("BasePart") then
        pcall(function()

            if REMOVE_SHADOWS then
                obj.CastShadow = false
            end

            obj.Reflectance = 0

            if SIMPLE_MATERIAL then
                obj.Material = Enum.Material.SmoothPlastic
            end

        end)
    end
end

--============================================================--
-- PROCESSAMENTO INICIAL
--============================================================--

-- Em vez de fazer tudo de uma vez,
-- processamos em pequenos lotes para evitar congelamento.

task.spawn(function()

    local objects = Workspace:GetDescendants()
    local batch = 150

    for i = 1, #objects, batch do

        local finish = math.min(i + batch - 1, #objects)

        for n = i, finish do
            optimize(objects[n])
        end

        task.wait()
    end

end)

--============================================================--
-- NOVOS OBJETOS
--============================================================--

Workspace.DescendantAdded:Connect(function(obj)

    task.defer(function()
        optimize(obj)
    end)

end)

--============================================================--
-- PERSONAGENS
--============================================================--

local function optimizeCharacter(character)

    for _, obj in ipairs(character:GetDescendants()) do
        optimize(obj)
    end

    character.DescendantAdded:Connect(function(obj)

        task.defer(function()
            optimize(obj)
        end)

    end)

end

for _, player in ipairs(Players:GetPlayers()) do

    if player.Character then
        optimizeCharacter(player.Character)
    end

    player.CharacterAdded:Connect(function(character)
        optimizeCharacter(character)
    end)

end

Players.PlayerAdded:Connect(function(player)

    player.CharacterAdded:Connect(function(character)
        optimizeCharacter(character)
    end)

end)

--============================================================--
-- QUALIDADE MÍNIMA
--============================================================--

pcall(function()
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
end)

print("==============================================")
print("      BLOX FRUITS MINECRAFT MODE 2.0")
print("==============================================")
print("LOW GRAPHICS")
print("LOW EFFECTS")
print("LOW SHADOWS")
print("LOW REFLECTIONS")
print("LOW PARTICLES")
print("LOW POST PROCESSING")
print("CPU FRIENDLY CLEANER")
print("==============================================")
