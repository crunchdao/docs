# Endpoint Deprecation

The following endpoint will be deprecated on January the 1st, 2023.

If you have any question or query, don't hesitate to ask _<mark style="color:purple;">@cruncher\_enzo</mark>_ on our official [Discord Server](https://discord.gg/veAtzsYn3M).

| Line Request                                                                              | Alternative                                                                |
| ----------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| <mark style="color:yellow;">`POST`</mark>`/submission`                                    | <mark style="color:yellow;">`POST`</mark>`/v2/submissions`                 |
| <mark style="color:yellow;">`POST`</mark>`/v1/submission`                                 | <mark style="color:yellow;">`POST`</mark>`/v2/submissions`                 |
| <mark style="color:green;">`GET`</mark>`/v2/weekly-crunches`                              | <mark style="color:green;">`GET`</mark>`/v2/crunches`                      |
| <mark style="color:green;">`GET`</mark>`/v2/weekly-crunches/{id}`                         | <mark style="color:green;">`GET`</mark>`/v3/crunches/{id}`                 |
| <mark style="color:green;">`GET`</mark>`/v2/weekly-crunches/{id}/active-targets`          | <mark style="color:green;">`GET`</mark>`/v3/crunches/{id}/active-targets`  |
| <mark style="color:green;">`GET`</mark>` ``/v2/weekly-crunches/@published`                | <mark style="color:green;">`GET`</mark>`/v3/crunches/@published`           |
| <mark style="color:green;">`GET`</mark>`/v2/weekly-crunches/@published/active-targets`    |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/weekly-crunches/@latest`                      | <mark style="color:green;">`GET`</mark>`/v3/crunches/@latest`              |
| <mark style="color:green;">`GET`</mark>` ``/v2/weekly-crunches/@latest/active-targets`    |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}/dataset-config`                   | <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}`                   |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}/crunches`                         |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@next/dataset-config`                  |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@next/crunches`                        |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@latest/dataset-config`                |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@latest/crunches`                      |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@current/dataset-config`               |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/rounds/@current/crunches`                     |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v2/global-leaderboard`                           | <mark style="color:green;">`GET`</mark>`/v3/leaderboard`                   |
| <mark style="color:green;">`GET`</mark>`/v2/dataset-round-configs`                        | <mark style="color:green;">`GET`</mark>`/v2/rounds`                        |
| <mark style="color:green;">`GET`</mark>`/v2/dataset-round-configs/{id}`                   | <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}`                   |
| <mark style="color:green;">`GET`</mark>`/v2/dataset-round-configs/{id}/round`             | <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}`                   |
| <mark style="color:green;">`GET`</mark>`/v2/dataset-round-configs/{id}/predicted-targets` | <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}/predicted-targets` |
| <mark style="color:green;">`GET`</mark>`/v2/dataset-round-configs/{id}/metadata`          | <mark style="color:green;">`GET`</mark>`/v2/rounds/{id}/metadata`          |
| <mark style="color:green;">`GET`</mark>`/v2/crunches/{id}`                                |                                                                            |
| <mark style="color:green;">`GET`</mark>`/v1/user/notification/unreads`                    |                                                                            |
