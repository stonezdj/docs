# Proposal: `Refactor admin server`

Author: `Daojun Zhang/stonezdj`

Discussion: 
[5080](https://github.com/goharbor/harbor/issues/5808)
[5525](https://github.com/goharbor/harbor/issues/5525)

## Abstract

Admin server is component used to manage the configuration for harbor. this proposal is going to provide a simple and mantainable configuration management.

## Background

In previous implementation of harbor admin server, it is a dependent http server to manage configuration, the prepare program parses harbor.cfg and generate env file for harbor and it serves the adminserver, the adminserver load these environment variables to database and also read these configuration from database when ui or job service retrieve/update configurations. some painpoints of mananging configurations.

1. There is a standalone admin server container, actually this server is only used configuration read/write, the api is exposed by core/api actually. there is no need to separate it from core api.

2. There are lots of code change when we need a new configuration item, it is error prone.

3. The system setting/user setting are messed together make the configuration file too large to maintain.


## Proposal

Because refactor adminserver involve lots of code change, in order to We are going to refactor the admin server with following tasks.

1. Remove adminserver container, all configuration related API is handled by core api. also include migrate some other admin server related API to core api.

2. Seperate user settings from harbor.cfg, and keep system settings in harbor.cfg, and system setting is set to env and can not change. user setting are read/write by core api, system settings is read only.

3. Remove adminserver related build script, docker file and code.

4. Refactor configuration item management, provide a unified type conversion, validation, default value setting, read, write.

## Non-Goals

 It is only a refactor of adminserver, no new feature.

## Rationale

 [A discussion of alternate approaches and the trade offs, advantages, and disadvantages of the specified approach.]

## Compatibility

  1. The admin server container is only used internal, after remove adminserver container,  some test scripts to ping the status of adminserver container might fail, need to remove it.

  2. Some test automation need to updated to configure user settings.

  3. Harbor tile configuration implementation need to change for some user setting is not include in harbor.cfg.


## Implementation

 Breakdown into following work items, Daojun Zhang will work on this items in harbor 1.7.0. 

1. Remove adminserver container.

2. Seperate user settings from harbor.cfg

3. Remove adminserver related build script, docker file and code.

4. Refactor configuration item management.

5. Change harbor tile configurations

## Open issues (if applicable)

No