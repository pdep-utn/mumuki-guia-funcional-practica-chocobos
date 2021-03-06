Las carreras de chocobos son un entretenimiento cada día más popular, y por lo tanto ya es hora de armar un programa que nos ayude a analizarlas como es debido. Elegimos hacerlo en Haskell, básicamente por inercia (y... ya que lo venimos usando hace un tiempo sigamos con eso).

Las pistas por las que nuestros emplumados amigos deben correr van a estar representadas por listas de tramos, cada tramo a su vez sera representado por una tupla (distancia, correcionDeVelocidad).

```haskell
type Tramo = (Int, Chocobo -> Int)
type Pista = [Tramo]

bosqueTenebroso :: Pista
bosqueTenebroso = [ (100, f1) , (50, f2) , (120, f2) , (200, f1) , (80, f3) ]

pantanoDelDestino :: Pista
pantanoDelDestino = [ (40, f2) , (90, (\(UnChocobo _ (f,p,v))-> f + p + v)) , (120, fuerza) , (20, fuerza) ]

f1 chocobo = velocidad chocobo * 2
f2 chocobo = velocidad chocobo + fuerza chocobo
f3 chocobo = velocidad chocobo `div` peso chocobo
```

Tenemos los chocobos (esenciales para una carrera de chocobos): el negro, el amarillo, el blanco y el rojo. Cada uno tiene distintas características, modeladas por medio de un data:

```haskell
data Chocobo = UnChocobo String (Integer,Integer,Integer) deriving (Show, Eq)

amarillo = UnChocobo "amarillo" (5,3,3)
negro = UnChocobo "negro" (4,4,4)
blanco = UnChocobo "blanco" (2,3,6)
rojo = UnChocobo "rojo" (3,3,4)

fuerza (UnChocobo _ (f,_,_)) = f
peso (UnChocobo _ (_,p,_)) = p 
velocidad (UnChocobo _ (_,_,v)) = v 
```

Finalmente estos chocobos estan dirigidos por los jinetes:

```haskell
data Jinete = UnJinete {
    String :: nombre,
    Chocobo :: chocobo
  } deriving (Show, Eq)

leo = UnJinete "Leo" amarillo
gise = UnJinete "Gise" blanco
mati = UnJinete "Mati" negro
alf = UnJinete "Alf" rojo

apocalipsis = [leo, gise, mati, alf]
```

Se dispone de esta función a modo de ayuda que, a partir de una lista y un criterio de ordenamiento, nos devuelve la versión equivalente a esa lista pero con los elementos ordenados por el criterio dado:

```haskell
quickSort _ [] = []
quickSort criterio (x:xs) =(quickSort criterio . filter (not . criterio x)) xs ++ [x] ++ (quickSort criterio . filter (criterio x)) xs
```
