

##Advantage

1. Faster boot time: As our software can be prepackaged
2. **AMI Source**
   1. Public AMI (AWS provided)
   2. Own AMI
   3. AWS maketplace AMI

## Limitation
1. Is limited to a region (Possible to copy the AMI to another region & use it in EC2 instance)    

## How to create AMI?

1. Start EC2 & Customize
2. Stop instance
3. Build an AMI (This creates EBS snapshots)
4. Launch instance from other AMI