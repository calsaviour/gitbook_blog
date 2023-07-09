## Steps to setup CDK
```
mkdir infra
cd infra
cdk init sample-app --language typescript
npm install @aws-cdk/aws-s3 @aws-cdk/aws-iam aws-sdk\n
```

## Steps to setup sst
```
npx create-sst@latest <APPLICATION_NAME>
npx sst dev
npx sst deploy --stage prod
```