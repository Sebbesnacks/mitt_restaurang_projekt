# mitt_restaurang_projekt
Building_AI_kurs_projekt_
# PrepPilot

Final project for the Building AI course

## Summary

PrepPilot is an AI-assisted planning tool for restaurant kitchens. It predicts future demand and recommends how much food should be prepared or ordered, with the goal of reducing food waste without leaving the kitchen unprepared for service.

## Background

Restaurant kitchens frequently make preparation and purchasing decisions under significant time pressure. Perishable ingredients such as fresh fish and delicate herbs may be ordered in excessive quantities because the kitchen wants to avoid running out during a busy weekend.

During an evening service, the senior kitchen staff must simultaneously manage food production, service times, cleaning, ordering and the working hours of the kitchen team. There may not be enough time to properly inspect a full or disorganized refrigerator or freezer before placing an order. Products already in stock can therefore be overlooked and ordered again.

The main problems PrepPilot attempts to address are:

* excessive preparation of products with a short shelf life
* ingredients being ordered without an accurate inventory check
* food waste caused by uncertain future demand
* financial losses from discarded ingredients, labour and energy
* preparation decisions being made under stress and time pressure
* the lack of structured information about actual consumption per portion

Underpreparation can also cause problems. However, an experienced kitchen can sometimes work faster and prepare additional food during service. Food that has already spoiled cannot be recovered. The primary objective is therefore to reduce overpreparation while maintaining a reasonable safety margin.

My motivation comes from working as a sous chef and experiencing how professional intuition, operational stress and incomplete information affect preparation and purchasing decisions.

## How is it used?

PrepPilot would be used by head chefs, sous chefs and mise en place chefs when planning preparation and purchasing for upcoming services.

Before ordering or preparing food, a chef enters a quick inventory count of important ingredients and prepared components. The system also receives information about upcoming reservations and historical sales.

The AI predicts the expected number of portions for each product or component. Since the evening service thinks in portions while the preparation team often works in kilograms, litres or individual items, the system converts the prediction into practical production quantities.

For example:

```text
Expected demand:               100 portions
Estimated required quantity:    16 kg
Current usable inventory:         9 kg
Recommended preparation:          7 kg
Risk of waste:                  Low
```

Early in the week, PrepPilot would recommend preparing products that have a longer shelf life and can safely last until the weekend. Products with a short shelf life would only be prepared for the following few days.

Later in the week, the system would check what remains, identify shortages and recommend the final preparation and purchasing required for the weekend. It would also begin planning the following week based on upcoming reservations and the results of the current week.

The planning logic could be described as:

```python
if early_in_the_week:
    prepare_long_life_products(for_the_weekend)
    prepare_short_life_products(for_the_next_few_days)

elif late_in_the_week:
    prepare_missing_products(for_the_weekend)
    prepare_short_life_products(for_the_nearest_services)

update_the_plan_for_next_week()
```

PrepPilot would provide recommendations rather than automatically ordering ingredients. Kitchen staff must retain the final decision because they may know about menu changes, special events or ingredient quality that the system cannot detect.

## Data sources and AI methods

PrepPilot would depend mainly on internal restaurant data.

Possible data sources include:

* upcoming reservations
* historical reservations and actual guest numbers
* sales of individual dishes
* weekday and season
* previous ingredient orders and deliveries
* current inventory counts
* quantities prepared by the mise en place team
* remaining food after service
* registered food waste
* product prices and shelf life
* estimated food cost per guest

The most important input would be upcoming reservations because this is usually the first information senior kitchen staff use when planning inventory and preparation.

A supervised regression model could learn the relationship between reservations and actual consumption. For example, 150 reservations on a Friday may result in a different demand from 150 reservations on a Tuesday.

The model would predict demand in portions. A separate calculation would convert the predicted portions into the measurement used by the preparation team:

```python
predicted_portions = demand_model.predict(service_data)

required_quantity = (
    predicted_portions
    * estimated_quantity_per_portion
)

preparation_needed = max(
    0,
    required_quantity - current_inventory
)
```

The restaurant does not currently have an accessible and standardized record of the exact quantity used in each portion. Portions are partly based on professional experience and intuition. The system would therefore require an initial data-collection and calibration process.

