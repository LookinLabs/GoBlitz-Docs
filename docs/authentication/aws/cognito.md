---
title: "Cognito"
---

Cognito is a fully managed identity service provided by AWS. It allows you to easily add user sign-up, sign-in, and access control to your applications. GoBlitz uses Cognito as an authentication provider for securing API Paths and Web Routes via JWT Tokens.

## Architecture

<img src="/img/auth_arch.png" alt="Architectural overview of authentication" width="600" height="600"></img>

## Getting Started

Before we start setting up the application against Cognito, make sure you have the following prerequisites. Cognito can be set up in a free tier account - you can check the pricing [here](https://aws.amazon.com/cognito/pricing/).

#### Prerequisites

- [AWS Account](https://aws.amazon.com/)
- [Configured AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

#### Setting Up Cognito

Cognito can be set up via terraform code under aws/terraform directory. This code sets up the Cognito User Pool and Identity Pool.

1. Change the directory

```bash
cd cloud/aws/terraform
```

2. Initialize the terraform

```bash
terraform init
```

3. Plan the terraform

```bash
terraform plan
```

4. Apply the terraform

```bash
terraform apply
```

### Connecting application with Cognito

1. Configure the Cognito User

You can sign up for a new user via Cognito Hosted UI.

Go to the `Cognito` tab in the AWS Console and click on the User Pools. Click on the User Pool you created and go to the `App Integration`.

By default there will be `GoBlitzClient` application client. Click on that client and scroll down till you see "Hosted UI" section. Press `View Hosted UI` and sign up for a new user.

2. Configure the .env

Configure Cognito Environment variables. The `AWS_COGNITO_USER_POOL_ID` and `AWS_COGNITO_APP_CLIENT_ID` can be found in terminal after running `terraform apply` under `Changes on Outputs:`.

```
Changes to Outputs:
  client_id    = "ah37r9t409rjotnm8h575edio"
  user_pool_id = "us-east-1_5laVcI9aL"
```

```bash
AWS_COGNITO_USER_POOL_ID=user_pool_id
AWS_COGNITO_APP_CLIENT_ID=client_id
```

`AWS_COGNITO_TOKEN_URL` is the URL to get the token from Cognito. It's in the format of `https://<user-pool>.auth.<region>.amazoncognito.com/oauth2/token`.

By default it's `https://goblitz.auth.us-east-1.amazoncognito.com/oauth2/token`

The `AWS_COGNITO_API_USER_EMAIL` and `AWS_COGNITO_API_USER_PASSWORD` are the email and password of the user you signed up with.


3. Run the application

```bash
make
```

4. Visit some API Path

Try to visit some path via Curl or browser. In case you're not setting up the JWT Token, you will get a 401 Unauthorized error.

```bash
curl -X GET http://localhost:8000/api/v1/ping

{"error": "401 Unauthorized Error. Token Not Found."}
```

## Fetching JWT Tokens

**Note** To fetch JWT Tokens from Cognito, you need to have configured AWS_COGNITO_* environment variables.

1. Install python packages

```bash
python3 -m pip install -r requirements.txt
```

2. Get the JWT Token

You can get the JWT Token by running the following command:

```bash
python3 bin/fetch_jwt.py
```