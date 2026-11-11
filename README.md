# Travel and Lodging

The common travel vocabulary -- the hotels and stays a traveller books, the room types and rate plans behind the price, the reservations themselves, the guests on them and the flights that get them there -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Travel, Hospitality, Booking, Hotels, Airlines, Aviation, Vacation Rentals, Tourism, Online Travel, Short-Term Rentals in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| booking | 60 | 57 | id 82%, status 72% | bookings 35, reservations 19, booking 7, trips 6 |
| hotel | 24 | 23 | name 91%, address 70%, id 61%, currency 52% | hotels 12, properties 11, hotel 3, accommodations 2 |
| room | 8 | 7 | id 71%, name 71%, hotel_id 57% | rooms 5, room 2, room_types 1, room-types 1 |
| rate_plan | 9 | 9 | hotel_id 67%, currency 56% | rates 6, rate-plans 2, rate_plans 1, rateplans 1 |
| guest | 6 | 5 | id 60% | guests 4, passengers 2, passenger 1 |
| flight | 14 | 12 | status 58%, flight_number 50% | flights 10, flight 2, flight-offers 2, schedules 2 |

Cohort: 184 providers with harvested OpenAPI, 67 crosswalks. Of the providers exposing a booking: 72% create, 82% list, 78% fetch, 45% update, 28% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A booking agrees only on id and status. Check-in, check-out and the guests sit at 37%: hotel stays, flights, tours and car rentals are all bookings with different shapes.
- A hotel agrees on name, address and id; amenities and star rating are modelled by too few to share.
- Flights agree on a flight number and a status, and little else: NDC-style offers, schedules and flight status are three different resources published under one word.
- Guests and rooms are thin (5-7 providers): property-management systems and channel managers keep their APIs to partners.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- A booking is `/bookings`: bookings lead reservations 42 to 19, and the scheduling context uses `/appointments`, so the word is free. Appointment bookings (AgendaPro, Cliniko) are excluded here and counted there.
- A hotel is `/hotels`; Expedia's and Booking.com's bare `/properties` counts as a hotel at a provider that also takes bookings, because property management uses the same word for buildings.
- A room is `/rooms` (rooms lead room types 7 to 2); a rate plan stays `/rate-plans`, though `rates` leads, because a bare rate is FX or tax elsewhere.
- Passengers and travellers are guests.
- Car-rental rates and air-cargo bookings are out of scope and excluded by name.
- Duplicates merged: CrunchTime, Cvent, DOT.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
