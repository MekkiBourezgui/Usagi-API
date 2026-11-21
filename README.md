# Usagi API {Roblox Studio}
(l'icon n'est pas fait par moi, je l'ai juste trouvé sur google et je l'ai pris)
<img width="736" height="736" alt="UsagiAPI Icon" src="https://github.com/user-attachments/assets/f49ad447-6e3d-49db-9a92-9d166e0ec7b2" />

Usagi API est un ModuleScript qui permet de faciliter les taches avec des fonctions intégrées.
Avec Usagi API, vous pourriez faire plusieurs de choses comme construire des hitbox, ajouter des tags à un objet, et d'autres fonctions que vous allez voir après.

## L'installation du API
[Usagi API](https://github.com/MekkiBourezgui/Projets-Lua/blob/main/UsagiAPI.rbxm)

Vous installez le fichier ensuite l'importer dans **ReplicatedStorage**.
Pour l'utiliser dans vos programmes, faudra juste faire ça : 
```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local usagiAPI = require(ReplicatedStorage:WaitForChild("UsagiAPI"))
```
## L'usage du API

### Hitbox
**Créer un hitbox**
```lua
local HitboxAPI = usagiAPI.Hitbox
local Hitbox = HitboxAPI.new({
        Size = Vector3,
        CFrame = CFrame,
        ignoreList={Instance|any},
        DoVisualize=boolean,
        Target="Humanoid"|"BasePart"|"All",
    })
```
Size - taille du Hitbox

CFrame - la position et rotation du Hitbox

ignoreList - la list des objets pour que le Hitbox ignore.

DoVisualize - pour visualiser le hitbox.

Target - si le target doit être un objet BasePart ou les personnages humanoid.
Exemple : 
```lua
local Hitbox = HitboxAPI.new({
        Size = Vector3.new(5,5,5),
        CFrame = humRoot.CFrame * CFrame.new(0,0,-3),
        ignoreList={Character},
        DoVisualize=false,
        Target="Humanoid",
})
```
Cette exemple permet de créer un hitbox devant le personnage pour pouvoir après détecter les autre personnages dans ce hitbox.


**Casting**
```lua
Hitbox:Start(callBack, TimeLength)
```
callBack - fonction que le Hitbox devra utiliser lorsqu'elle detecte un objet dans le hitbox.

Timelength - la durée du Hitbox casting. (elle se détruit automatiquement après cette durée)

Exemple
```lua
-- D'après l'exemple précédent
Hitbox:Start(function(Target)
        local TargetHumanoid = Target:FindFirstChild("Humanoid")
        if TargetHumanoid then
           TargetHumanoid:TakeDamage(12) 
        end
end, 0.5)
```
Le cast dure 0.5 secondes et lorsqu'elle detecte un personnage dans le hitbox, ce personnage prend 12 dégâts.


**Pause**
```lua
Hitbox:Pause()
```
Cette fonction nous permet de mettre le hitbox en pause.

**Set Size**
```lua
Hitbox:SetSize(Size)
```
Cette fonction permet de changer la taille du hitbox

Exemple
```lua
Hitbox:SetSize(Vector3.new(10, 30, 20))
```


**Set CFrame**
```lua
Hitbox:SetCFrame(CFrame)
```
Cette fonction permet de changer 
Exemple
```lua
Hitbox:SetCFrame(CFrame.new(humRoot.Position) * CFrame.Angles(math.rad(90), 0, 0))
```

**Destroy**
```lua
Hitbox:Destroy()
```
Cette fonction detruit le hitbox. (ce n'est pas la peine d'utiliser Destroy si vous mettiez le temps de durée.)



### Instances
**Créer un instance**
```lua
local InstanceAPI = usagiAPI.Instance
local instance = InstanceAPI.new({
        Instance = ClassName,
        Properties = {
            Property=value,
            ...
        },
        LifeTime=Lifetime,
        DoPooling=boolean,
    })
```
ClassName - Part, Beam, Trail, MeshPart, etc. tout les instances disponibles sur Roblox.

Properties - les propriétés de l'instance.

LifeTime - la durée de vie de l'instance.

DoPooling - mettre l'objet dans le fichier "Cache", et le reprendre lorsque c'est demandé. C'est très util pour l'optimization. Souvent utilisé pour les projectiles et les effets visuelles.

Exemple : 
```lua
local Part = InstanceAPI.new({
        Instance = ClassName,
        Properties = {
            Size = Vector3.new(2.5, 2.5, 5),
            Anchored=true,
            Material = Enum.Material.Neon,
            Position = Vector3.new(0, 3, 0),
            Name = "TestPart"
        },
        LifeTime=Lifetime,
        DoPooling=false
    })
```

**Set Properties**
```lua
local newProperties={
    Property=value,
    ...
}
Instance:SetProperties(newProperties)
```
Mettre des nouvelles propriétés dans l'objet

Exemple: 
```lua
Part:SetProperties({
        Size = Vector3.new(3,3,3),
        CFrame=Part.Instance.CFrame * CFrame.new(0,0,-4),
        Transparency = 0.8,
    })
```

**Get Properties**
```lua
local InstanceProperties = Instance:GetProperties()
```
Avoir les propriétés de l'instance

Exemple:
```lua
local PartProperties = Part:GetProperties()
```

**Destroy**
```lua
Instance:Destroy()
```
Detruire l'objet. Si DoPooling est vraie, donc au lieu de détruire l'objet, on le met dans le fichier Cache pour pouvoir la reprendre après.
