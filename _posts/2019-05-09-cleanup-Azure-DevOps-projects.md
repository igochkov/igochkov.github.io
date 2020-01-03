---
layout: post
title:  "Azure DevOps - project cleanup"
comments: false
---

Do you have to cleanup Repos and Pipelines in Azure DevOps (former TFS) after an old legacy project? The chances are that you will face the same challenge as I did, namely:

> How to preserve the Azure pipeline templates after deletion for future reference or restore?

### The problem

The Azure Repos have a nice nifty feature. In both Git and Team foundation version control (TFVC) when you delete source control item it actually just hides it from you. You still have the option to restore/undelete the item when needed. We call this feature "soft delete" or "logical delete". Next to your repository, you probably have Azure pipeline for building and releasing of your project as well. To my surprise, logical delete is not supported there. When you delete build and release templates they are gone. Forever.

In my opinion the Azure Pipelines and Azure Repos are interconnected and have to be preserved/versioned together. But how can we archive the old project without loosing any of those dependences? 

### The solution

It is obvious that we want to logically delete the build and release templates as well, if possible even in the same change set. To work around the lack of logical delete, we can use the **Azure Pipelines import/export feature**.


#### Procedure to archive a project
1.	Export the build and release definition templates as json files
2.	Commit/check-in the json files in the project repository related to the templates
3.	Delete the build and release templates from Azure Pipelines - *it is permanent, physical delete*
4.	Delete the project repository - *that is our logical delete*

#### Procedure to restore a project
1.	Restore/undelete project repository
2.	Import the build and release definition templates from json files in Azure Pipelines