## HCL DevOps Test Hub GitHub Action
HCL DevOps Test provides software testing tools across multiple domains: API testing, functional testing, UI testing, performance testing and service virtualization. It helps you automate and run tests earlier and more frequently to discover errors sooner - when they are less costly to fix.

This action enables the ability to run various tests from a HCL DevOps Test Hub project.

## Usage

## Prerequisites to run this action

Configure a self hosted runner on a suitable machine
1. The host machine will need to be able to access the DevOps Test Hub Server that will be used to run tests.
2. [Add a self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) at an appropriate level, ensure you have configured network access.
3. If you are using more than one runner [assign it a suitable label](https://docs.github.com/en/actions/hosting-your-own-runners/using-labels-with-self-hosted-runners) and note this for later.
**OR**
1. An instance of DevOps Test Hub Server that can be accessed from the GitHub's hosted runners

### Setup
In the repository you want to apply the action to
1. Create a folder named ".github/workflows" in the root.
2. Create a .yml file with any name, inside the ".github/workflows" folder based on the following example content:

```yaml
name: HCL DevOps Test Hub

on: workflow_dispatch

jobs:
    TestHub-Action:
        runs-on: self-hosted
        name: HCL DevOps Test Hub
        steps:
         - name: Execute Test
           uses: HCL-TECH-SOFTWARE/devops-test-hub-action@main
           with:
            serverUrl: <https://myserver>
            offlineToken: <xxxxx>
            teamspace: <MyTeam>
            project: <MyProject>
            branch: <main>
            assetId: <yyyyyyy>
```

3. Update the parameterized items to refer to your server, project and tests (see parameter details below).
4. Depending on connectivity, 
  - If you have more than one hosted runner configured, then use its label as the argument for **runs-on**.
  - If the DevOps Test Hub Server is accessible from GitHub then use **runs-on: ubuntu-latest** 
5. Push your updated yml file to the repository.
6. Go to the Actions section in the repository and select the workflow.
7. Click the Run workflow dropdown and the list of input boxes get displayed.

### Required Parameters

- **serverUrl** URL of the HCL DevOps Test Hub Server where the tests are located. URL should be of the format - https://hostname
- **offlineToken** The offline user token for the corresponding HCL DevOps Test Hub Server
- **teamspace** Team Space when the project is based.
- **project** Project name where the test is.
- **branch** Name of the Git branch where the test is.
- **assetId** AssetId of the test file in HCL DevOps Test Hub.

### Optional Parameters

- **environment** Test environment corresponding to the test. Mandatory to input the value if you want to run API tests.
- **datasets** Semicolon (;) delimited list of source:replacement datasets for the job to run. For example, dataset1:dataset2;dataset3:dataset4
- **labels** Labels to add to test results when the test run is complete. You can add multiple labels to a test result separated by a comma. For example, label1, label2.
- **secretsCollection** Secrets collection name if the test uses them.
- **variables** Variables corresponding to the test. The format is name_of_the_variable=value_of_the_variable. You can add multiple variables to the test run separated by a semicolon.

## Troubleshooting
- **No runner available:** If you are using a self-hosted runnter, check that the runner still exists in GitHub, if the local agent has not connected to GitHub for a period of 14 days then it will be automatically removed.