# Creating crossplane IRSA composition 

crossplane can extend kubernetes api to support cloud resources managed as kubernetes objects.  
you can create an interface for multiple resources to manage these resources using XRD (custom resource definition for crossplane).  

- start with XRD schema 
- create a composition to use the input on the base resources you are creating . its simpler. 

## The XRD 

```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
metadata:
  name: xirsas.example.company
spec:
  claimNames:
    kind: IRSA
    plural: irsas
  group: example.company
  names:
    kind: XIRSA
    plural: xirsas
  versions:
    - name: v1alpha1
      served: true
      referenceable: true
      schema:
        openAPIV3Schema:
          type: object
          description: IRSA is the Schema for the irsas API
          properties:
            spec:
              type: object
              description: IRSA Spec defines the desired state of IRSA
              properties:
                appName:
                  type: string
                policyDocument:
                  type: string
              required:
              - appName
              - policyDocument 
            status:
              type: object
              description: IRSA Status defines the observed state of IRSA
              properties:
                roleArn:
                  type: string
                roleID:
                  type: string
                policyArn:
                  type: string
                policyID:
                  type: string

```

## The composition that uses the above XRD 

```yaml
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: irsa-exact.example.company
spec:
  compositeTypeRef:
    apiVersion: example.company/v1alpha1
    kind: XIRSA
  resources:
    - name: iam-role
      base:
        apiVersion: iam.aws.crossplane.io/v1beta1
        kind: Role
        metadata:
          name: to-be-patched
        spec:
          forProvider:
            assumeRolePolicyDocument: ""
      patches:
        - type: FromCompositeFieldPath
          fromFieldPath: spec.appName
          toFieldPath: metadata.name
        - type: ToCompositeFieldPath
          fromFieldPath: status.atProvider.arn
          toFieldPath: status.roleArn
        - type: ToCompositeFieldPath
          fromFieldPath: status.atProvider.roleID
          toFieldPath: status.roleID
        - type: CombineFromComposite
          toFieldPath: spec.forProvider.assumeRolePolicyDocument
          policy:
            fromFieldPath: Required
          combine:
            variables:
            - fromFieldPath: metadata.labels[crossplane.io/claim-namespace]
            - fromFieldPath: spec.appName
            strategy: string
            string:
              fmt: |
                {
                  "Version": "2012-10-17",
                  "Statement": [
                    {
                      "Effect": "Allow",
                      "Principal": {
                        "Federated": "arn:aws:iam::{{.Values.irsa.awsAccountID}}:oidc-provider/{{.Values.irsa.eksOIDC}}"
                      },
                      "Action": "sts:AssumeRoleWithWebIdentity",
                      "Condition": {
                        "StringEquals": {
                          "{{.Values.irsa.eksOIDC}}:sub": "system:serviceaccount:%s:%s",
                          "{{.Values.irsa.eksOIDC}}:aud" : "sts.amazonaws.com"
                        }
                      }
                    }
                  ]
                }
    - name: policy
      base:
        apiVersion: iam.aws.crossplane.io/v1beta1
        kind: Policy
        spec:
          forProvider:
            name: ""
            document: ""
      patches:
        - type: ToCompositeFieldPath
          fromFieldPath: status.atProvider.arn
          toFieldPath: status.policyArn
        - type: ToCompositeFieldPath
          fromFieldPath: status.atProvider.policyID
          toFieldPath: status.policyID
        - type: FromCompositeFieldPath
          fromFieldPath: spec.policyDocument
          toFieldPath: spec.forProvider.document
        - type: FromCompositeFieldPath
          fromFieldPath: spec.appName
          toFieldPath: spec.forProvider.name
    - name: role-policy-attachment
      base:
        apiVersion: iam.aws.crossplane.io/v1beta1
        kind: RolePolicyAttachment
        spec:
          forProvider:
            policyArn: ""
            roleNameSelector:
              matchControllerRef: true
      patches:
        - type: FromCompositeFieldPath
          fromFieldPath: status.policyArn
          toFieldPath: spec.forProvider.policyArn
```

This doesn't create a service account (for simplicity i have not installed k8 provider on EKS because there is argo and helm as well. why complicate things). 

