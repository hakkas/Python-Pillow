# Een foto inladen

Je hoeft niet alle code goed te kennen. Volg gewoon de stappen om een paar leuke dingetjes te doen met foto's.

Ga naar trinket.io en maak een account. Maak vervolgens een nieuwe Python3-trinket aan.

Voer nu de volgende code uit:

```Python
from PIL import Image
from io import BytesIO
import requests

url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

img.save("mooiefoto.jpg")
```
Als het goed is krijg je een leuke foto te zien.

> #### Note::Opdracht 1
Ga op zoek naar een leuk plaatje. Niet te groot hoor! Vervang de link in de code door de URL naar jouw plaatje. Krijg je nu jouw plaatje te zien?

We gaan nu een zwart-wit versie van jouw plaatje toveren. Check:

```Python
from PIL import Image
from io import BytesIO
import requests

url = "https://as.ftcdn.net/r/v1/pics/ea2e0032c156b2d3b52fa9a05fe30dedcb0c47e3/landing/images_photos.jpg"
response = requests.get(url)
img = Image.open(BytesIO(response.content))

zwartwit = img.convert('L')

zwartwit.save("mooiezwartwitfoto.jpg")
```

Neem de code over en ga na dat je een zwart-wit foto te zien krijgt.

Het komt vooral door de regel: ```zwartwit = img.convert('L')```

Convert betekent _omzetten_. De convert-functie zet dus het plaatje om naar een zwart wit plaatje.

Als je in plaats van ```img.convert('L')``` de volgende code plaatst krijg je _ook_ een zwart-wit plaatje:

```zwartwit = img.convert('1')```

Wat is dan het verschil?

Met ```convert("L")``` wordt de kleur omgezet naar een _grijswaarde_ tussen 0 en 255 en bij ```convert ("1")``` wordt de kleur omgezet naar een 0 (wit) of een 1 (zwart).
