# jh-serverless-btmn

This Repository outlines a possible Serverless Foundation to drive a JH BTMN like Service.

# Use-Case #1: Authentication

  - Cognito user pool with externally provided data

## Provisioning

    AWS_PROFILE=abc make deploy

## Importing Users via CSV

https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-using-import-tool-csv-header.html

## Authenticating a User in a Browser

    https://docs.aws.amazon.com/cognito/latest/developerguide/using-amazon-cognito-user-identity-pools-javascript-examples.html#using-amazon-cognito-identity-user-pools-javascript-example-authenticating-user

## Validating OpenID Tokens "inside AWS / API-Gateway / Serverless / Lambda"

The resulting OpenID Token can be used to further authenticate against Backend Services shielded by an API-Gateway:

    https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-enable-cognito-user-pool.html

## Validating OpenID Tokens outside of AWS:

The User Pool's Public Key is located under: `https://cognito-idp.{region}.amazonaws.com/{userPoolId}/.well-known/jwks.json`, so even external entities (i.e. a heritage PHP application) can verify the validity of tokens issued by the cognito user pool:

    zined@baz:~/src/jh-serverless-btmn$ curl -s https://cognito-idp.eu-west-1.amazonaws.com/eu-west-1_PKYloFV7G/.well-known/jwks.json | jq .
    {
      "keys": [
        {
          "alg": "RS256",
          "e": "AQAB",
          "kid": "ofak6nuBYQlaBCoVUj+rlb9SJsOVhO/MpjgWMxKhxHc=",
          "kty": "RSA",
          "n": "v0OHZkOLFmg3L-oWdEj_3sSwF2pRAZ25uCQLMEgocqoc4fsaB-irx2ZVcaPR4ErHeVmXTSwmZWkO96-qUSnGjBbuFRl6IWPJ9Pxn1t6cwjHoSPw_Rq2jbkBtzKDy_y397qmfx6gCyS-sDyowwtaFeY2mHkFYFYdBByJRM0EL_dejXNTOGP-r77YU1ye46YYQX2qXZUAh260pCZ_iEbtz493aWz6z987U96k-zdX0QNE_5ERbBOmV5I35J9iyeShz81K5uJQGsJB4_nReegZ5IHkvTcG80nakB3tflb-XRJlEtke_y_mqmkTZZTEV0lLQuIYyp2x0aF7WMU3eD5dKCQ",
          "use": "sig"
        },
        {
          "alg": "RS256",
          "e": "AQAB",
          "kid": "1pMbVd2C/boou/OMmkjNe+8BzGEZLhZFLMV8bPyNb/8=",
          "kty": "RSA",
          "n": "iXfJzsdKWXs_AzOrin1ZDXqquZImiJV6NACx_WswKKJJ8B06vgzAasyGEHZNYvEKZEDtZkEhT3lP7iXlLUtLb5Kzg3NWJ9u2VgUhTCMFjecGbowMvKeIS5VEMs8AJfnOHrzYtKwomnkvMKe5NEz-L_0_3DAqOAXyhKm37W9SZCe2_F9MkPX-Nz8m4zF64q5ePwb-sCL4Lu6K6q_p93uwE0WBM5IBy8uqx6C-2DInRroTTg0ll-D-hBrisZg8Fi4pXoBWbV6Q3e5yJ7OLBq6wXeNMX99nb3oNjLmWttENqF_QVMLSzl3BwEcxJeAoa6B88HX1su9M-t8u4ID7UJRRMQ",
          "use": "sig"
        }
      ]
    }


# Use-Case #2: Data Storage
  
  - DynamoDB, Schema minimal:
    - Tiefenentladung ja/nein (<19% Ladestand Ende)
    - Equipment-Nummer (suchbar)
    - Interne Nummer (suchbar)
    - Fahrer (anonymisiert optional)
    - Einsatzbeginn
    - Einsatzende
    - Ladestand Start
    - Ladestand Ende

## Use Case #3: Fetch data from ISM, write into DynamoDB

  - mock external data source, interface: `get-entries(startPosition, count)`, return random data

## Use Case #4: Static React Single Page Website Hosting with Deployment Pipeline

  - s3 bucket, cloudfront, cloudformation template

## Use Case #5: Serverless Backend for Single Page React Application that returns data and provides filtering functionality

