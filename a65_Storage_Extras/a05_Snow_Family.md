

# **Data Migration**
   1. Is a portable device
   2. Two types
      1. `Snowcone`
         1. 8 - 14TB
      2. `Snowball edge`
         1. 80-210TB
      3. `Snowmobile`
   3. Steps
      1. User -> Snow family -> Ship -> AWS will export to S3
      2. In user side
         1. Snowball client (or) AWS OPsHub is used to download data to snow family
         
# **Edge Computing**

   1. Used to process data which is created on edge location (Ex: Truck, Ship etc...)
   2. How it works?
      1. Use `Snowball Edge` or `Snowcone`
      2. Snowcone: 2 CPU, 4GB RAM, wired or wireless access
      3. Snowball edge
         1. Compute optimized & Storage optimized available
         2. Can run EC2 instances or Lambda on the device
   3. Use case
      1. Data preprocessing
      2. Machine learning
         


# Snowball into Glacier

Not possible to import directly into glacier

**Solution:**

Snowball > S3 > lifecycle policy to Glacier
