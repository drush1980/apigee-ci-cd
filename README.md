# apigee-ci-cd

- In the [pom file](pom.xml), update the `apigee.org` property under `test` and `prod` POM profile to point to your Apigee org, or pass in a maven property named `apigeeorg`
- The repo has a [Jenkinsfile](Jenkinsfile) with the build info, so you can automatically import this repo using the Blue Ocean UI
  - Please configure the Apigee crendetials in Jenkins. The current Jenkinsfile uses `apigee` as the ID. If you wish to change that, please update the Jenkinsfile with the provided ID under the `environment` section - `APIGEE_CREDS = credentials('apigee')`
![](./media/apigee-credentials.png)
- Alternatively, if using [Cloud Build](https://cloud.google.com/cloud-build) the repo contains [build configuration files](https://cloud.google.com/cloud-build/docs/configuring-builds/create-basic-configuration) that can be used
- If using the provided Cloud Build [configuration file](cloudbuild.yaml) you should provide the following variable subsitutions in your [build trigger](https://cloud.google.com/cloud-build/docs/automating-builds/create-manage-triggers) settings:
  - `_APIGEE_PROFILE` - value should correspond to one of the profiles in the POM e.g. `test`, `prod`
  - `_APIGEE_ORG` - Apigee org name
  - `_APIGEE_USER` - Apigee admin account username
  - `_APIGEE_PREFIX` - optional value to use as a deployment suffix
- Additionally you should [create a secret](https://cloud.google.com/secret-manager/docs/creating-and-accessing-secrets) using [Secret Manager](https://cloud.google.com/secret-manager) named `apigee-admin-pw` containing the password corresponding to the `_APIGEE_USER` account
- When developing locally run the following commands to package, test and deploy:
  1. `mvn test` - run linting, unit test and code coverage steps
  2. `mvn apigee-enterprise:configure apigee-enterprise:deploy -Ptest -Dapigeeorg={apigee org name} -Dusername={admin username} -Dpassword={admin password}` - package and deploy the API proxy bundle
  - Running both of these commands will deploy a bundle with the local user name as the deployment suffix.