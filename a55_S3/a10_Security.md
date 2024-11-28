

# Encryption

1. Methods to encrypt objects
   1. **Server side encryption (SSE)**
      1. Using SSE-S3
         1. Enabled by default
         2. Done using S3 managed keys
         3. Encryption type: AES-256
         4. Must set header `x-amz-server-side-encryption`:`AES256`
      2. Using SSE-KMS
         1. Done using `Key Management Service`
         2. Custom KMS keys have cost 
         3. Advantage
            1. Can audit key usage using `CloudTrail`
         4. Limitations
            1. Can be impacted by KMS API limits (5500, 1000, 3000 reqs based on region)
            2. Upload: Uses GenerateDataKey API
            3. Download: Uses Decrypt KMS API
         5. Must set header `x-amz-server-side-encryption`:`aws:kms`
      3. `DSSE-KMS` (Double encryption based on KMS)
         1. Is new
      4. Using SSE-C
         1. Done using customer provided keys (From CLI & not Console)
         2. S3 will discard key after encryption (Do not store the key)
         3. HTTPS must be used
         4. User
             1. File Upload: Provides File + Key in header
             2. File read: User must provide the key again
   2. **Client side encryption**
      1. Done using client libraries like `S3 Client side encryption library`
      2. Everything happens outside of S3
   
2. Transit encryption
   1. Done using SSL/TLS
   2. S3 has both `HTTP` & `HTTPS` endpoints
   3. Bucket Policy
      1. Enable `aws:SecurityTransport` to force transit encryption
         
3. Bucket Policy
   1. Can be used to force encryption & refuse API call without headers
   2. Is always evaluated before default encryption settings
    

# CORS (`Cross-Origin Resource Sharing`)

1. **Concept**
   1. Is a web browser based mechanism to allow requests to other origins while visiting the main origin
   2. **Origin**: schema (Protocol) + host (domain) + port
      1. Example
         1. Same origin: http://example.com/app1 & http://example.com/app2
         2. Different origin: http://www.example.com/app1 & http://other.example.com/app2
   3. **Request will not be fulfilled unless other origin server allows for CORS headers**
2. **S3 Specific**
   1. If a client makes CORS request on S3 bucket, we need to enable the right CORS headers
   2. We can allow for specific origin or for `*` (all origins)
   3. Example
      1. web browser -GET-> bucket1 (Static website) 
      2. web site returned by has link to fetch image from another bucket (Static website: say bucket2)
      3. Web browser -GET(With Host & Origin in header)-> bucket2 (If not configured to have correct CORS headers, s3 server is going to refuse the request)



# MFA Delete

Is an extra protection to prevent against permanent deletion

1. When MFA is required?
   1. To permanently delete an object version
   2. Suspend versioning of the bucket
2. When MFA is not required?
   1. Enable versioning
   2. List deleted versions
3. How to enable MFA delete?
   1. Versioning of the bucket must be enabled
   2. Only bucket owner can enable/disable MFA delete

# S3 Access logs

Can be used to log all the access (requests, authorized, denied etc...) to S3

1. Constraint
   1. Target logging bucket should be in the same region
2. Note
   1. Do not set the logging bucket to be the monitored bucket (This creates a logging loop)
3. How to setup?
   1. bucket > properties > Server access logging

# S3 Pre-Signed URLs

1. Can be used to provide temporary access to someone
2. These are URLs which are pre-signed and has expiration
   1. **Expiration time**
      1. S3 Console: 1 min to 720 min (ie 12 hours)
      2. CLI: 3600 seconds to 604800 secs (ie 168 hours)
3. Pres signed URL inherits the permission of the user
4. Use case
   1. Say if I want to provide access for a specific file in s3 bucket to someone 
5. How to setup?
   1. bucket > object > Objectio actions > Share with a presigned URL

# S3 Glacier Vault lock

Can be used to adopt a `WORM` (Write once read many) model, where an object in S3 Glacier cannot be  modified or deleted

1. Steps
   1. Create a vault lock policy to restrict deletion
   2. Lock the policy for future edits
2. Use case
   1. Helpful for compliance and data retention
3. Note
   1. Event root user cannot delete the object

# S3 Object lock

1. Similar to Glacier valult lock but for S3 buckets
2. Steps
   1. Enable versioning
3. Constraint
   1. Lock can be adopted at object level & not bucket level
4. Retention modes
   1. Compliance mode
      1. Object can't be overwritten or deleted by any user (root user also)
      2. Retention modes & periods can't be changed
   2. Governance mode
      1. Most users cannot overwrite or delete an object
      2. Some users have special permission to do that
5. Note
   1. Retention period can be extended
   2. Legal Hold
      1. Can be used to protect the object indefinitely, independent from the retention period
      2. Can be placed or removed using IAM permission



# S3 Access points

1. An access point policy can be created to provide scoped access to bucket prefixes to specific set of users
2. Example:
   1. bucket/finance (We can create finance access point)
   2. bucket/sales   (...)
3. Advantage
   1. Instead of a complex S3 bucket policy, the access privileges are all moved to access points which are much cleaner to manage
4. Features
   1. Each access points will have its own DNS name 
   2. Can be set to either
      1. **Internet origin**
      2. **VPC origin(For private traffic))**
         1. Ex
            1. EC2 accessing the bucket
            2. Done by creating a VPC endpoint for EC2 and attaching endpoint policy to allow access to target bucket & access point
5. **S3 object Lambda (Use case)**
   1. Steps
      1. user -> S3 object lambda access point -> Lambda Function -> S3 access point -> S3 bucket
   2. Use case
      1. Analytics
         1. Can be used to get a redacted version of an image object
         2. Redact PII data
      2. Marketing
         1. Can be used to enrich a data







