# Niche Rankings v3: 68 candidates, combined money and feasibility scores

Status: research synthesis, 2026-09-29. Inputs: `research/niche-batches/` (programs, incumbents, data, streams) and `research/keyword-volumes.csv` (three head terms per niche).
Volume sources: 4 of 204 terms had a stated tool source; the rest are calibrated estimates. Treat volumes and pools as order-of-magnitude until pulled from Ahrefs or Semrush.

## Columns and scores

- Volume/mo: sum of US monthly searches for the three most common "best ..." head terms (terms listed in the table).
- Sales/mo (market): Volume x 25% click-out x 3% conversion = Volume x 0.75%. Estimated affiliate sales per month across all sites ranking for the three terms.
- $/sale: estimated commission per sale for a new site using the best realistically accessible program.
- Pool/mo: Sales/mo x $/sale. Whole-market monthly commission for the three terms; long tail excluded.
- Profit potential (P): money and volume only. 70% log-scaled Pool/mo ($2.5k = 2, $200k = 10) plus 30% log-scaled $/sale ($5 = 2, $300 = 10). Profit rank orders niches by P alone.
- Program access (Acc): how easily a new site gets approved (1-5 mapped to 2-10) minus return or clawback risk (0 to -2.5). Money is no longer in this score; it lives in P.
- Streams (B): extra revenue streams beyond product commissions: lead-gen, alerts or subscriptions, B2B data, consumables (1-5, doubled).
- SEO (C): three-term volume tier (3 to 9.5) minus incumbent strength (0 to -3.5) minus YMYL or regulatory penalty (0 to -2.5), plus 1.
- Data (D): certification and government dataset richness (1-5 x 1.2) plus spec comparability (1-5 x 0.8).
- Overall = 0.35 P + 0.10 Acc + 0.10 B + 0.20 C + 0.25 D. Overall rank orders by this.

## Combined table (all 68, ordered by Overall)

