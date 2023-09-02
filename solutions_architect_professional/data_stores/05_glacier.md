# Amazon Glacier

## General Idea
- Long term archive solution
- Cheap, slow to respond, not accessed frequently
- Integrated with AWS S3 via lifecycle management
- Used by AWS Storage Gateway Virtual Tape Library
    - Stores backups
- Faster retrieval speed options if paid for
    - Not super fast and should not replace S3 standard

## Access and Policies
- Glacier policy
    - Defines what rules the vault must obey
        - Enforce rules like no deletes or MFA
- Glacier access
    - Given through IAM to users and roles
- Glacier Archives
    - Immutable/permanent
        - Can be deleted or overwritten with a new version
    - Files, zip, tar, etc
    - Max size for an archive is 40 TB (S3 is 5TB)
- Glacier Vault Lock
    - Immutable/permanent
    - Can be overwritten or deleted
    - When initiated, there is a 24 hour timeout to confirm that the lock performs
        - Within the 24 hours, the lock can be aborted
        - If the lock is not confirmed, it will abort
    - Completing the lock applies the vault lock permanently