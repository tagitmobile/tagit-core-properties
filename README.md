# tagit-core-properties

The repository for sharing the default application properties and samples for each version.

# Introduction

As Spring Boot evolves, more and more configuration dials and knobs are introduced. These are all controlled via the application properties. This is in-line with the `The Twelve-Factor App` metholody specifically the third factor [III. Config - Store config in the environment](https://12factor.net/config).

This same methodology we are following for both Mobeix and TagitCore.

## Config

### Store config in the environment

An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc). This includes:

- Resource handles to the database, Memcached, and other backing services
- Credentials to external services such as Amazon S3 or Twitter
- Per-deploy values such as the canonical hostname for the deploy

Apps sometimes store config as constants in the code. This is a violation of twelve-factor, which requires strict separation of config from code. Config varies substantially across deploys, code does not.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.
 
Another aspect of config management is grouping. Sometimes apps batch config into named groups (often called “environments”) named after specific deploys, such as the development, test, and production environments in Rails. This method does not scale cleanly: as more deploys of the app are created, new environment names are necessary, such as staging or qa. As the project grows further, developers may add their own special environments like joes-staging, resulting in a combinatorial explosion of config which makes managing deploys of the app very brittle.

In a twelve-factor app, env vars are granular controls, each fully orthogonal to other env vars. They are never grouped together as “environments”, but instead are independently managed for each deploy. This is a model that scales up smoothly as the app naturally expands into more deploys over its lifetime.

> **Reference**
> 
> [III. Config - Store config in the environment](https://12factor.net/config).

## Mandatory Properties

### [component-default.properties](/component-default.properties)

The recommended default `application.properties` for all **TagitCore Rest** Spring Boot applications.

Use this as your template and add the custom properties at the bottom. Each property can be overridden subsequently via `Mobeix Cloud Config (mxconfig)` profiles and labels.

> **Mandatory from Mobeix 7.5**
>
> Mobeix 7.5 applications must follow these properties.

### [mxprocess-default.properties](/mxprocess-default.properties)

The recommended default `tagit-core.properties` for all **TagitCore** Spring applications deployed alongside Mobeix Platform's `mxprocess` component. Compared to TagitCore Rest properties, there are less configurable properties available since in this flow, a new TagitCore Spring Application Context is loaded during the [Mobeix Filter Execution](https://edocs1.tagitmobile.com/confluence/display/PPDG/Getting+Started+-+Server+Layer#GettingStartedServerLayer-MobeixFilterConfiguration).

Use this as your template and add the custom properties at the bottom. Each property can be overridden subsequently via `Mobeix Cloud Config (mxconfig)` profiles and labels.

> **Mandatory from Mobeix 7.5**
>
> Mobeix 7.5 applications must follow these properties.


## Mobeix Cloud Config Profile Properties

These properties are recommended to be inserted by Flyway scripts to the `Mobeix Cloud Config (mxconfig)` table `mx_comp_config.`

> **Mobeix Extensions Flyway Script Versioning**
>
> For product Mobeix Cloud Config properties, use the `mobeix.ext.flyway.location-suffix=<component_alias>/mbx/` location.
>
> For customization and extension Mobeix Cloud Config properties, use the `mobeix.ext.flyway_ext.location-suffix=<component_alias>/mbx_ext/` location.

### Environment Profiles

Use these profiles to switch from Development to Production configuration.

Production configuration will usually produce less verbose logs.

#### [component-env-profile-dev.properties](/component-env-profile-dev.properties)

The `dev` profile properties for development and debugging. Useful in DEV, QA and UAT environments.

#### [component-env-profile-prod.properties](/component-env-profile-prod.properties)

The `prod` profile properties for production. Useful in PREPROD and PROD environments.

### Database Profiles

Use these profiles to switch DB connectivity to a different RDBMS type:
- MySQL (default)
- Oracle 
- Microsoft SQL Server 

#### [component-db-profile-mysql.properties](/component-db-profile-mysql.properties)

The `mysql` profile properties for connecting to a MySQL database.

#### [component-db-profile-oracle.properties](/component-db-profile-oracle.properties)

The `oracle` profile properties for connecting to a MySQL database.

#### [component-db-profile-sqlserver.properties](/component-db-profile-sqlserver.properties)

The `mssql` profile properties for connecting to a MySQL database.