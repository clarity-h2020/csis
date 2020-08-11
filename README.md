# *C*LARTIY *C*limate *S*ervices *I*nformation *S*ystem

Management Project for coordinating the co-creation process of the [CLARITY](https://clarity-h2020.eu/) Climate Services Information System ([CSIS](https://csis.myclimateservice.eu/)). High-level tasks ([issues](https://github.com/clarity-h2020/csis/issues)) are defined in this repository and then broken down into concrete implementable issues in different [sub repositories](https://github.com/clarity-h2020/repositories) and [project boards](https://github.com/orgs/clarity-h2020/projects?query=sort%3Aname-asc).

The CLARTIY CSIS as such consists of a set of docker-containers and is deployed as [production](https://csis.myclimateservice.eu/) and [development](https://csis-dev.myclimateservice.eu/) of two virtual serves managed by [Austrian Institute Of Technology](https://www.ait.ac.at/).

## CSIS Development System

The CSIS Development System is deployed at virtual server `csis-dev.ait.ac.at` and is reachable under the URL [csis-dev.myclimateservice.eu](https://csis-dev.myclimateservice.eu/). Each subfolder of the `/docker/` directory contains a docker-compose.yml file to control the respective service groups. For each service groups, a respective GitHub repository exists in [organisation clarity-h2020](https://github.com/clarity-h2020/). It contains a `readme.md` file that provides more information regarding implementation, configuration, backups, upgrading, etc.

![csis-dev.ait.ac.at](https://raw.githubusercontent.com/clarity-h2020/csis/master/images/csis-dev.ait.ac.at.svg)

### hoster

A simple `etc/hosts` file injection tool to resolve names of local Docker containers on the host. Instead of exposing ports of a container on the docker host (localhost), you can access the container by its name directly, e.g. `csis-postgis:5432` for accessing the CSIS Drupal 8 PostGIS database running in `csis-postgis` container.

- **deployed at**: `/docker/000-hoster`
- **repository**: [docker-hoster](https://github.com/clarity-h2020/docker-hoster/)

### ngnix-proxy

The nginx reverse proxy is used to expose several internal docker services of [CLARITY](https://clarity-h2020.eu/)'s Climate Services Information System ([CSIS](https://github.com/clarity-h2020/csis/)) on `myclimateservice.eu` subdomains.

- **deployed at**: `/docker/010-ngnix-proxy`
- **repository**: [docker-compose-letsencrypt-nginx-proxy-companion](https://github.com/clarity-h2020/docker-compose-letsencrypt-nginx-proxy-companion)
- **public endpoint**: [csis-dev.myclimateservice.eu](https://csis-dev.myclimateservice.eu/)

### csis

CSIS Drupal 8 Instance of the **development system** with  PostGIS 10 as backend. 

- **deployed at**: `/docker/100-csis`
- **repository**: [docker-drupal](https://github.com/clarity-h2020/docker-drupal/)
- **public endpoint**: [csis-dev.myclimateservice.eu](https://csis-dev.myclimateservice.eu/)

Currently, the following custom modules and integrated apps ([Building Blocks]()) are deployed together with the Drupal 8 system:

- [CSIS Helpers Drupal Module](https://github.com/clarity-h2020/csis-helpers-module)
- [CSIS Helpers JavaScript Module](https://github.com/clarity-h2020/csis-helpers-js)
- [CSIS Drupal Theme](https://github.com/clarity-h2020/clarity-theme)
- [Map Component](https://github.com/clarity-h2020/map-component)
- [Simple Table Component](https://github.com/clarity-h2020/simple-table-component)
- [Scenario Analysis](https://github.com/clarity-h2020/scenario-analysis/issues)

### ckan

[CKAN](https://ckan.org/) is CLARITY's online meta-data catalogue that reports on all datasets produced and used within the project and thus represents a 'living' [Data Management Plan](https://github.com/clarity-h2020/data-management-plan).

- **deployed at**: `/docker/500-ckan`
- **repository**: [ckan](https://github.com/clarity-h2020/ckan/)
- **public endpoint**: [csis-dev.myclimateservice.eu](https://ckan.myclimateservice.eu/)

Access to CKAN password protected. Refer to the CLARTIY mailing list or `/docker/readme.md` for further information.

### duplicity

[duplicity](http://duplicity.nongnu.org/) is backup tool that can be used to perform incremental backups of CSIS container's data volumes. 

- **deployed at**: `/docker/999-duplicity`
- **repository**: [docker-duplicity](https://github.com/clarity-h2020/docker-duplicity)

## CSIS Production System

The CSIS Production System is deployed at virtual server `csis.ait.ac.at` and is reachable under the URL [csis.myclimateservice.eu](https://csis.myclimateservice.eu/). Synchronisation between deployment and production system is described in [Synchronisation between DEV and PROD](https://github.com/clarity-h2020/docker-drupal#synchronisation-between-dev-and-prod).

![csis.ait.ac.at](https://raw.githubusercontent.com/clarity-h2020/csis/master/images/csis.ait.ac.at.svg)

For most service groups that are deployed at the production virtual server, separate development and production branches exist in the respective GitHub repository. The name of the branches are `dev` and `master` or `csis-dev.ait.ac.at` and `csis.ait.ac.at`. Refer to [docker-duplicity](https://github.com/clarity-h2020/docker-duplicity#backed-up-directories) and [map-component](https://github.com/clarity-h2020/map-component#map-component) for an example.

### hoster

See [hoster](#hoster) @ development system.

### ngnix-proxy

See [ngnix-proxy](#ngnix-proxy) @ development system. 

The **public endpoint** is [csis.myclimateservice.eu](https://csis.myclimateservice.eu/). The production configuration is maintained in branch [csis.ait.ac.at](https://github.com/clarity-h2020/docker-compose-letsencrypt-nginx-proxy-companion/tree/csis.ait.ac.at/nginx-data/vhost.d).

### csis

See [csis](#csis) @ development system.

The **public endpoint** is [csis.myclimateservice.eu](https://csis.myclimateservice.eu/). The actual production and development Drupal site configuration is stored in the separate **private** [clarity-csis-drupal](https://scm.atosresearch.eu/ari/clarity-csis-drupal) repository. The custom modules and integrated apps are usually deployed from the `master` branch.

### duplicity

See [duplicity](#docker) @ development system.

The production configuration is maintained in branch [csis.ait.ac.at](https://github.com/clarity-h2020/docker-duplicity/blob/csis.ait.ac.at/filelist.txt).

## Other services

The CSIS relies on several external services and application that are not deployed as docker containers on `csis.ait.ac.at` or `csis-dev.ait.ac.at` virtual servers.

### myclimateservices profile service

**TODO** [@fgeyer16](https://github.com/fgeyer16): Please update [readme.md](https://github.com/clarity-h2020/csis/#myclimateservices-profile-service) and provide a description for profiles service and single-sign-on. See [ckan](https://github.com/clarity-h2020/ckan/blob/csis-dev.ait.ac.at/README.md) or [docker-drupal](https://github.com/clarity-h2020/docker-drupal/blob/dev/README.md) for an example.

- **repository**: ???
- **public endpoint**: [profile.myclimateservices.eu](https://profile.myclimateservices.eu/)

### AIT EMIKAT

**TODO** [@humer](https://github.com/Humer): Please update [emikat/readme.md](https://github.com/clarity-h2020/emikat/blob/master/README.md) and [csis/readme.md](https://github.com/clarity-h2020/csis/blob/master/README.md#ait-emikat) and provide implementation and deployment description for EMIKAT. See [ckan](https://github.com/clarity-h2020/ckan/blob/csis-dev.ait.ac.at/README.md) or [docker-drupal](https://github.com/clarity-h2020/docker-drupal/blob/dev/README.md) for an example.

- **repository**: [docker-duplicity](https://github.com/clarity-h2020/docker-duplicity)
- **public endpoints**: [See Services endpoints \(used by CSIS\)](https://github.com/clarity-h2020/csis/wiki/Services-endpoints-\(used-by-CSIS\))

### ATOS GeoServer

**TODO** [@DanielRodera](https://github.com/DanielRodera): Please update [readme.md](https://github.com/clarity-h2020/csis/blob/master/README.md#atos-geoserver) and provide deployment description for ATOS GeoServer. See [ckan](https://github.com/clarity-h2020/ckan/blob/csis-dev.ait.ac.at/README.md) or [docker-drupal](https://github.com/clarity-h2020/docker-drupal/blob/dev/README.md) for an example.

- **repository**: n/a
- **public endpoint**: [geoserver.myclimateservice.eu](https://geoserver.myclimateservice.eu/geoserver/web/)

### METEOGRID Transport Application

**TODO** [@ghilbrae](https://github.com/ghilbrae): Please update [readme.md](https://github.com/clarity-h2020/csis/blob/master/README.md#meteogrid-transport-application) and provide a description for the Transport Application. See [ckan](https://github.com/clarity-h2020/ckan/blob/csis-dev.ait.ac.at/README.md) or [docker-drupal](https://github.com/clarity-h2020/docker-drupal/blob/dev/README.md) for an example.

- **repository**: [gitlab.meteogrid.com/meteogrid/emmet](https://gitlab.meteogrid.com/meteogrid/emmet)
- **public endpoint**: [clarity.saver.red](https://clarity.saver.red/)



