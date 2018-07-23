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
Zwart-wit foto's zijn weer helemaal in! Ook op social media. Er kunnen meerdere redenen zijn waarom je zou kiezen voor een zwart-wit foto:
* kleuren leiden teveel af;
* zwart-wit foto's kun je altijd maken, terwijl kleurenfoto's bijvoorbeeld midden op een zonnige dag vaak moeilijker te maken zijn;

We gaan nu een zwart-wit versie van het plaatje toveren. Check de onderstaande code:

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

Je kunt ook plaatjes vervagen met de zogenaamde Blur filter. Hier een stukje voorbeeld:

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