| Overall rank | Niche | Three head terms (vol) | Volume/mo | Sales/mo | $/sale | Pool/mo | Profit P | Profit rank | Access | Streams | SEO | Data | Overall | Note |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Laptops and desktops | best laptop (135,000); best gaming laptop (74,000); best laptops (60,500) | 269,500 | 2,021 | $40 | $80,850 | 7.7 | 7 | 5.0 | 6 | 7.0 | 10.0 | **7.68** | Lenovo 5-7%, Dell Outlet 5%; ENERGY STAR/EPEAT; SKU sprawl |
| 2 | Refrigerators | best refrigerator (33,100); best refrigerator brands (9,900); best french door refrigerator (8,100) | 51,100 | 383 | $70 | $26,828 | 6.6 | 13 | 5.0 | 8 | 5.0 | 10.0 | **7.10** | Wayfair 5-7%; DOE/ENERGY STAR; dimension-fit tool |
| 3 | Dehumidifiers and air purifiers | best air purifier (74,000); best dehumidifier (33,100); best air purifier for allergies (9,900) | 117,000 | 878 | $20 | $17,550 | 5.3 | 37 | 8.0 | 6 | 6.5 | 10.0 | **7.06** | Levoit 8%, Sylvane 6%; AHAM CADR; filter re-buys |
| 4 | Credit cards, insurance, loans, solar leads | best credit cards (90,000); best car insurance (40,000); best personal loans (27,000) | 157,000 | 1,178 | $120 | $141,300 | 9.0 | 2 | 1.0 | 8 | 4.5 | 8.0 | **6.96** | Amex $200/approval; issuers closed to new sites; CFPB/TCPA |
| 5 | Car tires and wheels | best all season tires (27,000); best tire brands (14,000); best tires (12,000) | 53,000 | 398 | $45 | $17,888 | 5.8 | 25 | 8.0 | 8 | 4.0 | 10.0 | **6.93** | Tire Rack 6%, Priority Tire 7%; NHTSA UTQG; massive per-vehicle long tail |
| 6 | Washing machines and dryers | best washing machine (40,500); best washer and dryer (22,200); best dryer (12,100) | 74,800 | 561 | $40 | $22,440 | 6.0 | 21 | 5.0 | 8 | 5.0 | 10.0 | **6.91** | Wayfair 5-7%, AJ Madison, LG ~6%?; DOE CCMS + ENERGY STAR; Yale service data |
| 7 | Ductless mini-split heat pumps | best mini split (9,900); best ductless mini split (4,400); best mini split heat pump (3,600) | 17,900 | 134 | $90 | $12,082 | 5.7 | 29 | 7.0 | 8 | 4.5 | 10.0 | **6.90** | HVACDirect 5%, Sylvane 6%; AHRI + NEEP + ENERGY STAR; installer lead-gen |
| 8 | Mattresses and bedding | best mattress (135,000); best mattress for side sleepers (40,000); best mattress for back pain (27,000) | 202,000 | 1,515 | $140 | $212,100 | 9.6 | 1 | 3.5 | 6 | 7.0 | 4.8 | **6.89** | DreamCloud $150 flat, Purple $75-150; Pillar4 owns four top sites; 100-night trials |
| 9 | Office chairs and standing desks | best office chair (60,000); best standing desk (18,000); best ergonomic office chair (14,000) | 92,000 | 690 | $45 | $31,050 | 6.5 | 15 | 7.0 | 6 | 6.5 | 8.0 | **6.88** | Herman Miller 4%, Autonomous to 10%; BIFMA lists; BTOD |
| 10 | Hosting, VPN, password managers | best vpn (135,000); best password manager (40,000); best web hosting (27,000) | 202,000 | 1,515 | $50 | $75,750 | 7.7 | 6 | 3.5 | 8 | 6.0 | 6.8 | **6.75** | Bluehost $65, NordVPN 40% plus recurring; March 2026 crushed generic pages |
| 11 | Monitors | best gaming monitor (60,500); best monitor (27,000); best ultrawide monitor (9,900) | 97,400 | 730 | $18 | $13,149 | 4.9 | 42 | 7.0 | 6 | 5.5 | 10.0 | **6.61** | LG 6%, BenQ 3%; VESA/ENERGY STAR data; RTINGS |
| 12 | Hot tubs, saunas, cold plunges | best cold plunge (12,000); best hot tub (9,900); best infrared sauna (8,100) | 30,000 | 225 | $250 | $56,250 | 8.3 | 3 | 5.0 | 8 | 4.5 | 6.0 | **6.60** | Plunge/Redwood $250 flat; hot tubs via dealer lead-gen |
| 13 | E-bikes | best electric bike (29,900); best electric bikes (9,900); best ebike (8,100) | 47,900 | 359 | $70 | $25,148 | 6.5 | 16 | 5.0 | 8 | 5.0 | 8.0 | **6.57** | Ride1Up 5%, Aventon 4%, Trek 5%; UL 2849 lists; insurance lead-gen |
| 14 | Televisions | best tv (90,000); best 65 inch tv (40,500); best tvs (27,000) | 157,500 | 1,181 | $22 | $25,988 | 5.9 | 24 | 5.0 | 6 | 7.0 | 8.0 | **6.55** | Amazon 2%, LG 6%; RTINGS/Wirecutter/CNET labs |
| 15 | Pool equipment (robots, pumps, heaters) | best robotic pool cleaner (14,000); best pool vacuum (6,600); best pool heater (4,400) | 25,000 | 188 | $85 | $15,938 | 6.0 | 20 | 7.0 | 8 | 4.5 | 8.0 | **6.51** | Aiper 8-10%, In The Swim 10%; ENERGY STAR/WEF pump data; pool service lead-gen |
| 16 | Whole-house water filtration / softeners | best water softener (14,800); best water softener system (3,600); best whole house water filter (8,100) | 26,500 | 199 | $225 | $44,719 | 7.9 | 4 | 7.0 | 8 | 4.5 | 5.2 | **6.47** | SpringWell 10-20%, Aquasana 10%; NSF/WQA listings; health-claim scrutiny |
| 17 | Dishwashers | best dishwasher (49,500); best dishwasher brands (6,600); best bosch dishwasher (3,600) | 59,700 | 448 | $30 | $13,432 | 5.2 | 38 | 5.0 | 6 | 5.0 | 10.0 | **6.42** | NSF 184, DOE, ENERGY STAR; dBA universal spec |
| 18 | Smartphones and tablets | best phone (60,500); best tablet (60,500); best android phone (33,000) | 154,000 | 1,155 | $30 | $34,650 | 6.4 | 17 | 1.5 | 6 | 6.0 | 8.8 | **6.39** | Carrier CPA $25-75; Apple pays nothing; GSMArena |
| 19 | Smart home security (cameras, alarms) | best home security system (40,500); best home security camera (27,000); best security cameras (22,000) | 89,500 | 671 | $25 | $16,781 | 5.4 | 33 | 9.0 | 8 | 6.5 | 6.0 | **6.38** | Eufy 7-13%, Arlo 7%, SimpliSafe 8%; ADT leads $27.50; Matter DB |
| 20 | Espresso machines and grinders | best espresso machine (60,500); best coffee grinder (22,200); best home espresso machine (8,100) | 90,800 | 681 | $80 | $54,480 | 7.6 | 8 | 7.0 | 6 | 7.5 | 3.6 | **6.35** | Clive 10%, Seattle Coffee Gear 8%, De'Longhi 12%; no public data |
| 21 | Engagement rings and fine jewelry | best place to buy engagement ring (6,000); best lab grown diamonds (4,000); best engagement rings (3,600) | 13,600 | 102 | $260 | $26,520 | 7.3 | 9 | 3.5 | 8 | 0.5 | 10.0 | **6.32** | James Allen 5%, Blue Nile 5%; GIA and 4Cs feeds; lab-grown price collapse |
| 22 | Home battery + DIY solar kits | best solar battery (6,600); best home battery backup (4,400); best solar panels for home (9,900) | 20,900 | 157 | $125 | $19,594 | 6.5 | 14 | 7.0 | 8 | 3.5 | 7.2 | **6.28** | Renogy 6%, Signature Solar 9% (7d); UL 9540, CEC lists, DSIRE; installer lead-gen |
| 23 | Mesh Wi-Fi and networking | best router (40,500); best wifi router (33,000); best mesh wifi (33,000) | 106,500 | 799 | $22 | $17,572 | 5.4 | 34 | 7.0 | 4 | 6.5 | 8.0 | **6.28** | TP-Link to 10%, Netgear 7%; Wi-Fi Alliance certs; eero has no program |
| 24 | Portable power stations / solar generators | best portable power station (22,000); best solar generator (14,800); best power station (6,600) | 43,400 | 326 | $55 | $17,902 | 5.9 | 22 | 5.0 | 6 | 6.0 | 7.6 | **6.27** | EcoFlow/Jackery/Bluetti/Anker 5-10%; UL 2743; constant promos suit alerts |
| 25 | Water heaters (tankless, heat pump) | best tankless water heater (14,800); best water heater (12,100); best heat pump water heater (4,400) | 31,300 | 235 | $36 | $8,451 | 4.7 | 47 | 4.0 | 8 | 4.5 | 10.0 | **6.25** | No brand programs; AHRI UEF data; plumber lead-gen and rebates |
| 26 | Massage chairs | best massage chair (18,000); best massage chairs (4,400); best massage chair under 2000 (1,900) | 24,300 | 182 | $225 | $41,006 | 7.8 | 5 | 7.0 | 6 | 4.5 | 4.8 | **6.13** | Massage Chair Store 5-10%, Warehouse 10%; dealer-polluted SERPs |
| 27 | Gaming hardware (consoles, GPUs, PCs) | best gaming pc (40,500); best graphics card (22,000); best gpu (22,000) | 84,500 | 634 | $25 | $15,844 | 5.3 | 36 | 3.5 | 6 | 5.5 | 8.8 | **6.11** | Newegg 1-2%, Razer 15% peripherals; stock alerts are the product |
| 28 | Baby gear (car seats, strollers, monitors) | best baby monitor (22,000); best stroller (18,000); best car seat (14,000) | 54,000 | 405 | $25 | $10,125 | 4.7 | 46 | 7.0 | 6 | 4.0 | 9.2 | **6.06** | Nanit 10-20%, Owlet 15%; NHTSA/FMVSS/JPMA/FDA data; BabyGearLab crash rigs |
| 29 | Printers and home office | best printer (60,500); best home printer (40,500); best all in one printer (22,000) | 123,000 | 922 | $12 | $11,070 | 4.4 | 52 | 8.0 | 4 | 6.5 | 8.0 | **6.05** | HP Instant Ink $15 CPA; cost-per-page data |
| 30 | OTC hearing aids | best hearing aids (27,000); best otc hearing aids (9,900); best over the counter hearing aids (6,000) | 42,900 | 322 | $90 | $28,958 | 6.8 | 11 | 3.5 | 8 | 2.5 | 8.0 | **6.04** | Lexie $120 flat, MDHearing 15%; FDA database; HearingTracker lab |
| 31 | Dash cams, car audio, EV accessories | best dash cam (40,500); best car speakers (6,600); best car stereo (5,400) | 52,500 | 394 | $20 | $7,875 | 4.3 | 54 | 7.0 | 6 | 6.0 | 8.0 | **6.00** | Crutchfield 5-10% (60d); Viofo 6% |
| 32 | Home EV chargers | best ev charger (9,900); best home ev charger (6,600); best level 2 ev charger (4,400) | 20,900 | 157 | $50 | $7,838 | 4.8 | 44 | 5.0 | 8 | 3.5 | 9.2 | **5.98** | Emporia/Autel 10%; ENERGY STAR EVSE dataset; electrician lead-gen |
| 33 | Home fitness equipment | best treadmill (27,000); best rowing machine (22,000); best home gym (9,900) | 58,900 | 442 | $75 | $33,131 | 6.9 | 10 | 7.0 | 6 | 4.0 | 5.6 | **5.91** | NordicTrack 5-7%, REP 5%, Rogue 4%; Garage Gym Reviews owns it |
| 34 | Projectors and home theater | best projector (49,500); best 4k projector (14,800); best home theater projector (8,100) | 72,400 | 543 | $60 | $32,580 | 6.7 | 12 | 7.0 | 6 | 5.0 | 4.8 | **5.86** | XGIMI 5%, BenQ 3%; ProjectorCentral DB incumbent |
| 35 | Keyboards, mice, docks, peripherals | best gaming mouse (40,500); best mechanical keyboard (27,000); best wireless mouse (18,000) | 85,500 | 641 | $8 | $5,130 | 3.2 | 61 | 10.0 | 4 | 6.5 | 8.0 | **5.82** | Logitech 4-10%, Razer 15%, Keychron 5%; USB-IF/Thunderbolt lists |
| 36 | Robot vacuums and mops | best robot vacuum (74,000); best robot vacuum and mop (14,800); best roomba (6,600) | 95,400 | 716 | $40 | $28,620 | 6.3 | 18 | 7.0 | 6 | 6.5 | 4.0 | **5.82** | Roborock/Narwal/Dreame 5-10%; Vacuum Wars, RTINGS; specs unverified |
| 37 | Golf equipment and launch monitors | best golf drivers (22,000); best golf balls (18,000); best launch monitor (9,900) | 49,900 | 374 | $45 | $16,841 | 5.7 | 28 | 7.0 | 6 | 5.0 | 6.0 | **5.80** | Rapsodo 10%, TaylorMade 5-6%; MyGolfSpy lab; sim installer lead-gen |
| 38 | Drones and action cameras | best drone (27,000); best action camera (18,000); best drones (12,000) | 57,000 | 428 | $28 | $11,970 | 5.0 | 41 | 7.0 | 6 | 3.5 | 8.0 | **5.75** | Insta360 5-8%, DJI 2-5%; FCC Covered List risk for drones |
| 39 | Pressure washers, snow blowers, chainsaws | best pressure washer (33,000); best snow blower (18,000); best chainsaw (14,000) | 65,000 | 488 | $20 | $9,750 | 4.6 | 49 | 7.0 | 4 | 5.0 | 8.0 | **5.69** | Ohio Power Tool 6.5%; PWMA certified PSI/GPM |
| 40 | Window / portable air conditioners | best portable air conditioner (27,100); best window air conditioner (18,100); best window ac unit (9,900) | 55,100 | 413 | $13 | $5,372 | 3.5 | 58 | 5.0 | 4 | 5.0 | 10.0 | **5.64** | ENERGY STAR CEER data; lab incumbents; seasonal |
| 41 | Grills, smokers, pellet grills | best gas grill (22,200); best smoker (18,100); best pellet grill (14,800) | 55,100 | 413 | $50 | $20,662 | 6.0 | 19 | 7.0 | 6 | 5.0 | 4.8 | **5.62** | Z Grills 7%, Traeger 5%; AmazingRibs DB incumbent |
| 42 | Standby and portable generators | best portable generator (14,800); best generator (12,100); best home generator (8,100) | 35,000 | 262 | $36 | $9,450 | 4.9 | 43 | 4.0 | 8 | 2.5 | 8.8 | **5.60** | Retailer 1-2% only; standby is lead-gen; CO safety |
| 43 | E-scooters and e-mopeds | best electric scooter (27,000); best electric scooter for adults (6,600); best electric scooters (5,400) | 39,000 | 292 | $45 | $13,162 | 5.4 | 30 | 7.0 | 6 | 3.5 | 6.8 | **5.59** | Segway 6%, Apollo 6%; UL 2272; fewer incumbents |
| 44 | Watches, luggage, travel gear | best carry on luggage (18,000); best luggage (14,000); best travel backpack (12,000) | 44,000 | 330 | $25 | $8,250 | 4.5 | 51 | 7.0 | 6 | 5.0 | 6.8 | **5.56** | Peak Design 10%; airline carry-on compliance tool |
| 45 | 3D printers and filament | best 3d printer (60,000); best 3d printers (8,100); best budget 3d printer (6,600) | 74,700 | 560 | $25 | $14,006 | 5.1 | 39 | 7.0 | 6 | 5.0 | 5.6 | **5.50** | Creality 5-10%, Bambu 3%/5% filament; consumables |
| 46 | Ranges, wall ovens, induction cooktops | best gas range (9,900); best induction cooktop (8,100); best induction range (6,600) | 24,600 | 184 | $60 | $11,070 | 5.4 | 35 | 5.0 | 8 | 2.5 | 7.2 | **5.48** | Induction wave; rebate finder; electrician lead-gen |
| 47 | Blenders, stand mixers, food processors | best blender (40,500); best food processor (22,200); best stand mixer (14,800) | 77,500 | 581 | $34 | $19,762 | 5.8 | 26 | 8.0 | 4 | 5.0 | 4.8 | **5.42** | Vitamix ~10%?, Blendtec 8%; recall tracking |
| 48 | Headphones, earbuds, speakers, soundbars | best headphones (74,000); best wireless earbuds (74,000); best soundbar (60,500) | 208,500 | 1,564 | $8 | $12,510 | 4.3 | 53 | 8.0 | 4 | 7.0 | 5.2 | **5.42** | Amazon 3%; subjective quality; RTINGS/SoundGuys |
| 49 | Cameras and lenses | best camera (27,000); best mirrorless camera (18,000); best camera for beginners (12,000) | 57,000 | 428 | $45 | $19,238 | 5.9 | 23 | 5.0 | 6 | 4.0 | 5.6 | **5.36** | B&H 2-8% (60-hour cookie); KEH used 5-10%; DPReview |
| 50 | Safes, security hardware, smart locks | best smart lock (14,000); best gun safe (6,000); best home safe (5,000) | 25,000 | 188 | $14 | $2,625 | 2.7 | 66 | 6.0 | 6 | 3.5 | 10.0 | **5.33** | BHMA/UL ratings; brands lack programs; security lead-gen |
| 51 | Cordless stick vacuums and carpet cleaners | best cordless vacuum (49,500); best vacuum (40,500); best carpet cleaner (27,100) | 117,100 | 878 | $21 | $18,443 | 5.4 | 31 | 7.0 | 4 | 6.5 | 4.0 | **5.29** | Tineco/Bissell/Shark 5-10%; Dyson 1% |
| 52 | NAS and home storage | best external hard drive (18,000); best nas (12,000); best nas for home (4,400) | 34,400 | 258 | $30 | $7,740 | 4.5 | 50 | 5.0 | 6 | 4.5 | 6.8 | **5.27** | UGREEN 8%; Backblaze drive stats; price-per-TB tracker |
| 53 | Robot and battery lawn mowers | best robot lawn mower (14,000); best battery lawn mower (6,600); best robotic lawn mower (3,600) | 24,200 | 182 | $75 | $13,612 | 5.8 | 27 | 7.0 | 4 | 4.5 | 4.8 | **5.21** | Navimow, Greenworks, Husqvarna ~5%; category exploding |
| 54 | Sewing, embroidery, cutting machines | best sewing machine (27,100); best embroidery machine (6,600); best cricut machine (4,400) | 38,100 | 286 | $45 | $12,859 | 5.4 | 32 | 10.0 | 4 | 4.5 | 3.6 | **5.08** | Sewing Machines Plus 10%; Joann closure; no public data |
| 55 | Kayaks, SUPs, camping, coolers | best coolers (22,000); best kayak (9,900); best paddle board (9,900) | 41,800 | 314 | $30 | $9,405 | 4.7 | 45 | 5.0 | 4 | 5.0 | 6.0 | **5.06** | Backcountry 8%, REI 5%; Amazon Outdoors 5.5%; OutdoorGearLab lab |
| 56 | Mobility and medical alert (lead-gen) | best medical alert systems (9,000); best mobility scooter (5,000); best walker for seniors (4,000) | 18,000 | 135 | $65 | $8,775 | 5.1 | 40 | 5.0 | 10 | 1.0 | 6.0 | **4.99** | Medical Guardian $70, Bay Alarm $65 (365d); pay-per-call; Centerfield SERPs |
| 57 | Ceiling, attic, whole-house fans | best ceiling fan (14,800); best attic fan (4,400); best whole house fan (2,400) | 21,600 | 162 | $18 | $2,916 | 2.9 | 63 | 8.0 | 2 | 4.5 | 8.0 | **4.93** | Big Ass Fans 5%; ENERGY STAR CFM/W; style-driven |
| 58 | Pet tech (litter boxes, GPS collars) | best automatic litter box (12,000); best self cleaning litter box (8,000); best gps dog collar (5,000) | 25,000 | 188 | $40 | $7,500 | 4.6 | 48 | 7.0 | 6 | 4.5 | 3.6 | **4.72** | Litter-Robot 8% (90d), Petlibro 10%, Tractive 20%; easiest physical entry |
| 59 | Musical instruments | best digital piano (9,000); best audio interface (9,000); best acoustic guitar (8,000) | 26,000 | 195 | $28 | $5,460 | 4.0 | 55 | 7.0 | 6 | 4.5 | 4.4 | **4.70** | Sweetwater 4-6%; 1st-note spec DB; lesson subscriptions |
| 60 | Range hoods and microwaves | best microwave (22,200); best over the range microwave (8,100); best range hood (5,400) | 35,700 | 268 | $12 | $3,213 | 2.8 | 64 | 6.0 | 4 | 3.5 | 8.0 | **4.69** | HVI CFM/sones data unexploited; low ticket |
| 61 | Central AC / furnaces (installed, lead-gen) | best ac unit (6,600); best furnace brands (4,400); best central air conditioner (3,600) | 14,600 | 110 | $30 | $3,285 | 3.4 | 59 | 3.5 | 8 | 0.0 | 9.2 | **4.64** | Modernize/Networx leads; TCPA compliance; AHRI data |
| 62 | Cordless power tool platforms | best cordless drill (18,000); best impact driver (9,900); best power tool brand (6,600) | 34,500 | 259 | $15 | $3,881 | 3.2 | 60 | 8.0 | 6 | 4.5 | 4.8 | **4.62** | Amazon Tools 5.5%; battery compatibility DB; deal alerts |
| 63 | Air fryers, countertop ovens, pizza ovens | best air fryer (17,800); best toaster oven (22,200); best pizza oven (14,800) | 54,800 | 411 | $15 | $6,165 | 3.8 | 57 | 8.0 | 4 | 5.0 | 4.0 | **4.53** | Ooni 10%; air fryer volume, pizza ovens money |
| 64 | Electric toothbrushes, oral care, shavers | best electric toothbrush (40,000); best electric razor (14,000); best electric shaver (9,000) | 63,000 | 472 | $8 | $3,780 | 2.8 | 65 | 8.0 | 2 | 4.0 | 6.8 | **4.48** | Amazon 1% category; Electric Teeth owns it |
| 65 | Coffee makers, kettles, drip brewers | best coffee maker (33,100); best drip coffee maker (12,100); best electric kettle (9,900) | 55,100 | 413 | $10 | $4,132 | 3.0 | 62 | 10.0 | 2 | 5.0 | 4.8 | **4.47** | SCA certified list; low ticket |
| 66 | Online courses and certifications | best online courses (9,000); best certifications (5,000); best online certifications (4,000) | 18,000 | 135 | $35 | $4,725 | 4.0 | 56 | 5.0 | 6 | 2.5 | 4.8 | **4.18** | Coursera 10-45% (needs traffic); Class Central owns the DB |
| 67 | Smart thermostats and hubs | best smart thermostat (22,200); best thermostat (6,600); best smart home hub (5,400) | 34,200 | 256 | $11 | $2,822 | 2.6 | 67 | 6.0 | 4 | 3.5 | 6.0 | **4.12** | ecobee 5-6%; small basket |
| 68 | Home energy monitors and smart panels | best home energy monitor (1,900); best energy monitor (880); best smart electrical panel (720) | 3,500 | 26 | $15 | $394 | 1.2 | 68 | 8.0 | 6 | 3.5 | 6.0 | **4.04** | Emporia 10%; tiny volume; hub for energy niches |

