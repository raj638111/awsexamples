
# 101

1. Can be attached to 
   1. User (Also called `Inline Policy`)
   2. Group

# Structure

An IAM policy consist of
1. `Version`
   1. Usually `2012-10-17`
2. `Id`
   1. Id for the policy
3. `Statement`
   1. One or more individual statements
   2. Consists of,
      1. `Sid`: Identifier of the statement
      2. `Effect`: Allow or Deny
      3. `Principal`: account / user / role to which the policy is applied to
      4. `Action`
         1. List of actions that is `Effect`ed
         2. Example: `iam:Get*`
      5. `Resource` 
         1. List of resources to which the actions applied to
         2. Examples:
            1. `*`
      6. `Condition`: When this policy is in effect