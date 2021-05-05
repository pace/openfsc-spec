<p align="center"><a href="//connectedfueling.com"><img src="assets/connected-fueling-logo.svg" width="50%" /></a></p>

---

# OpenFSC Protocol Specification

OpenFSC (Open Fueling Site Connect) is a protocol specification for connecting petrol stations to the [Connected Fueling platform](//connectedfueling.com).

## Versioning

The specification is versioned based on `major.minor` notation. The `major` version will be incremented with any technical change to the specification format or way it should be interpreted. Changes to the `minor` version will be reserved for any clarification or correction of clerical errors associated with the specification language.

**Available Versions:**

- [Version 1.0](1.0/openfsc-spec.md) (Released 2020-08-03)

## Implementations

The OpenFSC protocol is used by several companies, apps and products under the hood to drive their innovative offerings and solutions:

**Platforms & Customer Solutions built upon Connected Fueling platform** _(alphabetically sorted!)_

- [Connected Fueling for Web](//fuel.site) is a browser based solution to pay directly at the gas station pump with your smartphone without installing an app.
- [DKV Mobility App](//www.dkv-euroservice.com/de/leistungen/leistungen-and-services/weitere-produkte/dkv-app/) supports drivers throughout Europe in their search for gas stations and vehicle service stations with DKV acceptance.
- [Hoyer Energie + Technik](//www.hoyer.de) is a mobile app to pay mobile at Hoyer stations in whole Germany.
- [PACE Car](//www.pace.car) is a aftermarket telematics platform for cars developed by [PACE Telematics GmbH](//business.pace.car).
- [PACE Drive](//drive.pace.car) is a gas station finder app for whole Europe. The app is free to use and can be found in the [Apple App Store](//apps.apple.com/app/apple-store/id1483917851) and the [Google Play Store](//play.google.com/store/apps/details?id=car.pace.drive).
- .. there are many more under development - ready to be released, soon.

**Cashier/POS Systems & Forecourt Controllers** _(alphabetically sorted!)_

- [Global-Office](http://www.ratio-elektronik.de/produkte/global-office) from [Ratio Elektronik](http://www.ratio-elektronik.de)
- [TaskSTAR](http://www.tasksystems.de/?page_id=23) Cash register systems from [Task Technology](http://www.tasksystems.de/)
- .. there are many more under development - ready to be released, soon.

**Libraries & SDKs**

- JavaScript Client Library: [On GitHub](//github.com/pace/openfsc-client-js)
- C#/.NET Client Library: [On GitHub](//github.com/pace/openfsc-client-dotnet)

**Testing & Integration Utilities**

- [Fueling Simulator](https://fueling-simulator-app.sandbox.k8s.pacelink.net/) is a little frontend used to simulate a gas station for development or presentation purposes. It uses the **JavaScript Client Library** to connect to Connected Fueling via OpenFSC protocol.
- [OpenFSC Debugger](https://openfsc-debugger.sandbox.k8s.pacelink.net) _(coming soon)_ is a OpenFSC proxy, logger and protocol analyzer that can be used to debug or verify protocol implementations.
- [PACE Developer Hub](//developer.pace.cloud) _(coming soon)_: Beside tons of documentation for all the PACE services, the **Developer Hub** has the ability to create credentials for the PACE Cloud Sandbox environment to develop, test and verify your own protocol implementation or integration.

## Contributing

If you are interested in contributing please refer to the [CONTRIBUTING.md](CONTRIBUTING.md) file.

## Authors

[Philip Blatter](//github.com/pnull), [Vincent Landgraf](//github.com/threez), [Nico Mürdter](//github.com/Smuerdt)

## License

The text of this specification is licensed under a [MIT License](LICENSE.md).
However, the use of this spec in products and code is entirely free:
there are no royalties, restrictions, or requirements.

---

# About Connected Fueling

Connected Fueling allows gas stations to secure market share, gain new customers, optimise processes, increase brand awareness and more:

- Chance for true **additional "on-top" business** for the fast-movers in the market as customers will flock to stations where convenient Connected Fueling is available.
- **Attracting customers** to gas stations for other, more sustainable reasons (i.e. convenience, user experience, time savings) than the cheapest price.
- Chances for **"on-top" revenue through data driven up- & cross-selling** via smart offerings (e.g. buyers of premium fuels can be offered premium motor oil) and couponing (e.g. coffee for anyone who has been on the road longer than 2 hours).
- Keeping stations **open overnight** by upgrading regular dispenser pumps (Post Pay or Pre-Auth) to "virtual automatic fuel terminals" (Pre-Auth).
- Higher customer flow rate, i.e. **making sites more efficient** by increasing throughput of dispenser pumps. After refueling, pumps become available more quickly (no more waiting for people to go inside, pay at the POS, come back, move their car).
- **Shortening queues** in the shop as people who only want to pay for fuel can do so on the forecourt.
- Making the **shopping experience better for buyers in the shop** as they feel less pushed by impatient customers paying solely for fuel.
- Building a digital **customer relationship** that can be nurtured over the customer lifetime.
- Creating customer loyalty by offering **state-of-the-art, modern, digital user experience**s.
- Getting a better **grip on data** of your gas station network and the customer flow.
- Adding **innovative**ness and digital prowess to your gas station brand personality.
- Attraction of innovative **early adopter** types who spend more and opt for **premium fuels** more frequently.
- Making the shop **more efficient** overall as there is less manual work at POS (less people to handle).
- **Less cash handling will save costs** and make the whole operation safer.

For more information, have a look at our website: [connectedfueling.com/partner](//www.connectedfueling.com/partner)
