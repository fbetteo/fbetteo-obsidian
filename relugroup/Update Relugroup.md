### General


#### **What is GDS in travel?**

A Global Distribution System (GDS) is a computerized network system that facilitates transactions between travel service providers such as [**airlines**](https://www.sabre.com/industries/full-service-carrier-airlines/), [**hotels**](https://www.sabre.com/industries/hoteliers/), [**car rental companies**](https://www.sabre.com/industries/car-rental-companies/) and [**travel agencies**](https://www.sabre.com/industries/travel-agencies/). The GDS software offers a centralized platform for distributing travel-related content and plays a vital role in streamlining the travel booking process, connecting travel agencies with a diverse range of service providers, and ensuring a smooth flow of information and transactions within the travel ecosystem.
Source: https://www.sabre.com/transforming-global-distribution-system/


#### **PSS**

A **Passenger Service System** or **PSS** is a network of software applications that help airlines manage all the passenger-related operations from ticketing to boarding.[[1]](https://en.wikipedia.org/wiki/Passenger_service_system#cite_note-1) The PSS usually comprises an [airline reservations system](https://en.wikipedia.org/wiki/Airline_reservations_system "Airline reservations system"), an airline inventory system and a [departure control system](https://en.wikipedia.org/wiki/Departure_control_system "Departure control system") (DCS).
Source: https://en.wikipedia.org/wiki/Passenger_service_system
#### **GDS vs PSS**
GDS systems are the generic term for the large travel distribution companies. They provide marketing/distribution systems to airlines (hence the “GDS” or Global Distribution Systems name). If you’re referring to traditional PSS (Passenger Service Systems) systems, all GDSs were originally the IT departments of a major airline so they also provide airlines with a variety of PSS (airline operating) system capabilities such as crew management/scheduling, passenger checkin, in flight meal provisioning, seat assignment, etc. that are needed to run their businesses.

More recently a focus for PSS systems has been put on facilitating “ancillary” sales to increase airline revenues, which also touches on GDS capabilities since items can be upsold at time of purchase but facilitated in transit (i.e. premium meal service, extra leg room, etc.).

These two systems are offered to airlines independently, and are usually run as completely separate businesses within the GDS companies. Its one of the reasons it has been difficult for GDSs to broadly/quickly support the ancillary sales efforts for airlines.
Source https://www.quora.com/What-is-the-difference-between-a-GDS-and-PSS-in-terms-of-Airline-systems


#### Price updates

Companies are willing to change price dynamically without limit. As soon as it makes sense to shift the price, we are able to do so.

#### Get flight information (competitor data)

Infare (?) - Paid API
Amadeus - Paid API - Tienen una version free de prueba con mock data
Google flights - Scrape but hasn't too much info and only lowest price.

### Corsair

#### 2024-07-01 

Call with Corsair. Orlando (Manager) + Data Engineer

ATPCO (distribuye el precio to GDS)
Altea de Amadeus (estado online de los vuelos) (GDS) PSS?

Call con otro chabon que tiene experiencia en amadeus. Next Steps.
AVS messaging, lo que usan las OTA (despegar) para saber si quedan asientos disponibles para vender. (esto es para pull)
Push indefinido por ahora. Ver si Corsair nos permite usar su API o hablar con el account manager a ver que API podemos usar y cual nos sirve.

### FlyeEgypt

Usan Avtra (en vez de amadeus) as GDS or PSS? PSS no?

Agosto 2024: No usan ATPCO, creo que usan solo PSS que puede ser Avtra.



###  ATPCO

better docs: https://relugroup.atlassian.net/wiki/spaces/OC/pages/89260033/ATPCO+Airline+Tariff+Publishing+Company

Api para footnotes y rules 
* footnotes: restrictiones
* rules: disponible para los proximos 2 dias y cosas asi

Fares:
ftp con cierto formato.

Se puede modificar que clase de fares esta abierto y no mediante footnotes. Restringis el eñ precio y no hace falta modificar el GDS (iria PSS aca no?).

