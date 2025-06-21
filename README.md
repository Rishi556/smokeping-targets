# Smokeping Targets Project
An open and community-driven project to maintain simple to use Smokeping targets with scripting to automatically generate/update your Targets file.

## Introduction
In early 2023 we started offering IP transit services at Eranium, this is a different kind of beast compared to an internet exchange and many other factors come into play. Running an autonomous system with a variety of customers and endusers requires monitoring. A nice and visual way to monitor your network (destinations) is to use Smokeping. Read more about the Smokeping project [here](https://oss.oetiker.ch/smokeping/), it may look outdated but it still works great.

The goal of this project is to build and maintain a standardized list of targets for our Smokepings, free to use by all network operators or network enthusiasts. The aim is to have as much locations as possible.

## Usage
This example assumes you have Smokeping installed using Docker, find out how [here](https://hub.docker.com/r/linuxserver/smokeping).
Download the latest `Targets` file and save it where Smokeping expects it, then replace `Example` with your company name:
```
wget -O /opt/smokeping/config/Targets https://raw.githubusercontent.com/eranium/smokeping-targets/refs/heads/main/config/Targets
```

If you want to generate your own `Targets` file, run `php tools/build.php default SMOKEPING-NAME` and the file will be created/updated in the `config` map.

## Format
In order to coordinate this, we've come up with a simple format for targets saved in JSON, so we can automate/script it. The directory structure is as follows:

```
targets  
│
└───anycast.json
│
└───root.json
│
└───$CONTINENT
│   │
│   └───$COUNTRY.json
│       │   {$TARGET}
│       │   {$TARGET}
|       |   {$TARGET}
```

| Variable | Explanation          |
|:-------:|------------------|
|  `anycast.json`  | A fixed list of known and popular anycast targets. |
|  `root.json`  | A fixed list of the authoritative root servers. |
|  `$CONTINENT`  | Directory name based on two letter ISO 3166 continent code. |
|  `$COUNTRY`  | File containing JSON data, name based on two letter ISO 3166-1-alpha-2 country code. |
|  `$TARGET`  | One target array containing data, stored in a big array with JSON. |

An example JSON file for the Netherlands would be `NL.json`:
```json
[
    {
        "name": "Example ISP",
        "asn": 64512,
        "ipv4": "0.0.0.0",
        "ipv6": "::1",
        "location": "City",
        "category": "isp"
    }
]
```

| Key | Explanation          |
|:-------:|------------------|
|  `"name"`  | Name of the network excluding company registration type. |
|  `"asn"`  | The network AS number formatted as integer. |
|  `"ipv4"`  | IPv4 address to test against, must be a reliable target without aggressive ratelimiting. |
|  `"ipv6"`  | IPv6 address to test against, if there is no IPv6, shame on them and set as `false`. |
|  `"location"`  | The location of the IP address, based on [UN/LOCODE](https://unece.org/trade/cefact/unlocode-code-list-country-and-territory) city name. |
|  `"category"`  | The category of this target, see the table below for clarification. |

| Category | Explanation          |
|:-------:|------------------|
|  `root`  | Authoritative root servers (only used in root.json) |
|  `anycast`  | Anycast network (only used in anycast.json) |
|  `isp`  | Providing internet access to eyeballs. |
|  `gov`  | Government related network. |
|  `edu`  | Educational network like schools and universities. |
|  `content`  | Content oriented network like streaming and hosting. |
|  `other`  | Other or unknown networks. |

## Contribute
Feedback is as always welcome. Contributions in the form of pull requests are even better! Make sure to follow the format. If you don't have the knowledge or the data, you can also consider opening an issue [here based on a template](https://github.com/eranium/smokeping-targets/issues/new?template=target-request.md) so we can try adding it.

## Customization
This project includes an example script `tools/build.php` that generates the Targets file based on all the source data, you should be able to customize this if you want to generate a different format.

## License
Mozilla Public License Version 2.0
