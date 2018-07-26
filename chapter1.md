# Workshop Intagramfilters maken met Python

Je hoeft niet alle code goed te kennen. Volg gewoon de stappen om een paar leuke dingetjes te doen met foto's.

## Trinket.io
Ga naar trinket.io en maak een account aan. Maak vervolgens een nieuwe Python3-trinket aan.

## Het openen van een plaatje
Voer nu de volgende code uit:

```Python
from PIL import Image
from io import BytesIO
import requests

# de link naar ons plaatje
url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"

# haal het plaatje van het internet op en bewaar het in response
response = requests.get(url)

# lees het nu echt als plaatje in
img = Image.open(BytesIO(response.content))

# bewaar het plaatje
img.save("mooiefoto.jpg")
```
Als het goed is krijg je een leuke foto te zien.

> #### Note::Opdracht 1
Ga op zoek naar een leuk plaatje. Niet te groot hoor! Vervang de link in de code door de URL naar jouw plaatje. Krijg je nu jouw plaatje te zien?


## Zwart-wit foto's
Zwart-wit foto's zijn weer helemaal in! Ook op social media.

We gaan nu een zwart-wit versie van het plaatje toveren. Zie de onderstaande code:

```Python
from PIL import Image
from io import BytesIO
import requests

url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

# converteer het plaatje naar zwart-wit
zwartwit = img.convert('L')

zwartwit.save("mooiezwartwitfoto.jpg")
```

> #### Note::Opdracht 2
Neem de code over en ga na dat je een zwart-wit foto te zien krijgt.

Het komt vooral door de regel: ```zwartwit = img.convert('L')```

Convert betekent _omzetten_. De convert-functie zet dus het plaatje om naar een zwart wit plaatje.

Als je in plaats van ```img.convert('L')``` de volgende code plaatst krijg je _ook_ een zwart-wit plaatje: ```zwartwit = img.convert('1')```
Wat is dan het verschil?

Met ```convert("L")``` worden de kleuren omgezet naar _grijswaardes_ tussen 0 en 255 en bij ```convert ("1")``` wordt de kleur omgezet naar een 0 (wit) of een 1 (zwart).

## Vervagen met blur

Je kunt ook plaatjes vervagen met de zogenaamde Blur filter. Hier een voorbeeldje:

```python
# Ook ImageFilter importeren voor blurring
from PIL import Image, ImageFilter
import requests
from io import BytesIO
url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

# pas de Blurfilter toe op het plaatje
blurred_image = img.filter(ImageFilter.BLUR)

blurred_image.save("blurredimage.jpg")
```

De echte _magic_ gebeurt in deze regel: ```blurred_image = img.filter(ImageFilter.BLUR)```

> #### Note::Opdracht 3
Naast ```ImageFilter.BLUR``` kun je ook voor de volgende filteropties kiezen:
* CONTOUR
* DETAIL
* EDGE_ENHANCE
* EDGE_ENHANCE_MORE
* EMBOSS
* FIND_EDGES
* SHARPEN
* SMOOTH
* SMOOTH_MORE
Probeer er eens een paar uit en bekijk het resultaat!

## Pixel voor Pixel

Nu gaan we iets meer de diepte in. Want wat is een plaatje eigenlijk? Een plaatje bestaat uit heel veel puntjes die we _pixels_ noemen. In een echt zwart-wit plaatje is een pixel dan zwart of wit. Op een kleurenfoto is elke pixel een RGB-waarde. Beluister eens aandachtig het volgende filmpje:

{%youtube%}15aqFQQVBWU{%endyoutube%}

![](./images/grid.png)

Dus:
* Plaatjes bestaan uit een heleboel pixels (puntjes).
* Elke pixel heeft een kleur.
* Elke kleur heeft een waarde voor rood, groen en blauw (RGB).
* Elke waarde is een getal tussen 0 en 255.
* Als de waarde voor rood, groen _en_ blauw allemaal 0 is, dan is de kleur die je als resultaat hebt _zwart_.
* En als de waarde voor rood, groen _en_ blauw 255 is (hoogste waarde) dan krijg je _wit_.

### Een voorbeeld:

Voer de volgende code eens uit. Wat gebeurt er?

```Python
from PIL import Image, ImageFilter, ImageDraw, ImageFont
import requests
from io import BytesIO

url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

width, height = img.size
for y in range(height):
    for x in range(width):
      r, g, b = img.getpixel((x, y))
      b = 0
      img.putpixel((x,y), (r,g,b))

img.save("filtered.jpg")
```

Uitleg:
* Met de regel ``` width, height = img.size ``` vragen we eerst de breedte en de hoogte van het plaatje op en zetten dat in variabelen.
* Vervolgens willen we loopen door alle pixels. Omdat we te maken hebben met een 2-dimensionaal grid doen we dat met een dubbele for-loop.
* Vervolgens pakken we een pixel met ```img.getpixel((x,y))``` en maken we van de b-waarde 0. Uiteindelijk zetten we het pixeltje terug op zijn plek.

Kortom: Al het blauwe wordt weggehaald van de foto :)

> #### Note::Opdracht 4
Kun jij het programma aanpassen zodat je niet al het blauwe weghaalt, maar al het rode? Hoe ziet je plaatje er dan uit?


## Omdraaien!
Je kunt ook de RGB waarde voor elke pixel omdraaien. Zie eens de volgende code:

```Python
from PIL import Image, ImageFilter, ImageDraw, ImageFont
import requests
from io import BytesIO

url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

width, height = img.size
for y in range(height):
    for x in range(width):
      r, g, b = img.getpixel((x, y))
      img.putpixel((x,y), (g,r,b))

img.save("omgedraaid.jpg")
```

Merk op dat we voor elke pixel de r en de g hebben omgedraaid! Cool he?!

> #### Note::Opdracht 5
Pas het programma aan zodat je andere kleuren omdraait?


## Maak een poster!
Probeer eens de volgende code uit.
Ga na wat er gebeurt!

```Python
from PIL import Image, ImageFilter, ImageDraw, ImageFont
import requests
from io import BytesIO


# url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"

url = "https://ae01.alicdn.com/kf/HTB1_gzOKpXXXXcEXXXXq6xXFXXXh/Bloemen-Zaad-Voor-Tuin-Klimmen-Rose-100-stks-China-Rozenstruik-Geweldige-Promotie-Planten-Plantenbakken-Bonsai-Zaden.jpg_640x640.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

width, height = img.size
for y in range(height):
    for x in range(width):
      r, g, b = img.getpixel((x, y))

      if r < 120:
        r = 0
      if g < 120:
        g = 0
      if b < 120:
        b = 0

      if r >= 120:
        r = 120
      if g >= 120:
        g = 120
      if b >= 120:
        b = 120

      img.putpixel((x,y), (r,g,b))

img.save("poster.jpg")
```

Kun je het korter schrijven?
