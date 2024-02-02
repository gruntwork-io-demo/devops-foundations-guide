# DevOps Foundations Process

# Prerequisites

- Permission to create a new AWS Organization
- A company credit card to create the new AWS Organization
- Gruntwork licenses for two machine users
- Naming convention for two machine users
    - One who will have “Write” access
        - referred to in the docs as `ci-user`
    - Another who will have “Read” access
        - referred to in the docs as `ci-read-only-user`
- GitHub.com administrator access to do things like
    - Allow and create fine-grained tokens
    - Administer users and groups
    - Manage GitHub.com Organization settings
        - Create repositories
        - Create teams
        - Create GitHub Actions Secrets
- Decide on a home region to host the Control Tower deployment.
- Decide on additional AWS regions to be managed/denied
- Plan for root email addresses to be created for management, logs, security, and shared accounts at a minimum
    - MFA for all root accounts
    - Password storage solutions, like 1Password

# Skills required

- **[Principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)**
- Git and GitHub
- CI/CD concepts
- Terraform
- AWS
- Secrets management
- ACLs

# Outcomes

- What we get at the end
    - A new AWS Organization
    - AWS SSO configured for all accounts
    - AWS Control Tower controls are configured for all accounts
    - A best practice IaC setup for all accounts
    - IaC Pipelines for all accounts
    - OUs setup
    - Multi-account AWS environment managed by
        - Organizations
        - Control Tower
        - SSO
        - IaC
        - Gruntwork Pipelines

# Create AWS Account

- Create a new AWS account

![Screenshot 2023-11-27 at 8.11.40 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.11.40_AM.png)

