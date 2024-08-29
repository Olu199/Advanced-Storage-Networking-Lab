**Default Snapshot Scheduling and Retention**

In NAS environments, the default snapshot schedule typically combines hourly, daily, and weekly snapshots, retaining up to 10 snapshots. Whenever a new snapshot is created, the oldest snapshot is deleted, ensuring that 10 consistent snapshots are always maintained.

**Custom Snapshot Policies**

You can create custom snapshot policies with different schedules and retention strategies. Each volume can be assigned a different snapshot policy, allowing flexibility in managing snapshots. The maximum number of snapshots per volume is 255.

**Snapshot Scheduling Strategies**

It's important to note that snapshots do occupy disk space. If users rarely lose files and any lost files are noticed quickly, the default snapshot policies are often sufficient. With a file change rate of 5-10% each week, the default policy typically consumes only 10-20% of the disk space, making it a practical choice.

However, if file loss is frequent or takes a while to be noticed, you may want to retain more snapshots and delete copies less often. For very active volumes, consider taking snapshots every hour or every few hours and retaining them for a short period. In extreme cases, you may even choose to turn off snapshot copies if they are not needed.

After a new, high-risk volume has been in use for some time, it's advisable to monitor the disk space consumed by snapshot copies and how often users need to recover lost files. This information can help you adjust your snapshot policies as necessary.

**Snapshot Copy Reserve**

The default snapshot reserve for NAS volumes is 5% of the volume's space. The active file system cannot use this reserved space.

In some cases, if there isn’t enough space reserved for snapshots, deleting files may not free up space because the blocks are still locked by snapshots. However, if there is sufficient space reserved, deleting files in the active file system will indeed release space.

It's crucial to reserve enough disk space for snapshots, but over-reserving can lead to wasted space. For example, if snapshots only consume 10% of disk space, setting a 20% reserve would result in inefficient use of storage. A snapshot reserve set at 12-15% would provide a safe margin, ensuring that deleting files will free up space even when the active file system is full.

**Restoring from Snapshots**

If snapshot directory visibility is enabled on a volume, users can restore files from snapshots themselves. However, it's important to note that client-initiated restores can be disruptive to other users.

On Windows, restoring files can be as simple as copying from the `\~snapshot` directory or using the Windows "Previous Versions" tool. For this to work, directory access must be enabled on both the volume and the share, allowing users to use both tools. Note that the snapshot directory is hidden on Windows, so users will need to enable the option to show hidden directories.

On Unix systems, the snapshot directory is visible at `\.snapshot`.

**FlexClone (Instant Copy)**

FlexClone, also known as an instant copy, is a readable and writable clone of an existing volume that inherits all of the volume’s configurations. Like a snapshot, FlexClone uses pointers to reference the data on the parent volume. This means it does not occupy additional space until new data is written to the clone or changes are made to the parent volume. FlexClone allows for rapid deployment and testing scenarios without the need for additional storage space, making it a highly efficient tool for managing data.
