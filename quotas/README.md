Starting with Eucalyptus 3.0, quotas are introduced to enforce resource limits on accounts and users.  The IAM policy language has been extended to add an additional effect key; "Limit".  The "Limit" key signifies to the policy parser that the policy statement is a quota. As with standard IAM policies, a quota statement contains both "Action" and "Resource" fields. This allows enforcement of the quota based on matched requests; for example, limit the number of volumes when the ec2:CreateVolume action is called:

>{  
>"Statement":[{  
>"Sid":"1",  
>"Effect":"Limit",  
>"Action":"ec2:CreateVolume",  
>"Resource":"*",  
>"Condition":    {  
>                "NumericLessThanEquals":{  
>                        "ec2:quota-volumenumber":"5"  
>                                        }  
>                }  
>}]  
>}  

This could then be extended to cover a specific resource.  A good example would be to limit the number of created volumes only in a particular cluster/availability zone:

```{
"Statement":[{  
"Sid":"1",  
"Effect":"Limit",  
"Action":"ec2:CreateVolume",  
"Resource":"arn:aws:ec2:::availabilityzone/cluster2",  
"Condition":    {  
                "NumericLessThanEquals":{  
                        "ec2:quota-volumenumber":"5"  
                                        }  
                }  
}]  
}```  

