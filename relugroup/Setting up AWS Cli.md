
https://relugroup.atlassian.net/wiki/spaces/Engineerin/pages/66584589/Connect+to+database

I downloaded the Session Manager and needed to configure the credentials file (no extension) in users/Franco.
I already had a credential for my personal account so I needed to add a new profile
```
[relugroup]
access
secret
```
Now, when running any command I can add `--profile relugroup` and it will use those credentials instead of my own


### Connecting to the database

Not sure how it works but it seems that with the CLI I can connect to some port of the AWS account (which instance/service?)

Then with Docker I run an postgres admin image that somehow is connected to the AWS db.

I can then connect in jupyter to the db using the credentials