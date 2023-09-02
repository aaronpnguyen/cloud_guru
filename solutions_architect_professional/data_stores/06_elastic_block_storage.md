# Elastic Block Storage (EBS)
- Think virtual hardrives
- Can only be used with EC2
- Tied to a single availability zone
- Variety of optomized choices for IOPS, throughput, and costs
- Space must be provisioned when first setting up a volume
    - Users will be billed for all the space provisioned, it is not pay for what you use

## EBS vs Instance Store
- Instance store
    - Locked to EC2 instances
    - Available only when EC2 instance is running
    - Temporary

- EBS Volumes
    - Not locked to EC2 instances
    - Can be attached or unattached to different EC2 Instances
    - Can create snapshots

- Why use one over the other?
    - Instance store is a directly attached storage
        - Instance store has better performace
    - EBS is based over the network
        - EBS is a little slower and may not be as performant

## Snapshots
- Cost effective and easy backup strategy
    - Example: Set a cloudwatch job to take periodic snapshots of volumes
- Easy to share datasets/snapshots with other users or accounts
    - Example: Take snapshot of a volume and share with other account(s)
- Able to migrate a system to a new availability zone or region
    - Example: Take a snapshot of the volume then recreate the volume in the new region or availability zone
- Can easily convert unencrypted volume to an encrypted volume

- How do they work?
    1. Given a 64 block, we use only 4 blocks
        - Creating a snapshot will contain all 64 blocks, even though 4 are being used
        - We will be billed for using all 64 blocks
    2. In addition to those 4 blocks, we now add 2 blocks
        - Creating a new snapshot will only contain the difference from the previous snapshot, so this snapshot contains only 2 blocks
    3. Now onto our third snapshot, we delete one block and create another
        - This snapshot will again, only contain the differences, which is the deletion of one block and addition of the new one

- Frequent snapshots do not equate to the same size or cost as the full size of the original EBS volume

- How do we restore to a previous version?
    - If none of the snapshots have been deleted and we want to revert to snapshot one from the previous example, it points to snapshot one and applies the changes from snapshot 2
- What if snapshot 2 does not exist? Can we still restore snapshot 3?
    - We can, however we cannot restore back to snapshot 2
- What if we delete the original snapshot? Can we still restore later versions of snapshots? (Snapshot 2/3)
    - No, if it is deleted the first snapshot will be incorporated with the second snapshot automatically and can be restored as if it was never deleted

## Amazon Data Lifecycle Manager
- Can schedule snapshots for volumes or instances every `x` hours
- Can create retention rules to remove/delete stale snapshots
    - Stale snapshots that have not been removed/delete are still billable