SortOrder: 2
# About Wix Members

[Wix Members area](https://support.wix.com/en/article/about-members-area) provides site members with a personal account area for social communities or non-social businesses. 
Members can choose if their account is private or public and set up their profile.

## Member Types

### General Status

**Blocked** - this member can not log-in to the site. 

**Pending** - this member has requested to join the site and is waitng for approval.

### Privacy Status

**Private** - this member's data is available only to themselves and the site owner.

**Public** - this member's data is available to all.

### Activity Status

**Muted** - this member may not perform social actions on this site (write/like/comment on posts, etc.).

**Active** - this member may perform social actions on this site (write/like/comment on posts, etc.).

## Query argument

### FieldSet

Fieldset in query defines how much data the endpoint will return.

**PUBLIC** - Default fieldSet. Request with this fieldSet value returns: ID and profile object (status, privacyStatus and activityStatus will return with UNKNOWN values).

**FULL** - Complete data fieldSet. Request with this fieldSet value returns: ID, status, contact object, profile object, privacyStatus, activityStatus, createdDate and updatedDate.

## Create Member Flow

Member creation flow is visualized and described below:

![alt text](https://s3.amazonaws.com/wixplorer-readme-images/members%2Fcreate-member-flow.png)

### 1. Create Member Request. 

Call [CreateMember]() to create site member for particular login email.
The new member is returned in the response. Save `member.loginEmail` for later use.

### 2. Send Set Password Email. 

Call [SendSetPasswordEmail](https://bo.wix.com/wix-docs/rest/drafts/member-authentication/authentication/send-set-password-email) 
with `member.loginEmail` to send member an email to set their password.
