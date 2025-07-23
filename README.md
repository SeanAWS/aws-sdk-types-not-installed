# aws-sdk-types-not-installed

Looks like clients depends on `@smithy/signature-v4`

```
npm ls @smithy/signature-v4            
vite-project@1.0.0 /Users/sboult/repro/aws-sdk-types-not-installed
└─┬ @aws-sdk/client-s3@3.523.0
  ├─┬ @aws-sdk/core@3.523.0
  │ └── @smithy/signature-v4@2.3.0
  ├─┬ @aws-sdk/middleware-sdk-s3@3.523.0
  │ └── @smithy/signature-v4@2.3.0 deduped
  ├─┬ @aws-sdk/middleware-signing@3.523.0
  │ └── @smithy/signature-v4@2.3.0 deduped
  └─┬ @aws-sdk/signature-v4-multi-region@3.523.0
    └── @smithy/signature-v4@2.3.0 deduped
```

Dep not in the package.json
```
cat node_modules/@aws-sdk/client-s3/package.json | grep @smithy/signature-v4
```

But is is installed in the core

```
cat node_modules/@aws-sdk/core/package.json | grep @smithy/signature-v4
```

