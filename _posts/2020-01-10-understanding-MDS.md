---
layout: post
title:  "Understanding Master Data Services"
comments: false
---

> Master Data Services (MDS) is Microsoft's platform for supporting Master Data Management (MDM). 

Recently, I have been reached with a question about MDS deployment strategy. The team was straggling to bring their MDS model changes to the production environment. After revision of their current process, I was surprised to discover that MDS model changes were "treated" as software changes and deployed with Azure DevOps (former TFS) release pipeline. Let’s step back for a second and understand what the objectives of MDM in general are and how it is used.

Master Data Management is all about a single point of reference or having single trusted "version of the truth". MDM's objective is to provide consistency and quality of the referenced data. The management of the MDM data is responsibility of the business. IT departments can provide support during the creation and maintenance of the data models but not the data itself. 

That means that data changes will always be applied in the production environment. If you choose to push model changes as regular software through release pipeline, eventually they will be in conflict with the data. To avoid surprises, here are my do's and don'ts when working with MDS.

### Do’s
- The business is owner of the MDS data, IT of the MDS model.
- Production MDS environment is leading and seen as the source for model and data.
- Any other environments (Dev, Test, etc.) when needed, are clones of the production environment.
- Changes to the model and data are applied only in production environment.
- Use MDS build-in Transactions feature to track history of data changes.
- Use MDS build-in Versions feature when model changes.
- Backup the MDS instance as normal SQL database.

### Don’ts
- Do not use Azure DevOps repository to store model and data packages.
- Do not use Azure DevOps release pipeline for deployment of packages.
  
In conclusion the MDS model and data changes do not fit into de standard software development pattern. Trying to deploy them as standard software will result in conflicts and confusion.

### Links to Microsoft Docs
- [Master Data Services - Overview][1]{:target="_blank"}.
- [Master Data Services – The Basics][2]{:target="_blank"} 
- [Master Data Services - Transactions][3]{:target="_blank"}.
- [Master Data Services - Versions ][4]{:target="_blank"}.

<!-- Links -->
[1]: https://docs.microsoft.com/en-us/sql/master-data-services/master-data-services-overview-mds
[2]: https://www.red-gate.com/simple-talk/sql/database-delivery/master-data-services-basics/
[3]: https://docs.microsoft.com/en-us/sql/master-data-services/transactions-master-data-services
[4]: https://docs.microsoft.com/en-us/sql/master-data-services/versions-master-data-services