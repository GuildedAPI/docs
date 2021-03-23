# Groups

A group is a sub-section of a [team](/resources/team) that allows for custom membership based on roles. They can have a custom icon and name (under the main team's name) in the sidebar, making them somewhat of a middle ground between a permission-locked category and a separate team entirely.

### Group Object

###### Group Structure

| Field                           | Type                                         | Description                                                           |
|---------------------------------|----------------------------------------------|-----------------------------------------------------------------------|
| id                              | [generic id](/reference#generic-object-ids)  | the id of this group                                                  |
| name                            | string                                       | the name of this group                                                |
| description                     | ?string                                      | the description of this group                                         |
| priority                        | integer                                      | where this group is visually the sidebar (base group is 1, not 0)     |
| type                            | string                                       | the type of this group. unsure in what context it would not be 'team' |
| avatar                          | ?string                                      | the group's avatar url                                                |
| banner                          | ?string                                      | the group's banner url                                                |
| teamId                          | [generic id](/reference#generic-object-ids)  | the id of the team that this group is part of                         |
| gameId                          | ?integer                                     | the id of the game (if any) that this group has been assigned         |
| additionalGameInfo              | dictionary                                   | ???                                                                   |
| visibilityTeamRoleId            | integer                                      | the primary role id that is allowed to view this group                |
| visibilityTeamRoleIds           | array of integer                             | all role ids that are allowed to view this group                      |
| additionalVisibilityTeamRoleIds | ?array of integer                            | extra role ids that are allowed to view this group                    |
| membershipTeamRoleId            | integer                                      | the primary role id that is a member of this group                    |
| membershipTeamRoleIds           | array of integer                             | all role ids that are a member of this group                          |
| additionalMembershipTeamRoleIds | ?array of integer                            | extra role ids that are a member of this group                        |
| isBase                          | boolean                                      | whether this group is the base group, or "Server home" in the UI      |
| createdAt                       | ISO8601 timestamp                            | when this group was created                                           |
| createdBy                       | ?[generic id](/reference#generic-object-ids) | the id of the user who created this group (null if base)              |
| updatedAt                       | ?ISO8601 timestamp                           | when this group was last updated (edited)                             |
| updatedBy                       | ?[generic id](/reference#generic-object-ids) | the id of the user who last updated (edited) this group               |
| deletedAt                       | ?ISO8601 timestamp                           | when this group was deleted                                           |
| customReactionId                | ?integer                                     | the custom emoji assigned to this group (just spitballing here)       |
| isPublic                        | boolean                                      | whether this group can be seen without being in the server            |
| archivedAt                      | ?ISO8601 timestamp                           | when this group was archived                                          |
| archivedBy                      | ?[generic id](/reference#generic-object-ids) | the id of the user who archived this group                            |
| membershipUpdates               | array                                        | ???                                                                   |
