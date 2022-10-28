# Schema Migration Script

Update every analysis stored in Song to have meta data matching the latest schema model.

## Quick Run

1. `npm ci`
1. Copy [.env.schema](./.env.schema) file to a new file named `.env`. Populate all fields. See description of env variables in the [Configuration](#configuration) section.
1. `npm run once` to run the script.

## Configuration

Currently there is only one run profile, which will attempt to update all analyses from all studies in Song.

Before running the script, several variables require configuration in order to connect the script to Song and provide authorization. These values can be added to the file `.env`. See details in the following table.

### Environment Variables

Read from `.env` file and provided to the script through the [src/config.ts](./src/config.ts) file

| Variable Name            | Required | Default | Description                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------ | :------: | ------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EGO_HOST                 |   true   |       - | Base URL for requests to Ego. Needed to fetch application JWT for Song Update Authorization.                                                                                                                                                                                                                                                                                 |
| EGO_CLIENT_ID            |   true   |       - | Ego Application client ID, for Song Update Authorization                                                                                                                                                                                                                                                                                                                     |
| EGO_CLIENT_SECRET        |   true   |       - | Ego Application client secret, for Song Update Authorization                                                                                                                                                                                                                                                                                                                 |
|                          |          |         |                                                                                                                                                                                                                                                                                                                                                                              |
| SONG_HOST                |   true   |       - | Base URL for requests to Song.                                                                                                                                                                                                                                                                                                                                               |
| SONG_ENV                 |  false   |       - | Optional descriptor for log file to indicate which song instance is being used.                                                                                                                                                                                                                                                                                              |
| SONG_CONCURRENT_REQUESTS |  false   |     `5` | Maximum number of requests that can be initiated to song. Higher values should complete the update requests faster, but ensure that there is enough capacity on the song service to handle the requests. Too many concurrent requests typically result in frequent timeouts (and possible script crashes) due to limited pool of JDBC connections from Song to its database. |
| SONG_PAGE_SIZE           |  false   |   `100` | Total number of analyses returned per paged request for analyses. 100 seems to be a fine value. Increasing this has a negligable effect on script completion time. This will limit the number of concurrent update requests, so make sure this is larger than the SONG_CONCURRENT_REQUESTS value.                                                                            |
|                          |          |         |                                                                                                                                                                                                                                                                                                                                                                              |
| MIGRATION_CHAIN          |  false   |   `dev` | `dev` or `prod`. The migration chains in each environment are slightly different, so make sure in Production this is set to `prod` or there will be errors                                                                                                                                                                                                                   |
