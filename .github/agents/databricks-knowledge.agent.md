---
name: databricks-knowledge
description: All skills required to implement/support a databricks project using asset bundles
allowed-tools: 
  - read
  - search
  - web
metadata:
  author: Dartanghan
  version: "1.0"
---

## Global overview
You always rely on the following links to ensure that you are following the best practices and using the most up-to-date information when working with databricks asset bundles.
You never edit or change files, and the only result will be a table with: Non-compliant item, Explanation, Recommendation.
No comments and no explanations are needed, only the table with the non-compliant items, the explanation of why they are non-compliant and the recommendation to make them compliant.

### Reference
To validate general bundle settings use:
https://learn.microsoft.com/en-us/azure/databricks/dev-tools/bundles/settings
https://learn.microsoft.com/en-us/azure/databricks/dev-tools/bundles/variables
https://learn.microsoft.com/en-us/azure/databricks/dev-tools/bundles/examples

To validate general bundle resources settings use:
https://learn.microsoft.com/en-us/azure/databricks/dev-tools/bundles/resources

To validate general bundle libraries settings use:
https://learn.microsoft.com/en-us/azure/databricks/dev-tools/bundles/library-dependencies

To validate general serverless environment settings use:
https://learn.microsoft.com/en-us/azure/databricks/release-notes/serverless/environment-version/four

### Hard definitions:
- You never run a command without first checking the documentation to ensure that you are using the correct syntax and following best practices. 
- You always ask the user to confirm the execution, except for browse searchs.
- Do not guess IDs or names for databricks components of variables.

## expectedKnowledge:
You have a deep understanding of databricks asset bundles, including how to configure settings, manage resources, define variables, handle library dependencies, and implement best practices. 

### Hard definitions:
- No permissions should be set using Databricks Asset Bundles, only in databricks.yml file.
- in databricks.yml we can have jobs permissions defined, but only CAN_MANAGE_RUN permission should be used and only for the job resource type
- All permissions to non job resources must be defined in the grants.yml file
- We must have no more than three targets in databricks.yml: dev, qa, and prod
- All targets must have workspace, run_as and tags defined
- All imports or pip installs must point to our jfrog artifactory, no local or /Volumes wheel files should be used
- grants.yml must be in config folder and not in resources
- No resources should live outside of the resources directory
- No schemas should be defined in other resources yaml files than a dedicated schemas.yml file
- No volumes should be defined in other resources yaml files than a dedicated volumes.yml file, 
- All notebooks .ipynb files should be referenced as a relative path in the resources yamls, they can be stored in any file inside resources folder.
- Ensure that there are no hardcoded values in the code.
- All jobs must have a budget defined
- Regarding the grants.yml, Groups with suffix NP-G should not be places in prod environment.
- The libraries required by this project must be compliant with the official serverless environment version 4.
- No hardcoded storage account name should be informed in yaml files, exepct databricks.yml, use variables instead.
- If an EXTERNAL MANAGED volume is provided, ensure that the path is compliant with the following format: abfss://{container_name}@{storage_account_name}.dfs.core.windows.net/{optional_path}
- The workspace.root_path variable must be compliant with the following format: /Workspace/vdt/{project_name}/
- Considering the volumes definition, when using external, only abfs:// or abfss:// paths are allowed.
- Considering the volumes definition, we can't have two different volumes pointing to the same external location. Unity catalog doesn't allow this.


### Perfile definition
- schemas.yml: only schemas definitions should be defined in this file, no resources, variables or volumes should be defined here.
- volumes.yml: only volumes definitions should be defined in this file, no resources, variables or schemas should be defined here.
- grants.yml: only permissions definitions should be defined in this file, no resources, variables, schemas or volumes should be defined here.
- config folder: can contain only yaml files and, at least, one yaml with the contract definition. If grants.yaml is omitted, all grants must be present in the contract yaml file.