```text
┌──────────────────────────────────────────────────────────────────────────┐
│                 You (write SAM template.yaml)                            │
│          (YAML + AWS::Serverless::* + Intrinsic Functions)               │
└──────────────────────────────────────────────────────────────────────────┘
                     |
                     | 1) sam build
                     |    - copies code, resolves build artifacts
                     v
┌──────────────────────────────────────────────────────────────────────────┐
│  .aws-sam/build/template.yaml                                            │
│  (still SAM syntax; code paths/artifacts rewritten)                      │
└──────────────────────────────────────────────────────────────────────────┘
                     |
                     | 2) sam deploy
                     |    - package/upload artifacts to S3
                     |    - transform SAM -> pure CloudFormation
                     v
┌──────────────────────────────────────────────────────────────────────────┐
│ CloudFormation Template (expanded)                                       │
│ - AWS::Serverless::*  ==>  AWS::* resources (Lambda, IAM, ApiGw, etc.)   │
│ - plus: AWS::CloudFormation::Transform = AWS::Serverless-2016-10-31      │
│ - Intrinsic functions kept as Fn::... / Ref                              │
└──────────────────────────────────────────────────────────────────────────┘
                     |
                     | 3) CloudFormation CreateStack/UpdateStack
                     v
┌──────────────────────────────────────────────────────────────────────────┐
│ CloudFormation Engine                                                    │
│  A) Parameter Resolution                                                 │
│     - Parameters get concrete values (defaults / overrides)              │
│                                                                          │
│  B) Template Parsing + Reference Graph Build                             │
│     - Build dependency graph from:                                       │
│         * Ref / Fn::GetAtt / Fn::Sub (implicit deps)                     │
│         * DependsOn (explicit deps)                                      │
│                                                                          │
│  C) Intrinsic Functions Evaluation (when possible)                       │
│     - Some can be resolved early (pure string/list ops)                  │
│     - Some require resource physical IDs -> resolved later               │
│                                                                          │
│  D) Resource Provisioning (topological order)                            │
│     - Create resources according to dependency graph                     │
│     - Each resource gets PhysicalResourceId (e.g., actual ARN/ID)        │
│                                                                          │
│  E) Late Binding / Post-create Intrinsic Resolution                      │
│     - Now Ref/GetAtt that needed physical IDs become concrete            │
│     - Used to configure downstream resources                             │
│                                                                          │
│  F) Outputs Evaluation                                                   │
│     - Evaluate Outputs (Ref/GetAtt/Sub) and return to you                │
└──────────────────────────────────────────────────────────────────────────┘
                     |
                     v
┌──────────────────────────────────────────────────────────────────────────┐
│ Stack Result                                                             │
│ - Resources created (Lambda, IAM Role, API, S3, etc.)                     │
│ - Outputs returned (Api URL, Bucket name, ARNs...)                        │
└──────────────────────────────────────────────────────────────────────────┘


Intrinsic Functions “who needs what” (quick mental model)
---------------------------------------------------------
(1) Pure-value / can resolve early (if inputs are known):
    - Fn::Join, Fn::Split, Fn::Select, Fn::If, Fn::Equals, Fn::And/Or/Not
    - Fn::Sub (only if it references Parameters / mappings / pseudo params)

(2) Needs existing resource physical IDs (often resolved after create):
    - Ref (for resources, returns physical ID; for params returns param value)
    - Fn::GetAtt (needs the resource to exist to return attribute)
    - Fn::Sub (if it references ${MyResource} / ${MyResource.Arn}, etc.)

(3) Transform note (SAM specific):
    - AWS::Serverless Transform happens BEFORE CloudFormation provisions
    - After transform, you’re effectively deploying a bigger CFN template
```