For selected expensive or perishable products, the kitchen could record:

```text
actual consumption =
opening inventory
+ prepared or delivered quantity
- closing inventory
- recorded waste
```

The result could then be divided by the number of portions sold to estimate the average quantity used per portion. The model should learn both the average and its normal variation rather than assuming that every portion is identical.

A classification model could additionally estimate whether a product has a high risk of waste or shortage. An optimization component could then create a rolling preparation plan that considers:

* expected demand
* current inventory
* shelf life
* ingredient cost
* preparation time
* risk of waste
* risk of running out

Cross-validation would be used to evaluate the models and reduce the risk of overfitting to a small number of unusual services.

## Challenges

The largest challenge is the absence of complete and standardized portion and inventory data. Manual counts may be incomplete or inconsistent, particularly during stressful services.

Previous ingredient orders can provide useful information, but they do not prove that a product is still available or usable. Reservations are also uncertain because of cancellations, walk-in guests and changes in group size.

Other limitations include:

* menus and ingredient prices change over time
* portion sizes may vary between chefs
* the same ingredient may be used in several dishes
* recorded preparation does not always equal actual consumption
* food quality cannot always be represented accurately as numerical data
* unusual events may produce demand that differs from historical patterns
* employees may not have time to register every inventory change

The system must not reduce food quality or encourage portions to become smaller simply to improve financial statistics. Its purpose is to reduce food that never reaches the guest, not to replace professional judgement or craftsmanship.

Predictions should therefore be presented as ranges with an indication of uncertainty. Kitchen staff must be able to modify or reject every recommendation.

The collected information should be used for planning food production, not for evaluating or monitoring the performance of individual employees.

## What next?

The project could begin with a small selection of expensive and perishable products, such as fresh fish and delicate herbs. This would make data collection manageable and allow the restaurant to test whether PrepPilot reduces waste without increasing shortages.

It could later include:

* sauces measured in litres
* bread and dough measured in kilograms
* kroppkakor and other components measured in individual items
* automatic integration with reservation platforms
* point-of-sale data
* purchasing and delivery systems
* digital inventory lists
* weekly reports showing waste and food cost per guest

After collecting sufficient data, PrepPilot could compare similar services, identify repeated causes of waste and continuously update its predictions through a rolling forecast.

The project could eventually be adapted for other restaurants. Each kitchen would still require its own calibration because menus, recipes, portioning and working methods are different.

To move forward, the project would require cooperation from kitchen staff, access to reservation and sales data, support from restaurant management and additional skills in data collection, machine learning, software development and user-interface design.

## Acknowledgments