## Profit potential ranking (money and volume only, all 68)

| Profit rank | Niche | Volume/mo | Sales/mo | $/sale | Pool/mo | Profit P | Overall rank |
|---|---|---|---|---|---|---|---|
| 1 | Mattresses and bedding | 202,000 | 1,515 | $140 | $212,100 | 9.6 | 8 |
| 2 | Credit cards, insurance, loans, solar leads | 157,000 | 1,178 | $120 | $141,300 | 9.0 | 4 |
| 3 | Hot tubs, saunas, cold plunges | 30,000 | 225 | $250 | $56,250 | 8.3 | 12 |
| 4 | Whole-house water filtration / softeners | 26,500 | 199 | $225 | $44,719 | 7.9 | 16 |
| 5 | Massage chairs | 24,300 | 182 | $225 | $41,006 | 7.8 | 26 |
| 6 | Hosting, VPN, password managers | 202,000 | 1,515 | $50 | $75,750 | 7.7 | 10 |
| 7 | Laptops and desktops | 269,500 | 2,021 | $40 | $80,850 | 7.7 | 1 |
| 8 | Espresso machines and grinders | 90,800 | 681 | $80 | $54,480 | 7.6 | 20 |
| 9 | Engagement rings and fine jewelry | 13,600 | 102 | $260 | $26,520 | 7.3 | 21 |
| 10 | Home fitness equipment | 58,900 | 442 | $75 | $33,131 | 6.9 | 33 |
| 11 | OTC hearing aids | 42,900 | 322 | $90 | $28,958 | 6.8 | 30 |
| 12 | Projectors and home theater | 72,400 | 543 | $60 | $32,580 | 6.7 | 34 |
| 13 | Refrigerators | 51,100 | 383 | $70 | $26,828 | 6.6 | 2 |
| 14 | Home battery + DIY solar kits | 20,900 | 157 | $125 | $19,594 | 6.5 | 22 |
| 15 | Office chairs and standing desks | 92,000 | 690 | $45 | $31,050 | 6.5 | 9 |
| 16 | E-bikes | 47,900 | 359 | $70 | $25,148 | 6.5 | 13 |
| 17 | Smartphones and tablets | 154,000 | 1,155 | $30 | $34,650 | 6.4 | 18 |
| 18 | Robot vacuums and mops | 95,400 | 716 | $40 | $28,620 | 6.3 | 36 |
| 19 | Grills, smokers, pellet grills | 55,100 | 413 | $50 | $20,662 | 6.0 | 41 |
| 20 | Pool equipment (robots, pumps, heaters) | 25,000 | 188 | $85 | $15,938 | 6.0 | 15 |
| 21 | Washing machines and dryers | 74,800 | 561 | $40 | $22,440 | 6.0 | 6 |
| 22 | Portable power stations / solar generators | 43,400 | 326 | $55 | $17,902 | 5.9 | 24 |
| 23 | Cameras and lenses | 57,000 | 428 | $45 | $19,238 | 5.9 | 49 |
| 24 | Televisions | 157,500 | 1,181 | $22 | $25,988 | 5.9 | 14 |
| 25 | Car tires and wheels | 53,000 | 398 | $45 | $17,888 | 5.8 | 5 |
| 26 | Blenders, stand mixers, food processors | 77,500 | 581 | $34 | $19,762 | 5.8 | 47 |
| 27 | Robot and battery lawn mowers | 24,200 | 182 | $75 | $13,612 | 5.8 | 53 |
| 28 | Golf equipment and launch monitors | 49,900 | 374 | $45 | $16,841 | 5.7 | 37 |
| 29 | Ductless mini-split heat pumps | 17,900 | 134 | $90 | $12,082 | 5.7 | 7 |
| 30 | E-scooters and e-mopeds | 39,000 | 292 | $45 | $13,162 | 5.4 | 43 |
| 31 | Cordless stick vacuums and carpet cleaners | 117,100 | 878 | $21 | $18,443 | 5.4 | 51 |
| 32 | Sewing, embroidery, cutting machines | 38,100 | 286 | $45 | $12,859 | 5.4 | 54 |
| 33 | Smart home security (cameras, alarms) | 89,500 | 671 | $25 | $16,781 | 5.4 | 19 |
| 34 | Mesh Wi-Fi and networking | 106,500 | 799 | $22 | $17,572 | 5.4 | 23 |
| 35 | Ranges, wall ovens, induction cooktops | 24,600 | 184 | $60 | $11,070 | 5.4 | 46 |
| 36 | Gaming hardware (consoles, GPUs, PCs) | 84,500 | 634 | $25 | $15,844 | 5.3 | 27 |
| 37 | Dehumidifiers and air purifiers | 117,000 | 878 | $20 | $17,550 | 5.3 | 3 |
| 38 | Dishwashers | 59,700 | 448 | $30 | $13,432 | 5.2 | 17 |
| 39 | 3D printers and filament | 74,700 | 560 | $25 | $14,006 | 5.1 | 45 |
| 40 | Mobility and medical alert (lead-gen) | 18,000 | 135 | $65 | $8,775 | 5.1 | 56 |
| 41 | Drones and action cameras | 57,000 | 428 | $28 | $11,970 | 5.0 | 38 |
| 42 | Monitors | 97,400 | 730 | $18 | $13,149 | 4.9 | 11 |
| 43 | Standby and portable generators | 35,000 | 262 | $36 | $9,450 | 4.9 | 42 |
| 44 | Home EV chargers | 20,900 | 157 | $50 | $7,838 | 4.8 | 32 |
| 45 | Kayaks, SUPs, camping, coolers | 41,800 | 314 | $30 | $9,405 | 4.7 | 55 |
| 46 | Baby gear (car seats, strollers, monitors) | 54,000 | 405 | $25 | $10,125 | 4.7 | 28 |
| 47 | Water heaters (tankless, heat pump) | 31,300 | 235 | $36 | $8,451 | 4.7 | 25 |
| 48 | Pet tech (litter boxes, GPS collars) | 25,000 | 188 | $40 | $7,500 | 4.6 | 58 |
| 49 | Pressure washers, snow blowers, chainsaws | 65,000 | 488 | $20 | $9,750 | 4.6 | 39 |
| 50 | NAS and home storage | 34,400 | 258 | $30 | $7,740 | 4.5 | 52 |
| 51 | Watches, luggage, travel gear | 44,000 | 330 | $25 | $8,250 | 4.5 | 44 |
| 52 | Printers and home office | 123,000 | 922 | $12 | $11,070 | 4.4 | 29 |
| 53 | Headphones, earbuds, speakers, soundbars | 208,500 | 1,564 | $8 | $12,510 | 4.3 | 48 |
| 54 | Dash cams, car audio, EV accessories | 52,500 | 394 | $20 | $7,875 | 4.3 | 31 |
| 55 | Musical instruments | 26,000 | 195 | $28 | $5,460 | 4.0 | 59 |
| 56 | Online courses and certifications | 18,000 | 135 | $35 | $4,725 | 4.0 | 66 |
| 57 | Air fryers, countertop ovens, pizza ovens | 54,800 | 411 | $15 | $6,165 | 3.8 | 63 |
| 58 | Window / portable air conditioners | 55,100 | 413 | $13 | $5,372 | 3.5 | 40 |
| 59 | Central AC / furnaces (installed, lead-gen) | 14,600 | 110 | $30 | $3,285 | 3.4 | 61 |
| 60 | Cordless power tool platforms | 34,500 | 259 | $15 | $3,881 | 3.2 | 62 |
| 61 | Keyboards, mice, docks, peripherals | 85,500 | 641 | $8 | $5,130 | 3.2 | 35 |
| 62 | Coffee makers, kettles, drip brewers | 55,100 | 413 | $10 | $4,132 | 3.0 | 65 |
| 63 | Ceiling, attic, whole-house fans | 21,600 | 162 | $18 | $2,916 | 2.9 | 57 |
| 64 | Range hoods and microwaves | 35,700 | 268 | $12 | $3,213 | 2.8 | 60 |
| 65 | Electric toothbrushes, oral care, shavers | 63,000 | 472 | $8 | $3,780 | 2.8 | 64 |
| 66 | Safes, security hardware, smart locks | 25,000 | 188 | $14 | $2,625 | 2.7 | 50 |
| 67 | Smart thermostats and hubs | 34,200 | 256 | $11 | $2,822 | 2.6 | 67 |
| 68 | Home energy monitors and smart panels | 3,500 | 26 | $15 | $394 | 1.2 | 68 |

## Home systems cluster totals

Thirteen members. Combined volume 548,100 per month, combined market sales 4,111 per month, combined pool $227,294 per month. Members by overall rank: Refrigerators (#2, profit #13), Dehumidifiers and air purifiers (#3, profit #37), Washing machines and dryers (#6, profit #21), Ductless mini-split heat pumps (#7, profit #29), Pool equipment (robots, pumps, heaters) (#15, profit #20), Whole-house water filtration / softeners (#16, profit #4), Dishwashers (#17, profit #38), Home battery + DIY solar kits (#22, profit #14), Portable power stations / solar generators (#24, profit #22), Water heaters (tankless, heat pump) (#25, profit #47), Home EV chargers (#32, profit #44), Standby and portable generators (#42, profit #43), Ranges, wall ovens, induction cooktops (#46, profit #35).
