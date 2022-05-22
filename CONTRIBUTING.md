# Development of the adapter

Python 3.9 is used for developing the adapter. To get started, setup your environment as follows:

Create a virtual environment, pyenv is used in the example:

```shell
pyenv install 3.9.12
pyenv virtualenv 3.9.12 dbt-synapse
pyenv activate dbt-synapse
```

Install the development dependencies and pre-commit and get information about possible make commands:

```shell
make dev
make help
```

## Testing

### pytest-dbt-adapter

The package [pytest-dbt-adapter](https://github.com/dbt-labs/dbt-adapter-tests) is used for running tests against the adapter.
However, this is no longer the recommended way to test adapters and we should into replacing this with [the recommended way to test new adapter](https://docs.getdbt.com/docs/contributing/testing-a-new-adapter)

### Tox

Running the unit tests:

```shell
make test
```

## CI/CD

We use the Docker image from our [dbt-sqlserver](https://github.com/dbt-msft/dbt-sqlserver/blob/master/CONTRIBUTING.md) repo.

There is a Circle CI workflow with jobs that run the following tasks:

* Run the unit tests
* Use the adapter to connect to a SQL Server Docker container
* Run the pytest-dbt-adapter specs against a SQL Server Docker container
* Use the adapter to connect to an Azure SQL Database with various options

### Azure integration tests

The following environment variables are available:

* `DBT_SYNAPSE_SERVER`: Name of the Synapse workspace
* `DBT_SYNAPSE_DB`: Name of the Synapse dedicated SQL pool
* `DBT_AZURE_TENANT`: Azure tenant ID
* `DBT_AZURE_SUBSCRIPTION_ID`: Azure subscription ID
* `DBT_AZURE_RESOURCE_GROUP_NAME`: Azure resource group name
* `DBT_AZURE_SP_NAME`: Client/application ID of the service principal used to connect to Azure AD
* `DBT_AZURE_SP_SECRET`: Password of the service principal used to connect to Azure AD
* `DBT_AZURE_USERNAME`: Username of the user to connect to Azure AD
* `DBT_AZURE_PASSWORD`: Password of the user to connect to Azure AD

## Releasing a new version

Make sure the version number is bumped in `__version__.py`. Then, create a git tag named `v<version>` and push it to GitHub.
A CircleCI workflow will be triggered to build the package and push it to PyPI. 