* The project is inspired by my personal experience working as a sous chef.
* The AI methods and project structure are inspired by the [Building AI course](https://buildingai.elementsofai.com/) created by Reaktor Innovations and the University of Helsinki.
* No external code, images or datasets have been used in this project description.



-------------------------------------------------------------------------------------------------------------------------------------

# PrepPilot

Slutprojekt för kursen Building AI

## Sammanfattning

PrepPilot är ett AI-baserat planeringsverktyg för restaurangkök. Systemet förutspår framtida efterfrågan och rekommenderar hur mycket mat som bör förberedas eller beställas. Målet är att minska matsvinn utan att köket blir oförberett inför service.

## Bakgrund

Restaurangkök behöver ofta fatta beslut om prepp och råvarubeställningar under stor tidspress. Känsliga råvaror som färsk fisk och örter kan beställas i för stora mängder eftersom köket vill undvika att de tar slut under en hektisk helg.

Under en kvällsservice måste den ansvariga kökspersonalen samtidigt hantera matproduktion, serveringstider, städning, beställningar och kockarnas arbetstider. Det finns inte alltid tillräckligt med tid för att noggrant kontrollera en full eller oorganiserad kyl och frys innan en beställning skickas. Produkter som redan finns i lager riskerar därför att förbises och beställas igen.

PrepPilot försöker lösa följande problem:

* överprepp av produkter med kort hållbarhet
* råvaror som beställs utan en ordentlig lagerkontroll
* matsvinn orsakat av osäker framtida efterfrågan
* ekonomiska förluster från kasserade råvaror, arbetstid och energi
* preppbeslut som fattas under stress och tidspress
* avsaknad av strukturerad information om faktisk förbrukning per portion

För lite prepp kan också skapa problem. Ett erfaret kök kan däremot ibland höja arbetstakten och förbereda ytterligare mat under service. Mat som redan har blivit dålig kan inte räddas. Det huvudsakliga målet är därför att minska överprepp samtidigt som en rimlig säkerhetsmarginal behålls.

Min motivation kommer från min egen erfarenhet som souschef och från att ha sett hur yrkesmässig magkänsla, stress och ofullständig lagerinformation påverkar prepp och beställningar.

## Hur används systemet?

PrepPilot skulle användas av kökschef, souschef och mise en place-kock när köket planerar prepp och inköp inför kommande servicetillfällen.

Innan råvaror beställs eller förbereds registrerar en kock en snabb inventering av viktiga råvaror och färdiga komponenter. Systemet får även information om kommande bokningar och historisk försäljning.

AI-modellen förutspår hur många portioner som sannolikt kommer att behövas av varje produkt eller komponent. Kvällsservicen räknar huvudsakligen i portioner, medan preppköket ofta arbetar med kilogram, liter eller antal. Systemet omvandlar därför portionsprognosen till praktiska produktionsmängder.

Ett exempel på en rekommendation:

```text
Förväntad efterfrågan:        100 portioner
Uppskattad mängd som behövs:   16 kg
Användbart lager:               9 kg
Rekommenderad prepp:            7 kg
Risk för svinn:               Låg
```

Tidigt i veckan rekommenderar PrepPilot att köket förbereder produkter som har längre hållbarhet och kan räcka fram till helgen. Produkter med kort hållbarhet förbereds endast för de närmaste dagarna.

Senare i veckan kontrollerar systemet vad som finns kvar, identifierar brister och rekommenderar den sista preppen och beställningen inför helgen. Systemet börjar samtidigt planera nästkommande vecka utifrån framtida bokningar och resultatet från den aktuella veckan.

Planeringslogiken kan beskrivas så här:

```python
if tidigt_pa_veckan:
    preppa_hallbara_produkter(for_helgen)
    preppa_kansliga_produkter(for_de_narmaste_dagarna)

elif sent_pa_veckan:
    fyll_pa_det_som_saknas(for_helgen)
    preppa_kansliga_produkter(for_de_narmaste_servicetillfallena)

uppdatera_planen_for_nasta_vecka()
```

PrepPilot ger rekommendationer men beställer inte råvaror automatiskt. Kökspersonalen måste fatta det slutliga beslutet eftersom den kan känna till menyförändringar, särskilda evenemang eller råvarukvalitet som systemet inte kan upptäcka.

## Datakällor och AI-metoder

PrepPilot skulle huvudsakligen använda restaurangens interna data.

Möjliga datakällor är:

* kommande bokningar
* historiska bokningar och faktiskt antal gäster
* försäljning av enskilda maträtter
* veckodag och säsong
* tidigare råvarubeställningar och leveranser
* aktuella lagerinventeringar
* mängder som har förberetts av mise en place-kocken
* mat som finns kvar efter service
* registrerat matsvinn
* råvarupriser och hållbarhet
* uppskattad råvarukostnad per gäst

Den viktigaste datakällan är antalet framtida bokningar, eftersom detta vanligtvis är den första information som den ansvariga kökspersonalen använder för att planera lager och prepp.

En modell för övervakad regression kan lära sig sambandet mellan bokningar och verklig förbrukning. Exempelvis kan 150 bokningar på en fredag skapa en annan efterfrågan än 150 bokningar på en tisdag.

Modellen förutspår efterfrågan i portioner. En separat beräkning omvandlar sedan portionsantalet till den måttenhet som används av preppköket:

```python
forvantade_portioner = efterfrageprognos(serviceinformation)

mangd_som_behovs = (
    forvantade_portioner
    * uppskattad_mangd_per_portion
)

mangd_att_preppa = max(
    0,
    mangd_som_behovs - aktuellt_lager
)
```

Restaurangen har i nuläget ingen lättillgänglig och standardiserad information om exakt hur mycket som används i varje portion. Portioneringen bygger delvis på yrkeserfarenhet och magkänsla. Systemet skulle därför behöva en inledande period av datainsamling och kalibrering.

För utvalda dyra eller känsliga produkter kan köket registrera:

```text
verklig förbrukning =
ingående lager
+ förberedd eller levererad mängd
- utgående lager
- registrerat svinn
```

Resultatet kan sedan divideras med antalet sålda portioner för att uppskatta den genomsnittliga mängden per portion. Modellen bör lära sig både genomsnittet och den normala variationen, i stället för att anta att alla portioner är identiska.

En klassificeringsmodell kan dessutom bedöma om en produkt har hög risk för svinn eller för att ta slut. En optimeringsdel kan därefter skapa en rullande preppplan som tar hänsyn till:

* förväntad efterfrågan
* aktuellt lager
* hållbarhet
* råvarukostnad
* preppens tidsåtgång
* risk för svinn
* risk för att något tar slut

Korsvalidering kan användas för att utvärdera modellerna och minska risken för överanpassning till ett fåtal ovanliga servicetillfällen.

## Utmaningar

Den största utmaningen är avsaknaden av fullständig och standardiserad information om portioner och lager. Manuella inventeringar kan bli ofullständiga eller inkonsekventa, särskilt under stressiga servicetillfällen.

Tidigare råvarubeställningar kan ge användbar information, men de bevisar inte att en produkt fortfarande finns kvar eller är användbar. Bokningsläget är också osäkert på grund av avbokningar, spontana gäster och förändringar i sällskapens storlek.

Andra begränsningar är:

* menyer och råvarupriser förändras
* portionsstorlekar kan variera mellan olika kockar
* samma råvara kan användas i flera rätter
* registrerad prepp motsvarar inte alltid verklig förbrukning
* matens kvalitet kan inte alltid beskrivas korrekt med siffror
* ovanliga evenemang kan skapa en efterfrågan som avviker från historiska mönster
* personalen kanske inte hinner registrera varje lagerförändring

Systemet får inte försämra matens kvalitet eller uppmuntra köket att göra portionerna mindre enbart för att förbättra ekonomiska nyckeltal. Syftet är att minska den mat som aldrig når gästen, inte att ersätta yrkesmässig bedömning eller hantverk.

Prognoserna bör därför presenteras som intervall med en uppskattning av osäkerheten. Kökspersonalen måste alltid kunna ändra eller avvisa en rekommendation.

Den insamlade informationen ska användas för att planera matproduktionen, inte för att utvärdera eller övervaka enskilda medarbetares prestationer.

## Nästa steg

Projektet kan börja med ett mindre urval av dyra och känsliga produkter, exempelvis färsk fisk och örter. Detta gör datainsamlingen hanterbar och ger restaurangen möjlighet att undersöka om PrepPilot minskar svinnet utan att öka antalet bristsituationer.

Systemet kan senare utökas med:

* såser som mäts i liter
* bröd och deg som mäts i kilogram
* kroppkakor och andra komponenter som räknas i antal
* automatisk anslutning till bokningssystem
* försäljningsdata från kassasystemet
* beställnings- och leveranssystem
* digitala lagerlistor
* veckorapporter över svinn och råvarukostnad per gäst

När tillräckligt mycket data har samlats in kan PrepPilot jämföra liknande servicetillfällen, identifiera återkommande orsaker till svinn och kontinuerligt uppdatera sina prognoser genom en rullande planering.

Projektet kan så småningom anpassas till andra restauranger. Varje kök behöver fortfarande sin egen kalibrering eftersom menyer, recept, portionering och arbetsmetoder skiljer sig åt.

För att utveckla projektet vidare behövs samarbete med kökspersonalen, tillgång till boknings- och försäljningsdata, stöd från restaurangledningen samt ytterligare kunskaper inom datainsamling, maskininlärning, mjukvaruutveckling och användargränssnitt.

## Erkännanden

* Projektet är inspirerat av min egen erfarenhet som souschef.
* AI-metoderna och projektstrukturen är inspirerade av [Building AI-kursen](https://buildingai.elementsofai.com/), skapad av Reaktor Innovations och Helsingfors universitet.
* Ingen extern kod, inga externa bilder och inga externa datamängder har använts i denna projektbeskrivning.
