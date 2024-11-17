

1. **Typical EC2 states**
   1. Stop - Data on disk (EBS) is kept intact
   2. Terminate - Root EBS volume will be destroyed
   3. Start
      1. First start: OS boots & EC2 user data script is run
      2. Following start: OS boots up
      3. Applications starts (Takes time for this step as caches gets warmed up)
2. **Advantage with Hibernate**
   1. In memory state (RAM) is preserved
   2. Instance boot is much faster
   3. Under the hood: RAM state is written to root EBS volume
   4. Use case
      1. Long running process which we do not want to stop
      2. Need fast boot up
   5. Limits
      1. RAM size should be less than 150 GB
      2. No support for bare metal instances
      3. Root volume should be encrypted
      4. Cannot hibernate no more than 60 days