# Referendums

To ensure that there is no controlling entity in the team, all decisions affecting members must be voted on. 🗳️

## Unanimity consensus

All votes shall be unanimous.

At the first `AGAINST`, the referendum is considered as `REJECTED`.

Once all members have voted `FOR`, the referendum is considered as `APPROVED`.

## Expiration

Polls only have a window of 3 days before they are considered EXPIRED.\
\
After that, no new votes will be accepted.

## Referendum Types

### Inviting a user

You can invite other users to join your team.&#x20;

Once the decision has been made, the target user will receive an invitation.

<figure><img src="../../.gitbook/assets/image (99).png" alt=""><figcaption><p>Invite a user form</p></figcaption></figure>

#### Conditions

* The target user must not already be invited
* The target user must not be in a team
* The new team size must be under 5

### Kicking a user

A user can only be kicked from the team if he or she accepts the kick.

This is the only way someone can leave a team. (apart from [disbanding](referendums.md#disbanding-a-team)).

<figure><img src="../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Kick a member form</p></figcaption></figure>

#### Conditions

* The target user must be a member of the team
* The target user must not be the last member of the team (use a [disband](referendums.md#disbanding-a-team) instead)

### Accepting a user

Teams that advertise for new team members can receive requests for people to join their team. When a user makes a request, it triggers a referendum.

<figure><img src="../../.gitbook/assets/image (106).png" alt=""><figcaption><p>View of a user searching for a team</p></figcaption></figure>

#### Conditions

* The target user must not be already invited
* The target user must not be in a team
* The new team size must be under 5

### Disbanding a team

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

## Voting

<figure><img src="../../.gitbook/assets/image (98).png" alt=""><figcaption><p>Voting for kicking a communist</p></figcaption></figure>

## Post-vote failure

Some referendum can fail to be deployed if some conditions are not true anymore, a few examples includes:

* The team is full
* An invited user joined another team
* An invited user rejected the invitation

Depending on the reason, teams can re-create another referendum.

{% hint style="info" %}
If a team of one member try to create a referendum, it will be automatically accepted.
{% endhint %}