- [https://portal.aws.amazon.com/billing/signup#/start/email](https://portal.aws.amazon.com/billing/signup#/start/email)
    - Name this account `management`

![Screenshot 2023-11-27 at 8.12.49 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.12.49_AM.png)

- Save the password and sign in as the root user

![Screenshot 2023-11-27 at 8.13.30 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.13.30_AM.png)

![Screenshot 2023-11-27 at 8.12.36 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.12.36_AM.png)

# Create break glass IAM administrator user and group

- Sign in and create an administrator IAM user and IAM administrators group **5 mins**

![Screenshot 2023-11-27 at 8.16.37 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.16.37_AM.png)

- [https://us-east-1.console.aws.amazon.com/iam/home#/users/create](https://us-east-1.console.aws.amazon.com/iam/home#/users/create)

![Screenshot 2023-11-27 at 8.17.25 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.17.25_AM.png)

- Create an administrator IAM Group to add the Administrator user to and give it administrator permissions; you can do this from the create user workflow
    - [https://us-east-1.console.aws.amazon.com/iam/home#/groups/create](https://us-east-1.console.aws.amazon.com/iam/home#/groups/create)

![Screenshot 2023-11-27 at 8.17.47 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.17.47_AM.png)

- Add the Administrator policy

![Screenshot 2023-11-27 at 8.18.19 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.18.19_AM.png)

- Associate new IAM user to IAM group with the AdministratorAccess policy

![Screenshot 2023-11-27 at 8.18.35 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.18.35_AM.png)

![Screenshot 2023-11-27 at 8.19.39 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.19.39_AM.png)

![Screenshot 2023-11-27 at 8.19.47 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.19.47_AM.png)

- *You **don’t** need to sign in as this user for the time being; it is a break glass user, so store these credentials somewhere safe and set up MFA*

# Create a KMS key to encrypt CloudTrail and Config

- Navigate to your HOME REGION in the console
- Create the KMS key described in the Gruntwork docs
    - The reason for this key is described here: [https://docs.aws.amazon.com/en_us/controltower/latest/userguide//kms-guidance.html](https://docs.aws.amazon.com/en_us/controltower/latest/userguide//kms-guidance.html)

![Screenshot 2023-11-27 at 8.23.40 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.23.40_AM.png)

![Screenshot 2023-11-27 at 8.24.20 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.24.20_AM.png)

![Screenshot 2023-11-27 at 8.36.07 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.36.07_AM.png)

- [https://docs.gruntwork.io/foundations/landing-zone/prerequisites#kms-key-creation](https://docs.gruntwork.io/foundations/landing-zone/prerequisites#kms-key-creation)
- [https://us-east-2.console.aws.amazon.com/kms/home](https://us-east-2.console.aws.amazon.com/kms/home)

![Screenshot 2023-11-27 at 8.37.46 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.37.46_AM.png)

![Screenshot 2023-11-27 at 8.37.46 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.37.46_AM%201.png)

![Screenshot 2023-11-27 at 8.38.33 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.38.33_AM.png)

- Give the administrator IAM user ownership of the KMS key and create the key

![Screenshot 2023-11-27 at 8.46.57 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.46.57_AM.png)

- After creating the KMS Key, edit the key policy based on this AWS guidance:
    - [https://docs.aws.amazon.com/en_us/controltower/latest/userguide//configure-kms-keys.html#kms-key-policy-update](https://docs.aws.amazon.com/en_us/controltower/latest/userguide//configure-kms-keys.html#kms-key-policy-update)
- Click on the key and select policy view

![Screenshot 2023-11-27 at 8.47.50 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.47.50_AM.png)

- Copy and paste what is already there into an IDE to help; doing this in an IDE allows you to replace text quickly and will also call out errors.

![Screenshot 2023-11-27 at 8.41.47 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.41.47_AM.png)

- Copy paste the **two** new statements (from here: [https://docs.aws.amazon.com/en_us/controltower/latest/userguide//configure-kms-keys.html#kms-key-policy-update](https://docs.aws.amazon.com/en_us/controltower/latest/userguide//configure-kms-keys.html#kms-key-policy-update)) into the statement array of the original policy we added.
- Our editor in red is calling out we’re missing a comma in our JSON array, make sure to add these and correctly format this JSON.

![Screenshot 2023-11-27 at 8.44.23 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.44.23_AM.png)

- Do a search and replace for:
    - YOUR-HOME-REGION
    - YOUR-MANAGEMENT-ACCOUNT-ID
    - YOUR-KMS-KEY-ID

![Screenshot 2023-11-27 at 8.45.38 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.45.38_AM.png)

- At the end it should look like this, with the modifications highlighted in the IDE

![Screenshot 2023-11-27 at 8.52.41 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.52.41_AM.png)

- Copy paste this into the KMS key policy and save it.

# Enable Control Tower + AWS SSO

- **Enable Control Tower, following the naming conventions in the docs**
    - [https://us-east-2.console.aws.amazon.com/controltower](https://us-east-2.console.aws.amazon.com/controltower)
    - This will take 30-40 mins, and you’ll use the KMS key here

![Screenshot 2023-11-27 at 8.54.08 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.54.08_AM.png)

- Select the home region your organization decided on

![Screenshot 2023-11-27 at 8.54.36 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.54.36_AM.png)

- Enable any additional regions you’d like governed. Many services are only in `us-east-1`, so always govern that.

![Screenshot 2023-11-27 at 8.56.18 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.56.18_AM.png)

- Enable `deny` access for the ungoverned regions
- Create two OUs with the following names
    - `security` and `pre-prod`

![Screenshot 2023-11-27 at 8.58.31 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.58.31_AM.png)

- Create the two administrative accounts `logs` and `security`
    - Note you’ll need root email address for every account you create

![Screenshot 2023-11-27 at 8.59.17 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.59.17_AM.png)

- Hit next when you’ve added the two account and email names for the root users to these AWS accounts

![Screenshot 2023-11-27 at 8.59.31 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.59.31_AM.png)

- Enable KMS encryption on the account, but the rest of the defaults should make sense for now

![Screenshot 2023-11-27 at 9.01.25 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.01.25_AM.png)

![Screenshot 2023-11-27 at 9.04.40 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.04.40_AM.png)

- Hit next, review all the information on this screen and hit Set up landing zone when ready

![Screenshot 2023-11-27 at 9.06.03 AM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.06.03_AM.png)

- Wait for Control Tower to finish setting itself up, take a break, stretch.
- Sign out of the root user

# Signing in via SSO for the first time 🎉

- You should receive an email for a new AWS SSO user
- Sign in and set a password for the new AWS SSO user
- Sign in via SSO

![Screenshot 2023-11-27 at 12.14.18 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.14.18_PM.png)

![Screenshot 2023-11-27 at 12.14.28 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.14.28_PM.png)

- Sign into the management account using the AWSAdministratorAccess permission set that AWS SSO

![Screenshot 2023-11-27 at 12.15.24 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.15.24_PM.png)

# Customize AWS SSO/IAM Identity Center settings

- Go to SSO via console and write down/customize the SSO domain name

![Screenshot 2023-11-27 at 12.20.10 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.20.10_PM.png)

- Open up the SSO sign-in console in a different tab
    - *if nothing is showing up, ensure you are signed into the HOME REGION*
    - If you customize the domain name, there will be a delay in resolving the new URL.

![Screenshot 2023-11-27 at 12.24.44 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.24.44_PM.png)

![Screenshot 2023-11-27 at 12.26.29 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.26.29_PM.png)

# Create `prod` OU

- Navigate to Control Tower to create the `prod` OU
    - It will take a moment to enroll itself
- *NOTE you should adjust your **ACCOUNT FACTORY** workflow file if you deviate from the capitalization of* [https://github.com/gruntwork-io/terraform-aws-architecture-catalog/blob/main/templates/devops-foundations-infrastructure-live/.github/workflows/account-factory.yml#L28-L35](https://github.com/gruntwork-io/terraform-aws-architecture-catalog/blob/main/templates/devops-foundations-infrastructure-live/.github/workflows/account-factory.yml#L28-L35)

![Screenshot 2023-11-27 at 12.28.33 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.28.33_PM.png)

![Screenshot 2023-11-27 at 12.52.04 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.52.04_PM.png)

![Screenshot 2023-11-27 at 12.53.05 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.53.05_PM.png)

- Although it says it will take awhile, it is usually done in a minute or two

![Screenshot 2023-11-27 at 12.53.59 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.53.59_PM.png)

![Screenshot 2023-11-27 at 12.54.10 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.54.10_PM.png)

# Create `shared` account

- Create the “Shared” account via Control Tower
    - It will take another 20 minutes for it to enroll itself

![Screenshot 2023-11-27 at 12.58.51 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.58.51_PM.png)

![Screenshot 2023-11-27 at 12.59.59 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_12.59.59_PM.png)

![Screenshot 2023-11-27 at 1.01.08 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.01.08_PM.png)

![Screenshot 2023-11-27 at 1.02.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.02.42_PM.png)

![Screenshot 2023-11-27 at 1.29.02 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.29.02_PM.png)

![Screenshot 2023-11-27 at 1.29.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.29.42_PM.png)

![Screenshot 2023-11-27 at 1.31.09 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.31.09_PM.png)

![Screenshot 2023-11-27 at 1.31.58 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.31.58_PM.png)

# Create two GitHub machine users (write and read-only)

- *WARNING: Despite the seemingly limited number of steps, this is fairly tedious work, so relax and take it slow.*
- Create two new GitHub machine users
- Create two new GitHub groups described in the Gruntwork docs:
    - [https://docs.gruntwork.io/pipelines/security/repository-access/](https://docs.gruntwork.io/pipelines/security/repository-access/)
    - Add the `ci` and `ci-read-only` users to the correct GitHub groups per the doc linked above
    - We will create fine-grained personal access tokens shortly, but that only covers the GitHub ID access; we still need to configure access from the *repo* side of the equation
    - Ensure you’ve added the correct repo rights to the GitHub groups via the Team view

# Create new GitHub Organization

- Create one new GitHub org, and invite your two machine users

![Screenshot 2023-11-27 at 1.36.34 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.36.34_PM.png)

- Sign into both accounts and accept the invitations to the organization

![Screenshot 2023-11-27 at 1.37.50 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.37.50_PM.png)

![Screenshot 2023-11-27 at 1.37.25 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.37.25_PM.png)

- Enable classic and fine-grained access tokens in the new GitHub org
    - Do not require administrator approval, and allow both types of tokens. (You can modify this later, but for the sake of bootstrapping this environment, do not require explicit approval).

![Screenshot 2023-11-27 at 2.25.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_2.25.42_PM.png)

![Screenshot 2023-11-27 at 2.28.11 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_2.28.11_PM.png)

- Invite, sign in, and link the GitHub IDs to Gruntwork to add them to the Gruntwork org
    - [https://docs.gruntwork.io/developer-portal/invite-team/](https://docs.gruntwork.io/developer-portal/invite-team/)

# Generate IaC repositories

- ***The steps below cannot be completed until Control Tower has been enabled and the steps above have been taken***
- Review pipelines security architecture
    - [https://docs.gruntwork.io/pipelines/architecture/](https://docs.gruntwork.io/pipelines/architecture/)
- Use the template to generate an infra-live repo
    - this needs to exist *before* we set up access keys for machine users
    - https://github.com/gruntwork-io/infrastructure-live-template

![Screenshot 2023-11-27 at 1.51.13 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.51.13_PM.png)

![Screenshot 2023-11-27 at 1.51.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.51.42_PM.png)

- Launch the template for infra-pipelines
    - This needs to exist *before* we set up access keys for machine users
    - https://github.com/gruntwork-io/infrastructure-pipelines-template

![Screenshot 2023-11-27 at 1.50.40 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.50.40_PM.png)

- Launch the template for infra-modules
    - https://github.com/gruntwork-io/infrastructure-modules-template

![Screenshot 2023-11-27 at 1.50.11 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.50.11_PM.png)

- **WORK WITH YOUR SOLUTIONS ARCHITECT TO ENSURE YOUR VERSIONS ARE CORRECT**

# Create GitHub Teams

- create the GitHub Teams outlined in [https://docs.gruntwork.io/pipelines/security/repository-access/](https://docs.gruntwork.io/pipelines/security/repository-access/)
    - repeat for the three teams and the two machine users
    - add any additional coworkers to the appropriate teams

![Screenshot 2023-11-27 at 1.55.23 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.55.23_PM.png)

![Screenshot 2023-11-27 at 1.56.47 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_1.56.47_PM.png)

# Generate machine user access keys

- follow this guide [https://docs.gruntwork.io/pipelines/security/machine-users](https://docs.gruntwork.io/pipelines/security/machine-users)
- do the `ci` and `ci-read-only` machine user access keys by signing into [GitHub.com](http://GitHub.com) for each of them
    - **Tip**: you can now switch between [GitHub.com](http://GitHub.com) accounts easily and can be signed into multiple
    - this is pretty tedious but a critical security step to do correctly
    - ensure the Fine Grained Access Tokens point to the `infra-live` and `infra-pipelines` repos and not to GitHub IDs of the machine user you’re currently signed in as, either the `ci` or `ci-read-only` ID you created.
        - for example, my name on [GitHub.com](http://GitHub.com) is `iangrunt`
            - I have a GitHub Org I named `iangrunt-org`
            - When it came to creating the `ci` and `ci-read-only` [GitHub.com](http://GitHub.com) users, I named them
                - `iagruntW` and `iangruntRO`
                - or “iangrunt-write” and “iangrunt-read-only”

## CI machine user secret creation

### INFRA_LIVE_ACCESS

- create the INFRA_LIVE_ACCESS secret for the `ci` user, and ensure it is scoped to your GitHub Organization and associated with the correct repositories:
    - sign into [GitHub.com](http://GitHub.com) as the `ci` user you created, and go to settings then access tokens

![Screenshot 2023-11-27 at 5.52.26 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_5.52.26_PM.png)

- ensure that the token is scoped to the GitHub Organization you created above
    - if it is not visible, ensure you’ve enabled fine-grained access tokens on the GitHub Organization

![Screenshot 2023-11-27 at 5.54.12 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_5.54.12_PM.png)

- set the correct permissions, and be ready to copy the token to a secure location (not Slack) while you create the rest of the tokens and store the values.
    - using a tool like 1Password is a good solution and storing it as secure text in a secure vault with the infrastructure team

### PIPELINES_DISPATCH

- do the same for PIPELINES_DISPATCH, which this time will point to the infrastructure-pipelines

![Screenshot 2023-11-27 at 5.59.47 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_5.59.47_PM.png)

![Screenshot 2023-11-27 at 5.59.43 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_5.59.43_PM.png)

![Screenshot 2023-11-27 at 6.06.22 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_6.06.22_PM.png)

### PIPELINES_BOOTSTRAP

- Still as the `ci` user, set up the PIPELINES_BOOTSTRAP token. This is for the pipelines repo but only needs 30 days until it expires as it will not be needed after this process is completed.

![Screenshot 2023-11-27 at 6.11.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_6.11.42_PM.png)

![Screenshot 2023-11-27 at 6.13.07 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_6.13.07_PM.png)

## **ci-read-only-user GitHub token creation**

### GRUNTWORK_CODE_ACCESS

- Create the `GRUNTWORK_CODE_ACCESS`. This token will be used to manage access to Gruntwork resources during GitHub Action runs.
- Sign into GitHub as the `ci-read-only-user`
    - In the security model this user is used for reading Gruntwork and other private repositories without having broad permissions
- This is a classic token which will have the `repo` scope

![Screenshot 2023-11-27 at 6.22.15 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_6.22.15_PM.png)

# Optional: Test access tokens locally

- Test the access keys to ensure they at least work
    - [https://docs.github.com/en/rest/overview/authenticating-to-the-rest-api?apiVersion=2022-11-28#basic-authentication](https://docs.github.com/en/rest/overview/authenticating-to-the-rest-api?apiVersion=2022-11-28#basic-authentication)
    
    ```
    curl --request GET \
    --url "https://api.github.com/octocat" \
    --header "Authorization: Bearer YOUR-TOKEN" \
    --header "X-GitHub-Api-Version: 2022-11-28"
    ```
    
- T**his is about 1hr 20 minutes of work if you’ve done it before**
- From the AWS SSO sign-in portal opened on another tab, gather the account names and root email addresses
- From the AWS SSO sign-in portal opened in another tab, familiarize yourself with the **`Command line or programmatic access` link inside each account**
    - This will make it easy to switch around accounts from the terminal
        - Later on, we need to set up AWS Vault or something similar for safer CLI access

# Paid GitHub Org - Create GitHub Organization-level secrets

- Switch GitHub profiles to use your standard user, not a machine user, and navigate to the settings page for your new GitHub Organization
- Following this guide, [https://docs.gruntwork.io/pipelines/security/machine-users#configure-secrets-for-github-actions](https://docs.gruntwork.io/pipelines/security/machine-users#configure-secrets-for-github-actions)

![Screenshot 2023-11-27 at 6.54.39 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_6.54.39_PM.png)

- Add the three values associated them to the specific repositories outlined in the guide [https://docs.gruntwork.io/pipelines/security/machine-users#configure-secrets-for-github-actions](https://docs.gruntwork.io/pipelines/security/machine-users#configure-secrets-for-github-actions)

![Screenshot 2023-11-27 at 7.08.20 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_7.08.20_PM.png)

# Free GitHub Org - Add secrets to each repository

- If you are testing this flow on a free GitHub Org, organization level secrets may not be available to you

### infrastructure-live

In the `infrastructure-live` repository create the following secrets:

1. `INFRA_LIVE_ACCESS_TOKEN`. This should be assigned the *`INFRA_LIVE_ACCESS`* token generated by the `ci-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-user) as its value.
2. `PIPELINES_DISPATCH_TOKEN`. This should be assigned the *`PIPELINES_DISPATCH`* token generated by the `ci-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-user) as its value.
3. `GRUNTWORK_CODE_ACCESS_TOKEN`. This should be assigned the *`GRUNTWORK_CODE_ACCESS`* token generated by the `ci-read-only-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-read-only-user) as its value.

![Screenshot 2023-11-27 at 7.30.29 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_7.30.29_PM.png)

[](https://docs.gruntwork.io/pipelines/security/machine-users#infrastructure-pipelines)

![Screenshot 2023-11-27 at 7.33.44 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_7.33.44_PM.png)

### infrastructure-pipelines

In the `infrastructure-pipelines` repository create the following secrets:

1. `GRUNTWORK_CODE_ACCESS_TOKEN`. This should be assigned the *`GRUNTWORK_CODE_ACCESS`* token generated by the `ci-read-only-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-read-only-user) as its value.
2. `INFRA_LIVE_ACCESS_TOKEN`. This should be assigned the *`INFRA_LIVE_ACCESS`* token generated by the `ci-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-user) as its value.
3. `PIPELINES_BOOTSTRAP_TOKEN`. This should be assigned the *`PIPELINES_BOOTSTRAP`* token generated by the `ci-user` in the [secrets section](https://docs.gruntwork.io/pipelines/security/machine-users#ci-user) as its value.

![Screenshot 2023-11-27 at 7.37.57 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_7.37.57_PM.png)

# Configure the IaC repositories for bootstrapping

## Review initial templated repositories with Solutions Architect

- Before following the README in `infrastructure-live` and `infrastructure-pipelines`
- **Work with your Solutions Architect to confirm the workflow files in all the repositories are up to date**
- GitHub Actions used in Gruntwork Pipelines
    - these are referenced once you’ve ge
    - https://github.com/gruntwork-io/pipelines-execute
    - https://github.com/gruntwork-io/pipelines-dispatch
    - https://github.com/gruntwork-io/pipelines-orchestrate
    - *Optionally*
        - https://github.com/gruntwork-io/patcher-action
        - https://github.com/gruntwork-io/terragrunt-action
- Architecture Catalog
    - https://github.com/gruntwork-io/terraform-aws-architecture-catalog
- Control Tower
    - https://github.com/gruntwork-io/terraform-aws-control-tower
- Terraform CIS Service Catalog
    - https://github.com/gruntwork-io/terraform-aws-cis-service-catalog
- Pipelines CLI
    - https://github.com/gruntwork-io/pipelines
    - https://github.com/gruntwork-io/pipelines-cli
- Boilerplate
    - https://github.com/gruntwork-io/boilerplate
- Gruntwork Installer
    - https://github.com/gruntwork-io/gruntwork-installer
- Terragrunt
    - https://github.com/gruntwork-io/terragrunt

### Confirm OU names and account names are correct

- **Work with your Solutions Architect to confirm the workflow files have the correct**
    - OU Names
    - Account names and IDs

## Bootstrap infrastructure-live

- Do not run the GitHub Action to bootstrap your IaC until you go to your `infrastructure-live` repo and review the instructions in the README.md
- All of the prerequisites should be met

![Screenshot 2023-11-27 at 7.44.34 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_7.44.34_PM.png)

- Fix the `bootstrap.yml` and enter your region
- Find `team` tag and replace it if needed in

![Screenshot 2023-11-27 at 8.08.08 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.08.08_PM.png)

- Open up a PR with the changes

![Screenshot 2023-11-27 at 8.11.09 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.11.09_PM.png)

![Screenshot 2023-11-27 at 8.11.46 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.11.46_PM.png)

- Get the product ID from the AWS service catalog described in the `infrastructure-live` README

![Screenshot 2023-11-27 at 8.27.25 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.27.25_PM.png)

![Screenshot 2023-11-27 at 8.27.43 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_8.27.43_PM.png)

- Start the GitHub Actions process to generate the initial code that represents the accounts already created by Control Tower, and baselines/pipelines them
- This will open up a pull request

![Screenshot 2023-11-27 at 9.03.09 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.03.09_PM.png)

- Open up the SSO console in another tab, and fill out the form that will use these values to generate your intial IaC scaffolding

![Screenshot 2023-11-27 at 9.04.36 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.04.36_PM.png)

- Fill out the form and it will open up a pull request

![Screenshot 2023-11-27 at 9.12.25 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.12.25_PM.png)

![Screenshot 2023-11-27 at 9.12.49 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.12.49_PM.png)

![Screenshot 2023-11-27 at 9.12.56 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.12.56_PM.png)

![Screenshot 2023-11-27 at 9.14.30 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.14.30_PM.png)

- The PR shouldn’t be merged until you’ve completed all the pre-requistes

![Screenshot 2023-11-27 at 9.15.41 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.15.41_PM.png)

- T**his is about 2hr 20 minutes of work if you’ve done it before**

# Local bootstrapping process from new PR

- `git clone` the `infrastructure-live` repo, and checkout the branch

![Screenshot 2023-11-27 at 9.36.17 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.36.17_PM.png)

- `cd` to `infrastructure-live`
- `git fetch -v -a && git checkout bootstrap-repository`
- Go to the SSO tab and click on the management account and then **`Command line or programmatic access` for the admin role**
- Copy-paste the section that export’s the keys and enter that into the same terminal window you have infra-live checked out in

![Screenshot 2023-11-27 at 9.37.54 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.37.54_PM.png)

- Test your AWS access with with
    - `aws sts get-caller-identity`

![Screenshot 2023-11-27 at 9.38.56 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.38.56_PM.png)

- Uncomment the Control Tower governed regions listed in `multi_region_common.hcl`
    - Open up the Control Tower dashboard to confirm which regions you are opted into
    - Use your favorite IDE to uncomment the appropriate regions

![Screenshot 2023-11-27 at 9.40.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.40.42_PM.png)

![Screenshot 2023-11-27 at 9.43.40 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.43.40_PM.png)

- Check in your changes and use `[skip ci]` in your commit message

![Screenshot 2023-11-27 at 9.50.22 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_9.50.22_PM.png)

![Screenshot 2023-11-27 at 10.01.39 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.01.39_PM.png)

# Review initial pull request for four new accounts

- REMINDER: **CONSULT WITH YOUR SOLUTIONS ARCHITECT**
    - At this stage, it is worth having your Solutions Architect review the PR with you unless you feel comfortable to proceed.

## Dependencies to confirm are on the correct versions by file

- `.github/workflows/account-factory.yml`
    - https://github.com/gruntwork-io/gruntwork-installer
    - https://github.com/gruntwork-io/boilerplate
    - https://github.com/gruntwork-io/terraform-aws-control-tower
    - https://github.com/gruntwork-io/terraform-aws-cis-service-catalog
    - https://github.com/actions/checkout
- `.github/workflows/patcher.yml`
    - https://github.com/actions/checkout
    - https://github.com/andstor/file-existence-action
- `.github/workflows/pipelines.yml`
    - https://github.com/actions/checkout
    - https://github.com/gruntwork-io/pipelines-orchestrate
    - https://github.com/gruntwork-io/pipelines-dispatch
- `.gruntwork/config.yml`
    - https://github.com/gruntwork-io/pipelines
    - https://github.com/gruntwork-io/pipelines-cli
    - Terraform/OpenTofu version
    - https://github.com/gruntwork-io/terragrunt
- `_envcommon/landingzone/account-baseline-app-cis-base.hcl`
    - https://github.com/gruntwork-io/terraform-aws-control-tower

## Additional files to review

- `_envcommon/landingzone/account-baseline-app-cis-base.hcl`
    - Confirm value for `security_hub_enable_cis_check` is set to the desired value; `false` if you will use another tool other than SecurityHub to check for CIS.
    - `slack_webhook_url_secrets_manager_arn` confirm this is something you need
- `accounts.yml`
    - Confirm account IDs and emails are correct
- `common.hcl`
    - Review inputs, confirm nothing looks off
- `terragrunt.hcl`
    - The “root” `terragrunt.hcl` that influences all others; worth a review for oddities.

## Phrases to search for across files

- `TG_ENABLE_PARALLELISM_LIMIT`
    - See if this is called anywhere and set it to something reasonable
- `0.0.0.0/0`
    - Remove “quad zero” mentions to ensure security group rules aren’t flagged by CIS
- Files to avoid customizing:
    - `_envcommon/common/multi_region_providers.hcl`
        - This generates **several** provider blocks for critical modules.
    - `.github/scripts/source-ref.sh`
        - Used with Gruntwork Pipelines, not worth modifying.
    - `.github/workflows/{account-factory.yml,patcher.yml,pipelines.yml`
        - Unless instructed by Gruntwork, breaking changes here should be handled with automation and not customized.

## Things to have your Solutions Architect update

- `.github/workflows/README.md`

# Run Terragrunt locally for four initial accounts

- This step happens locally because Gruntwork Pipelines needs to be bootstrapped, and it allows for faster iterating during initialization.
- Once this step is over, all changing will take place via Gruntwork Pipelines, including changes to these four new administrative accounts.

### management account bootstrapping

- `cd management/_global/github-oidc-role` after exporting your AWS credentials from the SSO console (and confirming they work with as expected testing with `aws sts get-caller-identity`)

![Screenshot 2023-11-27 at 10.06.52 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.06.52_PM.png)

- Confirm the `terragrunt run-all plan` looks good and proceed to `terragrunt run-all apply` in the `github-oidc-role` module
    - the purpose of this module is so Gruntwork Pipelines can run, it is critical.
    - The `plan` looks good if it looks approximately like this

![Screenshot 2023-11-27 at 10.08.09 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.08.09_PM.png)

- Proceed to running `terragrunt run-all apply` for the `github-oidc-role` module and accept the changes

![Screenshot 2023-11-27 at 10.08.31 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.08.31_PM.png)

![Screenshot 2023-11-27 at 10.08.42 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.08.42_PM.png)

- `cd` to `_global` and run `terragrunt run-all plan`

![Screenshot 2023-11-27 at 10.12.02 PM.png](DevOps%20Foundations%20Process%20b30232390b42442a938cffd07178b33a/Screenshot_2023-11-27_at_10.12.02_PM.png)

- Confirm the `terragrunt run-all plan` in `_global` looks good
- You can optionally `cd` into each remaining module, and run `terragrunt run-all apply` in this order to get a feel for things:
    - infrastructure-live/management/_global/cross-account-iam-roles
    - infrastructure-live/management/_global/pipelines-policy-update-role-write
    - infrastructure-live/management/_global/pipelines-read-only-role
    - infrastructure-live/management/_global/pipelines-policy-update-role-ro
    - infrastructure-live/management/_global/control-tower-multi-account-factory
    - infrastructure-live/management/_global/sso-permission-sets/billing-access
    - infrastructure-live/management/_global/sso-permission-sets/full-access
        - This takes about 15 minutes and will give you a better understanding of what’s being deployed.
    - Run a final `run-all apply` from `_global` to catch anything that may have been missed
- **This is about 3 hrs 10 minutes of work if you’ve done it before**
- Time to move onto the `logs` account
    - Get the token from the **`Command line or programmatic access` for the `logs` account as the admin and export it into your CLI**
    - `aws sts get-caller-identity` to test
        - Manual `terragrunt` plan and applies
            - infrastructure-live/logs/_global/github-oidc-role
            - infrastructure-live/logs/_global/pipelines-read-only-role
            - infrastructure-live/logs/_global/pipelines-policy-update-role-ro
            - infrastructure-live/logs/_global/pipelines-policy-update-role-write
            - infrastructure-live/logs/_global/account-baseline
                - Inspect the `terragrunt plan`
                - You might hit a `macie` propagation error, just re-run the `apply`
                    - │ **Error: error creating Macie ClassificationJob: ValidationException: Macie can't create the job right now. The Macie service-linked role for your account hasn't finished propagating. Try again in a few minutes.**
    - Run `apply run-all` from  `logs/_global`
- Get the token from the **`Command line or programmatic access` for the `security` account as the admin and export it into your CLI**
- `cd` `../../security`
- `aws sts get-caller-identity` to test
- Confirm account number
- To speed things up, I ran `terragrunt apply` in
    - infrastructure-live/security/_global/github-oidc-role
- And then ran `terragrunt run-all apply` from `_/global` , this prevents a lot of Terragrunt noise
- Time for the **final** bit of local handwork; don’t worry, it’ll be all pipelines after this
- Get the token from the **`Command line or programmatic access` for the `shared` account as the admin and export it into your CLI**
    - `aws sts get-caller-identity` to test
    - Confirm account number
    - ⚠️ run `aws iam create-service-linked-role --aws-service-name [autoscaling.amazonaws.com](http://autoscaling.amazonaws.com/)`
    - `cd` `shared/_global`
    - `infrastructure-live/shared/_global/github-oidc-role 0 $ terragrunt apply`
    - `cd ..`
    - `terragrunt run-all apply`
- T**his is about 4 hours into the process**
- Navigate to pipelines repo
- Run GitHub Actions bootstrap
- Review, merge pipeles PR and remove temporary tokens
- Go back to `infrastructure-live`, merge that bootstrap PR
- J**obs will run, this is about 4hr 40 minutes into the process**

# New account factory manual setup, adding an IAM princpal:

### Permissions for Account Factory Portfolio

- This was/is a constant source of tripping up when setting up the account factory. The account factory pipeline *will fail* until you grant access to the portfolio to the pipelines roles (`github` and `github-read-only`). This step **must be done after** the user manually provisions the pipelines roles in the management account (where control tower is set up).
- Access to the portfolio is separate from IAM access, it *must be* granted in the Service Catalog console.

### **Steps to grant access**

- To grant access to the Account Factory Portfolio, you must be an individual with Service Catalog administrative permissions. This is written with the assumption that the `github` and `github-read-only` roles exist in the management account at this point, which all users will need to manually run an `apply` to do from their local machine.
    1. Log into the management AWS account
    2. Go into the **Service Catalog** console
    3. Select the **Portfolios** option in **Administration** from the left side navigation panel
    4. Click on the portfolio named **AWS Control Tower Account Factory Portfolio**
    5. Select the **Access** tab
    6. Click the **Grant access** button
    7. In the **Access type** section, leave the default value of **IAM Principal**
    8. Select the **Roles** tab in the lower section
    9. Enter `github` into the search bar, there should be two results (`github` and `github-read-only`). Click the checkbox to the left of each role name
    10. Click the **Grant access** button in the lower right hand corner
- Go to Actions, and run the new account workflow

## Bootstrap Pipelines Repo

- Follow the bootstrap process
- `.github/workflows/terragrunt-executor.yml` needs to be up to date with:
    - [https://github.com/gruntwork-io/pipelines-execute](https://github.com/gruntwork-io/pipelines-execute)
    - **apply-new-account-baseline.yml**
        - This is in a bunch of places to bump the GHA to 2.1.0
    - **create-account-and-generate-baselines.yml**
        - Review as well for any udpates

# Setup GitHub branch protection

- Follow [https://docs.gruntwork.io/pipelines/security/branch-protection](https://docs.gruntwork.io/pipelines/security/branch-protection)
- Set up squash and  merge as the default

# Cleanup

- Enable MFA on root email accounts for the new accounts
- Remove `bootstrap.yml` from the two repositories
- Remove the bootstrap token
- Remove default VPC from management account